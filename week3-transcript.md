### Rails Study Group Week 3 Transcript

- [The Odin Project](http://www.theodinproject.com)

- [Study Group page on Odin](http://www.theodinproject.com/studygroup)

- [Week 3 Hangout Video](https://www.youtube.com/watch?v=8as2mxkUu7c)

---
### Welcome to week 3!

- If you're new, you'll want to look at [transcripts from week 1 & 2](https://github.com/afshinator/OdinRailsStudyGroup), and the [Odin project rails curriculum](http://www.theodinproject.com/courses/ruby-on-rails/lessons).

- In weeks 1 & 2 we set up our environments for Ruby & Rails, and dove right in to some smaller tutorials (Jumpstart Lab's Blogger2, and the RailsGuides').  We also spent a little time on HTTP, REST, and other misc. knick-knacks including a little journey into RegExps 101.  As far as the Odin Rails curriculum goes, we covered Step 1 and the first project.

- We continue this week traversing and fleshing out our circular journey around all the technology involved in making a web app with Ruby and Rails.    I like the metaphor of the concentric circles of knowledge - each trip around the circle we hit the material yet again but with more experience and support from previous journeys.


From last weeks assignment for beginners...

**[Chris Pine's Learn To Program](https://pine.fm/LearnToProgram/)**

- Once again: Why are we even concerned with this exercise?  Because Rails is a bunch of Ruby code.  Without Ruby knowledge you can't really develop misc Rails apps.

- Site was down most of the week.  Bummer. 

- Go and listen to [Wed Dev Study Group week 6](https://plus.google.com/u/0/events/cl5d2lc8p5cfp1v3ftt04njo10s) hangout, as they are also covering it.

- If time allows, it'd be nice to approach the exercises test-first, writing specs for each one before writing the implementation.

- You'll get more Ruby this coming week in [Chpt 4 of Hartl](http://ruby.railstutorial.org/chapters/rails-flavored-ruby#top).

### Let's get right into Testing related stuff!

- I'm going to tackle this material from all the angles I can; TDD / BDD is my Achilles heel in Rails development and I hear the same from many others.  

- Lets get our heads around the concepts, methodology, using RSpep & Capybara;  We're **not going to explore** Cucumber or TestUnit, unless someone really wants to.

#### First, the high level overview

- Depending on what source you reference, you'll get different definitions for Integration, Functional, and Acceptance tests.  The definition of a Unit test is pretty consistent across the board. 

  Check out this succint yet detailed 
  [overview of testing type definitions](http://stackoverflow.com/questions/4904096/whats-the-difference-between-unit-functional-acceptance-and-integration-test),
  not tied to the Rails world.  The [Test Pyramid](http://martinfowler.com/bliki/TestPyramid.html)
  is also a useful concept.

  Here's some terms to know (go with it for now!):

- **Unit Tests** - Tests the smallest unit of functionality, typically a method/function. Unit tests should be focused on testing one particular piece of functionality.  They're low level, and usually fast.   In the Rails universe, that usually means testing **models**.  Unit tests should catch edge cases and confirm correct object behavior.  More details to come in a minute.

- **Acceptance Tests** - High level (typically user-level) tests.  They run a lot more code than unit tests, and sometimes depend on external services; so they're generally slow.   Some people use this term to cover both Integration and Functional tests.

- **Integration Tests** - Integration tests build on unit tests by combining the units of code and testing that the resulting combination functions correctly.  Tests the interactions b/w different controllers and app workflow; end to end (but not necessarily testing a whole app feature?).

- **Functional Tests** - Functional tests check a particular feature for correctness. Functional tests don't concern themselves with intermediate results or side-effects, just the end goal.  In Rails context, functional tests refer to testing controller & view by, for example, looking for key HTML elements.

- **Feature Tests** - Some combo of the last 3... related to a specific app feature you want to exercise.

<a id="rspec-cmd-line"></a>
##### *RSpec advice #1 : *

#### Call RSpec on the command-line with --format documentation - [ref](http://blog.carbonfive.com/2010/10/21/rspec-best-practices/)

- Uses your tests syntax to show output; helps when refactoring the test so that the language is clear.

- Example:

```
describe "temperature conversion functions" do
  describe "#ftoc" do
    it "converts freezing temperature" do
      ftoc(32).should == 0                         
    end
  ...
```    
*will output:*

```
temperature conversion functions
    #ftoc
    converts freezing temperature
    converts boiling temperature  
```

##### *RSpec advice #2 : *

- Put often used RSpec commands into your ```.rspec``` file;  (use ```ls -a``` to see all hidden files)

```
--color
--format documentation
```
---
#### RSpec common testing pattern
+ ```describe``` blocks
+ ```context``` blocks
+ ```it``` blocks

*Example:*
```
describe "Object" do
  context "when created" do
    it "has an attribute X" do
      expect(something).to equal(z)  # assert something
    end
    it "has an attribute Y" do
      expect(something).to ... 
    end
  end
end
```

---

##### *Newest [RSpec syntax](https://relishapp.com/rspec/rspec-rails/docs): *

#### Use ['expect' over 'should'](https://www.relishapp.com/rspec/rspec-expectations/docs/syntax-configuration)
- A lot of blog posts and resources are using the older 'should' syntax.  For example the above is from Test-First Ruby; here it is re-written:

```
describe "temperature conversion functions" do
  describe "#ftoc" do
    it "converts freezing temperature" do
      expect(ftoc(32)).to eq(0)           # notice change in 'should' and '== 0'  from above
  end
  ...
```

- betterspecs.org brings up this point.  Funny thing is betterspec.org article uses 'should' in other parts of the article, and so do many especially older posts.  I'm not sure if I'm missing some information about where 'should' is more appropriate, or at least acceptable.

---

 **[Test First Ruby](http://testfirst.org/learn_ruby)** - the first 7 problems

- [My solutions](https://github.com/afshinator/playground/tree/master/TestFirstRubyExercises)

- In the middle of the week I asked that you re-wite the tests according to [best practices from betterspecs.org](http://betterspecs.org/); how far did folks get on that?



- Bottom of betterspecs.org article has some links to other good RSpec/Testing Best Practices articles:

  [Eggs on Bread](http://eggsonbread.com/2010/03/28/my-rspec-best-practices-and-tips/)
  
  [Carbon Emitter](http://blog.carbonfive.com/2010/10/21/rspec-best-practices/) - Has a step-by-step breakdown of how to approach your tests

  [Andy Vanasse](http://blog.andyvanasse.com/post/503615383/rspec-best-practices)

  [Bitfluxx](http://bitfluxx.com/2011/05/23/some-rspec-tips-and-best-practices.html)

  [Dmytro S.](http://kpumuk.info/ruby-on-rails/my-top-7-rspec-best-practices/)

  [thoughtbot](https://learn.thoughtbot.com/rspec)

  - **[How We Test Rails Applications](http://robots.thoughtbot.com/how-we-test-rails-applications)** Article
    
*Notes from that article...*

- "...distinguishing between RSpec and Capybara methods: Capybara methods are the ones that are actually interacting with the page, i.e. clicks, form interaction, or finding elements on the page."

- *Testing Convention:* Prefix class methods tests with a '.' and instance methods with a '#' 

- **Model Specs** - Think unit tests, tests small parts of the system such as classes or methods
    + Four phase test pattern for unit tests
        - setup
        - exercise
        - verify
        - teardown

```ruby
describe User, '.active' do                     
  it 'returns only active users' do
    # setup
    active_user = create(:user, active: true)
    non_active_user = create(:user, active: false)

    # exercise
    result = User.active

    # verify
    expect(result).to eq [active_user]

    # teardown is handled for you by RSpec
  end
end
```

- **Feature Specs** - High level tests written from perspective of user clicking around the app; combines RSpec & Capybara.

    + "...feature specs are slow to run. Instead of testing every possible path through your application with Capybara, leave testing edge cases up to your model, view, and controller specs."

- **Now for Controllers** ...

    + When testing multiple paths through a controller is necessary, favor using controller specs over feature specs; they are faster to run and often easier to write.
    
    + betterspecs.org :
      "When I first started testing my apps I was testing controllers, now I don't. Now I only create integration tests using RSpec and Capybara. Why? Because I truly believe that you should test what you see and because testing controllers is an extra step you don't need."

      [This guy disagrees](http://blog.codeship.io/2013/12/16/yes-you-should-write-controller-tests.html).

    + Plenty of discussion about it at https://github.com/andreareginato/betterspecs/issues/14 ; user 'svs' and 'marnen' have interesting points to make
    
- **View specs** - "A lot of developers forget about these tests and use feature specs instead... While you can cover each view conditional with a feature spec, [it would be slower]."

---
##### *Testing Related*

- What is **Factory Girl** ? - A gem that allows you to set up database records in a way to test against them in different scenarios; eg) create users for your test.  Prefer 'factories' over ```User.create``` / Rails fixture.  More on this in future weeks.

- To test **front-end JS**, install a JS driver, add 'metadata key' to test

```ruby
 feature 'User creates a foobar' do
   scenario 'they see the foobar on the page', js: true do
     ...
   end
 end
```

- **Database Cleaner** , why? "when we use a JavaScript driver, the test is run in another thread. This means it does not share a connection to the database and your test will have to commit the transactions in order for the running application to see the data..."; To make sure you start with a blank slate test database between test runs.  [Avdi has some good info](http://devblog.avdi.org/2012/08/31/configuring-database_cleaner-with-rails-rspec-capybara-and-selenium/) on it.

- **Test Doubles** - Simple objects that emulate another obj in your system; { see article example, will come up in coming weeks }

- **Test Stubs** - Add some functionality to the double ; { as above }

- **Test Spies** - RSpec Mocks; "...scenarios where you want to validate that an object receives a specific method. In order to follow Four Phase Test best practices, we use test spies so that our expectations fall into the verify stage of the test."  { as above }

- **[Webmock](https://github.com/bblimke/webmock)** - Instead of making third party requests, learn how to stub external services in tests.  This is a library for stubbing and setting expectations on HTTP requests in Ruby.  Again, its early in the game to get too deep into this but soon come!  Some [details here](http://robots.thoughtbot.com/how-to-stub-external-services-in-tests).

---

#### An approach to Unit Tests

- Generally,

  We can say, we want our tests to be thorough, stable, fast.  We want to prove that the single object under test is behaving correctly.  The fewer tests that provide adequate expression of your proof, the better.

  Test return values, side-effects, critical interactions; but not implementation details.

- We can say that for each 'System-Under-Test' / object, there are **3 origins for messages** associated with the object/system:

    - **Incoming** messages
    - **Outgoing** messages
    - Messages **to self**

- And Lets say there are **2 types of messages**, (and 1 combo type):
  
    - **Query** : *Returns something, changes nothing* (no side effects)
      eg) Get the city of user that has name 'Bob Loblaw; 

    - **Command** - *Returns nothing, changes something* -
      eg) writing to the database; change the name of the user

    - **Combination** - eg) popping off a stack; *returns something, changes something*

*When you approach testing your classes, think about which of these cases fit its methods.  Let's look at the different scenarios and some **general rules about how to test them**:*

- **Incoming, Query** - Rule: Make assertions about what they send back; assert a known output for known input.  Test the interface, not the implementation.
    
- **Incoming, Command** - Rule: Test incoming command message by making assertions about direct public side effects.
    
```    
# Here's a common scenario:  combined query / command : 

def set_x(new_x)
    @x = new_x        # returns x which is a query; sets @x with is a command
end

# Test that is you send a particular value to set_x, it returns that exact same value (tests the interface)
# Test set_x by calling it with a known number new_x, assert @x = new_x (which is asserting the side-effect)
```

- **Outgoing, Query** -  Rule: Do not test them! don't make assertions about their results, don't expect to send them.  One objects' outgoing is another objects' incoming... so don't redundantly check the results you get back.  Example of blog post object sending message to it's attached category object.  Dont write tests for blog post for sending message, write tests on category object for receiving them.

- **Outgoing, Command** - Rule: Just test your expectation that the message is sent, dont test the outcome; (break rule if side effects are stable and cheap).   { Use mocks. }

- **To Self** - Rule: Do not test private methods; essentially ignore them.  Do not make assertions about their result; do not expect to send the message to them.  eg) private functions  ( And break the rule when you need to. )

{ refs: [Video: Sandi Metz](http://www.youtube.com/watch?v=URSWYvyc42M), [Video: Katrina Owen](http://vimeo.com/68730418), [Book: Growing OO Software guided by Tests](http://www.growing-object-oriented-software.com/) }

*We'll apply these rules as we encounter testing in Hartl and see how it matches with what he's doing.*

### For the coming week...

- This is the last week in [the curriculum](http://www.theodinproject.com/courses/ruby-on-rails/lessons) 
with material not strictly related to Rails.

+ Read [Step 2: A Railsy Web Refresher](http://www.theodinproject.com/courses/ruby-on-rails/lessons/a-railsy-web-refresher) and more!
    
    - HTTP, REST, URLs, MVC, API's, Cookies, Sessions, Authentication, and Authorization - all on one page!
    
    - This should be at least your 2nd pass through the material.
    
+ Read [Step 3: Deployment](http://www.theodinproject.com/courses/ruby-on-rails/lessons/deployment)
    
    - This should be at least your 2nd pass, 1st pass was end of Chpt1 in Hartl.

+ Do [Project: Let's Get Building](http://www.theodinproject.com/courses/ruby-on-rails/lessons/let-s-get-building)

    - **Assignment: RESTClient** - Get familiar with making HTTP requests using your command line, which should prepare you for making them from within a Rails app later.  *Please track how long it takes you to complete this.*
    
    - **Assignment: Hartl's Chpts 3 & 4** - Static pages & Ruby overview
    
    - **Assignment: [Treehouse's Intro to RSpec](http://blog.teamtreehouse.com/an-introduction-to-rspec)**, 
    
    - **Assignment: Peruse the other articles referenced by betterspecs.org**  listed above; they are generally a little older and use maybe a little outdated syntax, but have good general info.  Don't try to memorize everything they say because if you haven't been writing a lot of tests, you won't be able to relate to their reasoning (or you might say, the writing isn't that consistently good) - so you'll get benefit from knowing they're there and coming back to them in the future.
    
    - Extra: Add [simplecov](https://github.com/colszowka/simplecov) and [annotate_models](https://github.com/ctran/annotate_models) gems to the Hartl app we're building and play, play, play!

  
