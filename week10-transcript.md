## Rails Study Group Week 10 Transcript
![alt text](./img/rrs.png "our logo")

- [The Odin Project](http://www.theodinproject.com)

- [Study Group page on Odin](http://www.theodinproject.com/studygroup)

- [Week 10 Hangout Video](https://www.youtube.com/watch?v=K0TNUXOj0y0)

---

Let's go over this week's projects... **[Projects: Active Record Basics](http://www.theodinproject.com/ruby-on-rails/building-with-active-record)**

### the Warmup

- Any questions/comments on these four warmup questions?


### This weeks 2nd project: **Micro-reddit**


The associations I came up with...

	Users
		- has_many :posts
		- has_many :comments
	Posts
		- belongs_to :user
		- has_many :comments

	Comments
		- belongs_to :user
		- belongs_to :post

Pretty straightforward.

Let's code-along as we explore...

### One-to-One relationships

**["Best applied when you have an object that has more than one role in the system."](http://tutorials.jumpstartlab.com/topics/models/relationships.html)** 
- *What does that mean?* --> eg) a User table with a whole bunch of details, some have to do 
with auth, some with address, ... these columns serve "more than one role..." in our system.

Let's use and expand our User table from [the Micro-Reddit project](https://github.com/afshinator/micro-reddit) as an example.


#### First Let's take a side trip and document our model.

- Add ['annotate' gem](https://github.com/ctran/annotate_models) to gemfile

```ruby
group :development do
  gem 'annotate', :git => 'git://github.com/ctran/annotate_models.git'
end
```

- bundle install

- ```rails g annotate:install``` to generate rake file in ```lib/tasks``` that'll execute whenever you run ```rake db:migrate``` in dev mode.
	A whole bunch of config options in there; 
	just to get fancy we'll flip exclude_tests, exclude_fixtures, and exclude_factories to true.

- Run ```rake db:migrate``` just for annotation sake, look at the top of the model files now for the commented summary of schema.  

Right now our User model just has name and email fields; **what if we want to add the following for each user?**
- birthday
- gender
- city


**Natural tendency is to add them to User table.**  This makes sense because each instance of the User class / each row of the Users table will have one associated birthday, gender, and city fields.  There are a few reasons you might want use an extra table though:

- One I can think of :
	
	What if each city is associated with a bunch of other data that needs its own columns... like maybe branch office location in that city, how long you've been in that city, ... 

- Another from [Jumpstart](http://tutorials.jumpstartlab.com/topics/models/relationships.html): 

	Wherever you do a ```User.find(3)```, rails gets *all* the columns (fields) corresponding to
	user with id 3 from the User table.  Especially if you have a lot of columns in that table, it 
	makes it ineffecient to do that extra work; especially if for example, all you need 
	is ```@user.name``` for your view.

Let's explore their solution.

#### Put details in a new table, create a 1-to-1 relationship between User and their details

A new 'Detail' class and associated 'details' table with the less-frequented info: birthday, gender, city, ... and finally ```user_id``` as foreign key.

```rails g model detail birthday:string gender:string city:string user:references```

Add model relationships:

- in user.rb: ```has_one :detail```

- detail.rb will already contain: ```belongs_to :user```


Let's add some sample data in the ```rails c```onsole; or better yet, seed the database
by adding the following to ```db/seeds.rb``` and run ```rake db:seed```  :

```ruby
user_list = [
  [ "Johnny Appleseed", "jaseed@ibm.com", "3/4/89", "male", "New Mexico", 1 ],
  [ "Lou Ferigno", "lfer@ups.com", "5/14/79", "male", "Portland", 2 ],
  [ "Sammy Davis", "sammy@jr.com", "8/11/82", "male", "Chicago", 3 ],
  [ "Polly Wannacrakca", "pwanna@crak.net", "4/3/69", "female", "New York", 4 ],
]

user_list.each do |name, email, dob, gender, city, user|
  User.create( name: name, email: email )
  Detail.create( birthday: dob, gender: gender, city: city, user_id:user )
end
```

#### Queries -- We can do things like 

- ```User.find(3)```,

- To get to the details, can we do ```User.find(2).detail.birthday``` for a specific detail, or

- ```User.find(2).detail``` for all details associated with a user; 

- We can also do ```Detail.first.user```, and so on.


#### Now, what if we want a new detail created for every new User created?

- We can do that with [```after-initialize```](http://edgeguides.rubyonrails.org/active_record_callbacks.html#after-initialize-and-after-find) - "Callbacks are methods that get called at certain moments of an object's life cycle. With callbacks it is possible to write code that will run whenever an Active Record object is **created, saved, updated, deleted, validated, or loaded** from the database.  The after_initialize callback will be called whenever an Active Record object is instantiated, either by directly using new or when a record is loaded from the database. ."

- Let's add an ```after-initialize``` callback to to our User model...

```ruby
class User < ActiveRecord::Base
  # ...
  has_one :detail

  after_initialize do
    self.build_detail if detail.nil?
  end
end
```


Now in the console, lets create a user **without** details and see what happens:

```User.create(name:"Bruce Wayne", email:"thebat@man.com")```

```
   (0.1ms)  begin transaction                                                                                                                                              
  User Exists (0.2ms)  SELECT 1 AS one FROM "users" WHERE "users"."email" = 'thebat@man.com' LIMIT 1                                                                       
  SQL (1.8ms)  INSERT INTO "users" ("created_at", "email", "name", "updated_at") VALUES (?, ?, ?, ?)  [["created_at", Sun, 11 May 2014 23:04:45 UTC +00:00], ["email", "the
bat@man.com"], ["name", "Bruce Wayne"], ["updated_at", Sun, 11 May 2014 23:04:45 UTC +00:00]]                                                                              
  SQL (0.3ms)  INSERT INTO "details" ("created_at", "updated_at", "user_id") VALUES (?, ?, ?)  [["created_at", Sun, 11 May 2014 23:04:45 UTC +00:00], ["updated_at", Sun, 1
1 May 2014 23:04:45 UTC +00:00], ["user_id", 5]]                                                                                                                           
   (85.7ms)  commit transaction                                                                                                                                            
 => #<User id: 5, name: "Bruce Wayne", email: "thebat@man.com", created_at: "2014-05-11 23:04:45", updated_at: "2014-05-11 23:04:45"> 
```

- We didn't specify the values for the details fields!... so they're nil.  Hmmm, more on this later.

**Reaching through the User object to get to the details object is not good practice.**

- From the outside, we shouldn't even know about the Detail object - its an implementation detail.

- Instead, User should present an interface to the Detail object.

- The "Law of Demeter" is a series of guidelines to follow in Obj Oriented programming which leads to loose coupling, encapsulation, and more... ;  all desirable traits.  Reaching through User to get to Details violates this law.  More on this [here](http://devblog.avdi.org/2011/07/05/demeter-its-not-just-a-good-idea-its-the-law/).

To hide the details of Detail object (pun intended!), we can use Rails' ```delegate``` method:

```ruby
class User < ActiveRecord::Base
  # ...
  has_one :detail, dependent: :destroy

  delegate :birthday, :gender, :city, to: :detail

  after_initialize do
    self.build_detail if detail.nil?
  end
end
```

Now, a call to ```User.find(3).city``` will proxy the call to the associated Detail object, 
fetch it from the database if it hasn’t already been loaded, 
and return us the value returned from the Detail object’s method. 

