## Rails Study Group Week 7 Transcript
![alt text](./img/rrs.png "our logo")

- [The Odin Project](http://www.theodinproject.com)

- [Study Group page on Odin](http://www.theodinproject.com/studygroup)

- [Week 7 Hangout Video](https://www.youtube.com/watch?v=b6zrBYHTxTI)

---

Six made it stick, but seven is from heaven.  **Welcome to week 7: Views (and more testing)**

Last week we tried to do some testing, didn't get far.  This week also I wanted to spend more time on testing but I didn't get far enough to have much to present.  So this week we'll start off on some interesting material on Application servers, then talk a little about issues with views that we don't get from the tutorials.

---

### Ruby/Rails Application Servers

- **What's that!??** - 
Here's a short & sweet explanation (from [this source](https://devcenter.heroku.com/articles/ruby-default-web-server) ):

  "The Ruby standard library comes with a default web server named WEBrick. As this library is installed on every machine that has Ruby, most frameworks such as Rails use WEBrick as a default development web server.  While WEBrick should be fine for development, it was not designed to handle a high concurrent workload that a Ruby app must serve in production. A production web server should be used instead."

  Furthermore, "By default WEBrick is single threaded, single process. This means that if two requests come in at the same time, the second must wait for the first to finish."


"If you continue to run WEBrick [in production,] it is likely that requests will take a long time, possibly timeout, and you will need to use many more dynos [on Heroku] than your application requires.  Rather than doing this, **ensure you use a production web server**.  A production Ruby web server is capable of handling multiple request concurrently."

  [Heroku Recommends Unicorn](https://devcenter.heroku.com/articles/ruby-default-web-server).    You might have noticed its alredy there in a default gemfile, commented out:

  ```
# Use unicorn as the app server
# gem 'unicorn'
  ```

**That's the quick and dirty of it,**  let's try to understand the issues a little further...

**Background jobs and threads** -  a relevant aside to app servers.

  Your web server is a piece of software that takes processor time.  It's main job is to address the HTTP request/response cycle, i.e.  process the number of incoming requests for web pages and serve them in a timely manner.

In the 'traditional' means of deploying a Rails app, an individual Rails process can only handle one request at a time. From when the request comes in until the response is delivered, all the app can do is deal with that one thing. If other requests come in during this time, they need to be kept in line waiting, and then they are handled one at a time, "serially".

This is an inefficient use of CPU resources, and especially bad when some of that 'blocked' time is spent waiting on external I/O, as is typical in a web app.  It can also lead to very uneven response times, as some requests end up spending a lot of waiting in line to be processed, and others don't.

Lets say for example your app wants to send emails.  This can slow response time because you've busied the machine doing something other than processing page requests, something that is also I/O intensive.

There are two basic solutions that involve a combo of hardware & software:

- The most common is the **multi-process concurrency model**.  Each Rails process can still only handle one request at a time, but you run multiple processes (aka, multiple workers). Perhaps as many workers as you have CPU cores, or even more if you can afford the RAM.

- Another way is **multi-threaded concurrency**. Allow an individual Rails process to actually handle multiple requests concurrently, **each in a separate thread**. This has recently started to receive more attention in the Rails universe.   Historically Rails was not thread-safe, but Ruby *does* allow you to easily spawn threads.

    Here is [a good article on Running Rails jobs in the background](https://www.agileplannerapp.com/blog/building-agile-planner/rails-background-jobs-in-threads), and once you finished that, read [this](http://tenderlovemaking.com/2012/06/18/removing-config-threadsafe.html) for more on threads in Rails.
    
- Yet another is **a hybrid** model where you have multiple worker processes, each of which dispatches multi-threaded processes.

Each of these has their pluses and minuses.  "While Heroku is currently recommending a multi-process model with Unicorn, ... if your app can be run safely under multi-threaded request dispatch, a multi-threaded model would actually provide better throughput.." - this is a quote from [this excellent article and github app](https://github.com/jrochkind/fake_work_app) that does some rocket science to benchmarks this stuff!


So, some app servers are better at dealing with multi-processes and multi-threading than others, which do you choose?  Generally, you'll want to look at: their stated and proven use cases, configuration options, and of course perfomance.  Reconcile these with your application needs.

**Here's four popular Rails App Server options**:

- **Passenger** app server

  Widespread use, easy configuration, good all around, best at multiple apps on same hardware

- **Unicorn** app server

  Multi-process, fast client/request execution; less "wtf" than Pasenger.  

- **Thin** app server

  Similar to Unicorn; evented I/O, well suited to long-running requests, live streaming, websockets, etc... due to evented architecture.

- **Puma** app server

  Multi-threaded: concurrent request processing, runs a request in a new thread; anything that can benefit from 
  truly concurrent execution would make Puma a good choice.  Benchmarks results: looks like hybrid/clustered/multi-worker puma is likely to provide significant advantage for an io-bound app, as most web apps are.

**Conclusion** - There's not really any reason I know of to use the default WEBrick, even in development.  Check out [**Jumpstart ContactManager**, one of the lesser known Rails tutorials](http://tutorials.jumpstartlab.com/projects/contact_manager.html) out there; it uses **Unicorn**.  Heroku recommends Unicorn on their website;  Brian mentioned some Heroku folks talking a lot about Puma at the recent Ruby conference.  The best decision will depend on the needs of your app and in a successful app those needs will change in time.  

Here is some references to help you get deeper knowledge on this stuff:

- [Slideshare on comparing app servers; general info](http://www.slideshare.net/jaustinhughey/app-server-arena-a-comparison-of-ruby-application-servers) - Good overview.

- [Benchmark app on github, and results](https://github.com/jrochkind/fake_work_app) - Awesome stuff, chuck full of info.

- [Another comparison from Digital Ocean](https://www.digitalocean.com/community/articles/a-comparison-of-rack-web-servers-for-ruby-web-applications)

- [Threading in Rails](http://tenderlovemaking.com/2012/06/18/removing-config-threadsafe.html)

- [Engineyard Passenger, Unicorn, or Puma? article](https://www.engineyard.com/articles/rails-server)

- [Puma on Heroku](http://blog.codeship.io/2013/10/16/unleash-the-puma-on-heroku.html)

---

### Views

Passing data from the controller to the view

- The common style is to use **instance variables** ( a.k.a. *ivars* )

  To use an ivar in your view, call it the same way as you would in the
  controller; so inside the view : ```<%= @user.first_name %>```, etc...

- Ivars are available in partials as well as layouts, but it's common to instead explicitly pass an 
  ivar to a partial from a view as a local var;  So for example, in your app/views/some_controller/show.html.erb:

  ```<%= render "fancy_title", title: @item.title %>```, 

  and in app/views/some_controller/_fancy_title.html.erb:

  ```<h1 class="fancy"><%= title %></h1>```

  *Syntax reference* [RailsGuides' section 3.4.6 on Local Variables within a partial](http://guides.rubyonrails.org/layouts_and_rendering.html)


  The advantage of passing the ivar to the partial as a local-to-the-partial variable: the partial is not tied to any specific ivar.  This makes the partials easier to re-use and move around.


- You can extend this idea to action views in general; instead of relying on the presence of an ivar from the controller in your view, make an explicit call to ```render```, passing values that will be used...  So in your thing_controller.rb:

```ruby
def show
  render locals: {
    item: Item.find(params[:id])    # can be refactored further
  }
end
```

Note: **Where did the instance variable go?**  In this example, we see that the controller doesn't have a need for it anymore.  It took care of delivering the goods from the database to the view without it.

So, why do this with action views and not just partials?

- Instance variables can be set all over the place. For example, before filters in the current controller, or before filters in a parent controller. If you don’t pass them explicitly to each view,  it's harder to know where the value came from.

- If you forget to set the ivar in the controller, it would be ```nil``` in the view without you immediately knowing why.  On the other hand, using the method above, if you forget to pass the info, you get an error right away.

More on this issue [here](http://thepugautomatic.com/2013/05/locals/) and [here](http://therealadam.com/2014/02/09/a-tale-of-two-rails-views/), and [this slide](http://www.slideshare.net/elia.schito/rails-oo-views).

This isn't *necessarily* the best way to do things.  As an OOP purist, you might call sharing the ivar across the conceptual boundaries of the Controller and the View a violation of encapsulation.   From what I understand, if your app is big/complicated enough to warrant this adherance to strict protocol for info exchange between object, then you do it.   Then again, some people would say anything but our simple tutorial apps are going to be complicated enough to warrant this.   In other words I don't think this is a given best practice; you'll have to judge whether it provides value for your particular app.

**Which brings us to several View related gems that address this and other issues ...** -

- [**Draper Gem**](https://github.com/drapergem/draper) - Does a good job of explaining the problem with putting logic in the view; another standard practice thats frowned upon.   Of course, their solution is to use their gem and many agree.

Here's a quick looksy at the problem.   Aside from the if-then logic we're used to seeing in views and partials, gems like **Can-Can** for authorization can litter your views with stuff like:

```
<% if can? :update, @article %>
  <%= link_to "Edit", edit_article_path(@article) %>
<% end %>
```

So the idea is to take that logic out of the presentation layer.

Draper gem uses the decorator pattern to address the problem.  Here is an [article on the Decorater pattern](http://dev.af83.com/2013/06/18/cleanup-rails-s-views-with-decorators.html), and [another](http://blog.codelette.com/2014/02/16/cleaning-up-rails-view-logic-using-draper/).

- [**Deface Gem**](https://github.com/spree/deface) - Lets you customize Erb templates without needing to directly edit the underlying view file. Deface allows you to use standard CSS3 style selectors to target any element (including Ruby blocks), and perform an action against all the matching elements.

  Example [here](http://guides.spreecommerce.com/developer/view.html).

- [**Cells gem**](https://github.com/apotonick/cells) - Provides reusable view components. Syntax looks a lot like controllers. 

  Example [here](http://nerdblog.pl/2013/09/03/using-cells-in-rails/), and [here](http://nicksda.apotomo.de/2010/10/10-points-how-cells-improves-your-rails-architecture/).

---

###Testing

**Negative assertions in tests** can be problematic!

A positive assertion example like ```page.should have_content("Hello BooBoo")``` passes as long as the content is there, and fails otherwise.  This is straightforward.

But negative assertions like ```page.should_not have_selector(".post")``` can pass; and that may only be because you forget to update your test after you renamed your ```.post``` class to ```.article``` !  **A false positive**.

What to do?

- In the above example, given that you have a condition that asserts the opposite where ```.post``` should exist on the page, make them both reference a method for the definition of what it is being tested.  Now, forgetting to rename in the tests will cause both to fail.  Example:

```ruby
describe 'Home page'do
  describe "page in some condition A" do
    before { # some precondition in which widget will exist }

    it "sometimes has a widget" do
      page.should have_widget         # we define have_widget method at the bottom
    end

  end

  describe "page in some condition B" do
    before { # some condition in which widget wont be on the page }

    it "sometimes doesn't have a widget" do
      page.should_not have_widget
    end
  end

  ...

  # Tests above refer to this definition for what widget refers to
  def have_widget
    have_selector(".post")
  end

end
```

Could do the same thing with RSpec's ```let``` statement.

More on this [here](http://thepugautomatic.com/2013/04/negative-assertions/).  Should you do it this way all the time??  I doubt it.  I think more than anything this is about being clear when you can get false positives from tests that make negative assertions.

---

###More Testing of the search feature on Odin

What to test:

- Acceptance tests - treating the app (feature in this case) as a black box

  Does the search box exist on the pages we expect it to?

  Does searching for a simple term bring up the results box?

  Does clicking on a result take us to the results?


- Edge cases:

  no input (whitespace)

  multi words


- Unit Tests - tests small parts of the system such as classes or methods

  hmm?

  Incoming Query to the search function in general when we fill out the search box.  Rule is to assert a known output for known input.

Maybe we can go through this whole feature with the tests by next week!

---

For next week:

- Finish off [The Asset Pipeline](http://www.theodinproject.com/ruby-on-rails/the-asset-pipeline), 
do the [Project: Basic Routes, Views and Controllers](http://www.theodinproject.com/ruby-on-rails/basic-routes-views-and-controllers)

  Project modification: for the second Project, instead of implementing a test app with Bootstrap, I want you to integrate [Zurb's Foundation](http://foundation.zurb.com/), or some other front-end 'framework':

  [uikit](http://www.getuikit.com/)

  [Gumby](http://gumbyframework.com/) dammit!

  [ivory](http://weice.in/ivory/)

  [kube](http://imperavi.com/kube/)

  [HTML Kickstart](http://www.99lime.com/elements/)


  *If you're looking for something to blog on, or you want to make a contribution to the community in way of a new gem or something, integrating and documenting one of the above less-well-known frameworks into Rails would be a good way to stand out among the crowd.*




  [Foundation-Rails gem](https://github.com/zurb/foundation-rails)

- See if you can finish off chpts 9 & 10 in Hartl.


