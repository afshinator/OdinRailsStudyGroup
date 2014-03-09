### Rails Study Group Week 1 Script

-[The Odin Project](theodinproject.com)

-[Study Group page on Odin](http://www.theodinproject.com/studygroup)

-[Week 1 Google Hangout](https://plus.google.com/u/0/events/cot10jfo8isvp486c9vkut2t33s?authkey=CNvcqOHw37W61AE)

-TODO: link to recorded session

---

#### Objectives

+ Main goal of this group is to provide value above and beyond what you'd get if you were to go through the material purely on your own.  

	Let's leverage each other for teaming up, answering questions, and venting!

	Use the Google Odin Community page to get in touch with other Odinators!

	###### If you're into it, send me your email, I'll make a mail list so you can reach each other during the week.

+ We are NOT going over the reading & projects in the hangout as if I'm teaching it.  You're going to have to do all the reading on your own.  However, I encourage x amount of group coding on the projects especially if you're stuck.

+ We ARE 

	going to meet weekly, go over last weeks efforts, revise schedule if need be.

	going to use my experience / hear my opinions around learning Rails, Ruby, & CS in general - hopefully to your benefit.

	**Advice #1** : Hartl is of the opinion that you can learn Rails w/o much understanding of Ruby.  My observation is that without knowledge & experience of 1. Ruby, 2. Programming in general, you'll at best become good at building what the tutorials build, and will lack the know-how to implement your own original ideas.

	**Advice #2** : While following the tutorials, embrace the side projects like getting up to speed in github, getting on Heroku & deploying often.  ( Lack of Github skills will continuously hinder you.   Heroku deployment is simple but can also be a land-mine (asset issues!). )


#### Paraphrasing the [How This Course Will Work](http://www.theodinproject.com/courses/ruby-on-rails/lessons/how-this-course-will-work) section

+ This portion of the Odin curric will be the most build-heavy
+ You'll be asked to read docs, check out blog posts, watch videos, run marathons
+ Projects will be the major focus
+ Related/included technologies: HTTP, MVC, REST, APIs, Cookies and Authentication

+ **Prerequisites** - that section oulines installing, back-end secion of web dev 101, basic Ruby, Rails overview, and our first assignment!

	Software installation - **Necessary stuff**: Ruby, Rails, Git & Github account, eventually Heroku account

	Installation is non-trivial and time-consuming stuff.  [InstallFest](http://www.theodinproject.com/courses/web-development-101/lessons/installations) 

	**If for whatever reason, you can't install Rails**, you can use online IDE's, our weapon of choice at this time: [Nitrous.io](https://www.nitrous.io/)

	Even if your local dev environment is all set up, get a Nitrous account anyway, you'll use it when group coding with others in this track and throughout Odin project efforts.

#### The [Rails portion of the Web Dev 101](http://www.theodinproject.com/courses/web-development-101/lessons/ruby-on-rails-basics) section kicks off our Rails learning

+ A **quick video tour** of what has been done with Rails... We'll just watch from 0:50 to ~2:45 in the hangout
	[Video with Hartl showing popular sites that run on Rails](http://www.youtube.com/watch?v=b_DJdmvBStE)

+ **Points to ponder** - You'll learn this stuff in detail in the curric; let's do a once-over...

	**What is Rails?** - An open source web application framework that's buit using Ruby. It is a full-stack framework: it allows creating pages and applications that gather information from the web server, talk to or query the database, and render templates to the browser.

	**What language is Rails written in?** - Ruby.  But since it also interfaces with the database and serves web pages, we have SQL, HTML, CSS, Javascript, and related technologies in the mix.

	**What are Ruby gems?** - Basically a packaged ruby application or library; comes stamped with a name and a version.  You use them to extend or change the functionality in Ruby applications.

	Here is a typical gem structure:  [more info here](http://rubylearning.com/blog/2010/12/14/ruby-gems-%E2%80%94-what-why-and-how/)

<code>
gem/
|-- lib/
|   |-- gem.rb
|   |-- [and more]
|
|-- test/
|-- README
|-- Rakefile			# scripts to help, build, test, debug the gem
|-- gem.gemspec			# defines stuff like gem name, vers, platform, authors, email, repo page, summary, rubyforge_project name, other dependencies
</code>

	**What are the gems that make up Rails?**

	for [rails 4.0.3](https://rubygems.org/gems/rails), 
	*ActionMailer* - Allows you to send emails from your application.
	*ActionPack* -  the Controller and View layers are handled together by Action Pack. These two layers are bundled in a single package due to their heavy interdependence.
	*ActiveRecord* - Active Record is the M in MVC, the model; the layer of the system responsible for representing business data and logic.  The non-database functionality of Active Record is extracted out into ActiveModel (validations, ...).
	*ActiveSupport* - Responsible for providing Ruby language extensions, utilities, & more.
	*bunder* - manages app dependecies
	*Railties* - Rails internals: application bootup, plugins, generators, and rake tasks..
	*sprockets-rails* - Sprockets concatenates and serves JavaScript, CoffeeScript, CSS, LESS, Sass, and SCSS


	**What is the purpose of the gemfile?**

	To define the gems you application will use, their versions and what environments they'll be used in (dev, test, depolyment)

	**What is the command to create a new Rails app from the command line?**

	rails new appname	