## Rails Study Group Week 6 Transcript
![alt text](./img/rrs.png "our logo")

- [The Odin Project](http://www.theodinproject.com)

- [Study Group page on Odin](http://www.theodinproject.com/studygroup)

- [Week 6 Hangout Video](https://www.youtube.com/watch?v=-XI1KVtEsdY#t=14)

---

Five is hella live, but six will make it stick!  **Welcome to Week 6: Controllers, testing**

- Last week was mostly about Routing.  By today we were supposed to have read the 
main material on Controllers, and finished Hartl's chapter 5,6, & 7.   These Hartl chapters
dove right into the bulk of what we need to know about RSpec, Capybara, and Factory
Girl. Lots of good material.



### My workflow these days...

- Multiple console's open while developing:

  **Guard** gem is awesome; I dedicate a console window to guard.

  Another console dedicated to running the rails server & watching its output,

  ```tail -f log/development.log``` in one console,

  **Spring** gem to speed things up; comes in Rails 4.1, 
  I had some probs with integrating it with Hartls sample-app

  ( I had to gem install rubygems-update, then gem pristine --all )

---

### Some Highlights of Hartl's Chpts. 5-7

**Chapter 5** - Good chapter to see basic testing syntax and refactoring to be super-terse;  excellent demonstartion of ```before {}```, ```let```, ```subject {}```, and shared examples.  Good stuff for testing in the chapter exercises too.

- 5.3.4 : Goes from standard RSpec syntax to compact version, expect syntax 

``` ruby
  it "should have the content 'Sample App'" do
    expect(page).to have_content('Sample App')
  end
```

to 'should' : ```it "should have the content 'Sample App'" do``` ; but I wonder if you can do

```specify it."has content 'Sample App'" do``` ... or something like that?

but also it requires the addition of ```subject { page }``` the call to ```should``` automatically uses the page variable supplied by Capybara.

**Chapter 6** - 

- ( 6.1.1 ) SQLite DB Browser app - http://sourceforge.net/projects/sqlitebrowser/

- (same) ```rake db:migrate```  can be undone with ```rake db:rollback```, see also [Box 3.2](http://ruby.railstutorial.org/chapters/static-pages#sidebar-undoing_things)

- ( [6.1.3](http://ruby.railstutorial.org/chapters/modeling-users#top) ) 
Lots of good info about rails **console in sandbox mode**...

- ( [6.2.1](http://ruby.railstutorial.org/chapters/modeling-users#top) ) 
```rake test:prepare``` ensures that the data model from the development database is reflected in the test database. Run this Rake task after a migration 

    "Sometimes the test database gets corrupted and needs to be reset. If your test suite is mysteriously breaking, be sure to try running rake test:prepare to see if that fixes the problem."

- After making the simple User model, we wrote the first tests.  Here is some details  what is checked and when: 

```
# spec/models/user_spec.rb
require 'spec_helper'

describe User do
  before { @user = User.new(name: "Example User", email: "user@example.com") }
  subject { @user }

  it { should respond_to(:name) }
  it { should respond_to(:email) }
end
```

The before block tests the attributes when passed as a hash to new; so if you create a User object that doesn’t have (say) a name attribute, it will throw an exception in the before block.

The ```it``` parts ensure that the constructions user.name and user.email are valid.

Hartl remarks: "...testing for model attributes is a useful convention, as it allows us to see at a glance the methods the model should respond to".

- table 6.14 - the regex and explanation; ref to rubular.com

- Listing 6.20 - callback in the model

- Box 6.3 - Using let

**Chapter 7** - We start playing with **FactoryGirl**!

- [Listing 7.11](http://ruby.railstutorial.org/chapters/sign-up#sec-tests_with_factories) - Redefining the Bcrypt cost factor in test envt.

Issue: part of why running tests with FactoryGirl is slow is that the bcrypt algorithm creating a secure pw hash is slow by design; this next line speeds it up for us in the test environment:

```
  # Speed up tests by lowering bcrypt's cost function.
  ActiveModel::SecurePassword.min_cost = true  
```

- [Listing 7.29](http://ruby.railstutorial.org/chapters/sign-up#sec-deploying_to_production_with_ssl)
Configuring the application to use SSL in production.  


### [Rails Conf 2013 BDD and Acceptance Testing Video](https://www.youtube.com/watch?v=BG_DDUD4M9E)

with RSpec & Capyb,  by Brian Sam-Bodden; This video presents a definition and approach to BDD &
TDD in the context of Rails that I haven't encountered before in the main tutorials I've seen
(like Hartl).

- **BDD** is a refinement in the language and tooling used for TDD.  In BDD the focus is on behavior
specification.

- Typically BDD works from the outside in, that is starting with the parts of the software whose behavior is directly perceive by the user. 

    In Web applications, outside-in means starting with specs related to UI, or the outermost API level.

	What to test? Use Cases or User Stories, test what something does (behavior), rather than 
	what something is (structure).	

	What to ignore? "Anything else... Until proven that you can't"; in BDD there is a decoupling of the tests and the implementation (i.e.. don’t tests implementation specifics, test perceived behavior)

- **"Get the words right"** - He stresses re-factoring the tests so that the terminology that
RSpec pumps out is natural.
	
	RSpec combined with Capybara have somewhat flexible syntax to allow for this.

```
	describe "An instance of", Cart do
		context "when new" do
			it "contains no items" do           # could've used 'specify' here
				expect(@cart).to be_empty       # could've used 'should' here 
			end
		end
	end
```
	If you read the quoted parts from above, you know what the output is (using --format documentation)

	The resulting specs then become a form of documentation.

- ***TDD*** - is a design technique, and not *really* about testing.

   Red --> Green --> Refactor; you know this.

   We won't dwell too much on TDD stuff since it's basically Hartl's chpt 5-7 all over again...


- His whole approach can be summed up as this quote from Jeff Casimir:

*"A great testing strategy is to extensively cover the data layer with unit tests 
then skip all the way up to acceptance tests. This approach gives great code 
coverage and builds a test suite that can flex with a changing codebase."*

<<< picture of request-response-cycle >>>

In above picture, the 'outside' is the Views and the Models.  So **testing from the outside-in** means to write acceptance tests (coming in through the view) and writing unit tests (in through the model); and eventually writing integration tests (flexing the controller) when the feature set and the code
stabilizes.

In other words, start at the view, complement with the model *[many sources tell you to start with unit testing the model]*, leave the stuff in the middle (controller) to mature as the functionality of the app solidifies because these tests are very fragile, (cuz app functionality is in flux.)

<<< cartoon picture  sinks it in with regards to outer level going in with first failing acceptance tests( user button press , form fill, between app and user, + edge cases ) on top. 

Note that he 'does TDD' to some extent at controller layer because you're exploring/not quite sure what needs to be written. Also TDDing at the model level unit tests.


- **Acceptance Tests**

  Proper acceptance tests treat your app as a black box.

  They should know as little as possible about what happens under the hood.

  **[Capybara](http://learn.thoughtbot.com/test-driven-rails-resources/capybara.pdf)** is our weapon of choice.

  Here is [Thoughbot's RSpec Acceptance Tests Cheatsheet](https://learn.thoughtbot.com/test-driven-rails-resources/rspec_acceptance.pdf) too.

  To test JavaScript in your acceptance tests you can use the selenium-webdriveror capybara-webkitdriver.
