### Ruby on Rails Points to Ponder 

#### From Week 1 Rails Study Group

See Odin Project >> Web Development 101 >> Web Devevelopment Frameworks >> 2: Ruby on Rails Basics : [Ruby on Rails 101](http://www.theodinproject.com/courses/web-development-101/lessons/ruby-on-rails-basics), halfway down the page.


-[Week 1 Script](https://github.com/afshinator/OdinRailsStudyGroup/blob/master/week1-script.md)

-[Week 1 Google Hangout](https://plus.google.com/u/0/events/cot10jfo8isvp486c9vkut2t33s?authkey=CNvcqOHw37W61AE)

---

##### You'll learn this stuff in detail while going through the curriculum; let's do a once-over now so you're ahead of the game

**What is Rails?** - An open source web application framework that's buit using Ruby. It is a full-stack framework: it allows creating pages and applications that gather information from the web server, talk to or query the database, and render templates to the browser.

**What language is Rails written in?** - Ruby.  But since it also interfaces with the database and serves web pages, we have SQL, HTML, CSS, Javascript, and related technologies in the mix.

**What are Ruby gems?** - Basically a packaged ruby application or library; comes stamped with a name and a version.  You use them to extend or change the functionality in Ruby applications.

Here is a typical gem structure:  [more info here](http://rubylearning.com/blog/2010/12/14/ruby-gems-%E2%80%94-what-why-and-how/)


<code><br>
gem/<br>
|-- lib/<br>
|   |-- gem.rb<br>
|   |-- [and more]<br>
|<br>
|-- test/<br>
|-- README<br>
|-- Rakefile			            # scripts to help, build, test, debug the gem<br>
|-- gem.gemspec			       # defines stuff like gem name, vers, platform, authors, email, repo page, summary, rubyforge_project name, other dependencies<br>
</code>


**What are the gems that make up Rails?** - for [rails 4.0.3](https://rubygems.org/gems/rails),

	*ActionMailer* - Allows you to send emails from your application.

	*ActionPack* -  the Controller and View layers are handled together by Action Pack. These two layers are bundled in a single package due to their heavy interdependence.

	*ActiveRecord* - Active Record is the M in MVC, the model; the layer of the system responsible for representing business data and logic.  The non-database functionality of Active Record is extracted out into ActiveModel (validations, ...).

	*ActiveSupport* - Responsible for providing Ruby language extensions, utilities, & more.

	*Bunder* - manages app dependecies

	*Railties* - Rails internals: application bootup, plugins, generators, and rake tasks..

	*Sprockets-rails* - Sprockets concatenates and serves JavaScript, CoffeeScript, CSS, LESS, Sass, and SCSS


**What is the purpose of the gemfile?** - To define the gems you application will use, their versions and what environments they'll be used in (dev, test, deployment)


**What is the command to create a new Rails app from the command line?** - 	rails new appname

**How is a GET request different from a POST request?** 
	[ what is HTTP ?: a protocol for data communication on the web]

+ Both are HTTP request methods,

+ GET - Requests data from a specified resource, query strings (name/value pairs) is sent in the URL of a GET request

+ POST - Submits data to be processed to a specified resource, query strings (name/value pairs) is sent in the HTTP message body of a POST request		

+ GET requests can be cached, POST are never cached

+ GET requests stay in the browser history, POST do not remain in browser history

+ GET reqs can be bookmarked, POST reqs cannot

+ GET reqs shouldnt be used for sensitive data, have length restriction, should only be used to retrieve data

+ POST reqs have no restrictions on data length

	###### Other HTTP Request Methods:

	-HEAD - same as GET but returns only headers & status line, no document body

	-PUT - edit a current resource

	-DELETE - deletes a specified resources

	-OPTIONS - returns the HTTP methods that the server supports

	-CONNECT - Converts the request connection to a transparent TCP/IP tunnel, Usually is it used for SSL connections (HTTPS), though it can be used with HTTP too

	-TRACE - for testing, echo back what was sent

+ **What is CRUD?** - Your application consists of resources - A resource  is the term used for a collection of similar objects, such as articles, people or animals. You can create, read, update and destroy items for a resource and these operations are referred to as CRUD operations

+ **What is REST?** - REpresentation State Transfer; an architecture style for designing networked applications. 

	RESTful applications use HTTP requests to post data (create and/or update), read data (e.g., make queries), and delete data. Thus, REST uses HTTP for all four CRUD (Create/Read/Update/Delete) operations.  REST is simple and easy to implement.  

+ **What is RESTful Routing?** - Instead of relying exclusively on the URL to indicate what webpage you want to go to, it's a combination of URL + a VERB. So, he same URL used with a different verb (either GET, PUT, POST, DELETE) will get you to a different page. This makes for clean, short URLs, and is particularly adapted to CRUD applications.

+ **What is MVC?** - An app architecture / software pattern that divides an app into 3 interconnected parts; its aim is to separate internal represenation of info from ways that that data is presented to user.

	**Model** - Represents app data, biz rules, logic, functions

	**View** - A view can be any output representation of information, such as a chart or a diagram

	**Controller** - Accepts input and converts it to commands for the model or view

	**MVC in the context of a RAILS App...**

	*Model* - Maintains the relationship between Object and Database and handles validation, association, transactions, and more.

	*View* - Outputs the HTML, triggered by a controller's decision to present the data. 

	*Controller* - The part/objects of a rails app that directs traffic; queries the models for specific data, and organizes that data (searching, sorting, massaging it) into a form that fits the needs of a given view.
