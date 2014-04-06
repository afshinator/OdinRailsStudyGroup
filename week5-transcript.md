## Rails Study Group Week 5 Transcript
![alt text](./img/rrs.png "our logo")

- [The Odin Project](http://www.theodinproject.com)

- [Study Group page on Odin](http://www.theodinproject.com/studygroup)

- [Week 5 Hangout Video]()

---

Four gives you more, but five is hella live!  **Welcome to Week 5: Routing**

- Last week we did an overview of how a combo of HTTP verbs plus the url map to an 
action in our Rails app's controller.  Routing is about *how* we go from request to 
controller action.


- **The routing system does two things:** It maps requests to controller action methods, 
and it enables the dynamic generation of URLs for you for use as arguments to methods like
link_to and redirect_to.

- I thought the [Routing Intro section on Odin](http://www.theodinproject.com/ruby-on-rails/routing)
 was an easily digestable, good read - 
I'm not just saying that!

- I had a hard time reading and learning the routing information from the RailsGuides!

- Luckily basic routing is pretty... basic!  The concepts are pretty easy to get, the
syntax and the details we'll have to do over-and-over for it to get natural.  Until then,
this is yet another one of those things where we'll be looking up and copying and pasting
code. 

In this session I want to keep touching on the basics (RESTful routing), but also hit on 
parts that we'll encounter as we journey outside the world of tutorial apps, and into 
the "real world".

Let's quickly look at routing in the context of the oh-so-prevalent [Devise authentication gem](https://github.com/plataformatec/devise).  
Then we'll parse the routes file from the Odin project - which is not boring!

---

### Routing in Devise

- ```devise_for``` -  Generates all needed routes for itself in routes.rb 
based on what modules you have defined in your model;  
[example:](http://rubydoc.info/github/plataformatec/devise/master/ActionDispatch/Routing/Mapper)

Let's say you have a good ol' User model, and in your ```routes.rb``` you have ```devise_for :users```; 
it'll look inside your model and create the needed routes:

``` ruby
# Session routes for Authenticatable (default)
     new_user_session GET    /users/sign_in                    {controller:"devise/sessions", action:"new"}
         user_session POST   /users/sign_in                    {controller:"devise/sessions", action:"create"}
 destroy_user_session DELETE /users/sign_out                   {controller:"devise/sessions", action:"destroy"}

# Password routes for Recoverable, if User model has :recoverable configured
    new_user_password GET    /users/password/new(.:format)     {controller:"devise/passwords", action:"new"}
   edit_user_password GET    /users/password/edit(.:format)    {controller:"devise/passwords", action:"edit"}
        user_password PUT    /users/password(.:format)         {controller:"devise/passwords", action:"update"}
                      POST   /users/password(.:format)         {controller:"devise/passwords", action:"create"}

# Confirmation routes for Confirmable, if User model has :confirmable configured
new_user_confirmation GET    /users/confirmation/new(.:format) {controller:"devise/confirmations", action:"new"}
    user_confirmation GET    /users/confirmation(.:format)     {controller:"devise/confirmations", action:"show"}
                      POST   /users/confirmation(.:format)     {controller:"devise/confirmations", action:"create"}
```


- **[Odin's ```routes.rb```](https://github.com/TheOdinProject/theodinproject/blob/master/config/routes.rb)**

  Remember, order matters in the routes.rb file, lets go from top down:

``` ruby
devise_for :users, :controllers => { :registrations => "registrations" }  # lines 12-18 in picture below
  devise_scope :user do
    get '/login' => 'devise/sessions#new'								# line 19 to line 4
    get '/logout' => 'devise/sessions#destroy', :method => :delete 		# line 20 to line 6
    get 'sign_up' => 'devise/registrations#new'							# line 21
    get 'signup' => 'devise/registrations#new'							# line 22
  end
```

- What is this: ```:users, :controllers => { :registrations => "registrations" }``` ?

  It's telling the router that for the user resource, use the registrations controller 

- How about: ```devise_scope :user do``` ?

  Also know as ```as```, if you have custom routes, its required in order to specify which controller is targeted : 

```
as :user do                 # notice it is singular
   get "/some/route" => "some_devise_controller"
end

devise_for :users            # notice its now pluralized
```

- Notice ['devise_scope' and 'as'](http://rubydoc.info/github/plataformatec/devise/master/ActionDispatch/Routing/Mapper:devise_scope) 
  use the singular form of the noun where other devise route commands expect the plural form.


![alt text](./img/OdinRoutes1.jpg "odin1")

---

This next set is easier to decode:

``` ruby
  root :to => 'static_pages#home'					    # line 23 in picture below
  get 'home' => 'static_pages#home'					  # line 24;  /home also goes to root of site
  get 'scheduler' => redirect('/courses')			# line 25;  notice this generates a 301
  post 'thank_you' => 'static_pages#send_feedback'	
  post 'suggestion' => 'static_pages#suggestion'	
  get 'students' => 'users#index'					    # line 28; users controller
  get 'about' => "static_pages#about"         # line 29-36 : the static_pages controller
  get 'faq' => "static_pages#faq"
  get 'contact' => "static_pages#contact"			# ...
  get 'contributing' => "static_pages#contributing"
  get 'studygroup' => "static_pages#studygroup"
  get 'legal' => "static_pages#legal"
  get 'cla' => "static_pages#cla"
  get 'tou' => "static_pages#tou"
  get 'press' => redirect('https://docs.google.c   	# line 37; redirects (301) to page outside domain

```

![alt text](./img/OdinRoutes2.jpg "odin2")

---

Then, the good ol', resourceful one call to seven actions: ```resources: cal_events```; 
***notice PUT & PATCH** are both routed to update action...:

```
#               cal_events GET    /cal_events(.:format)                cal_events#index
#                          POST   /cal_events(.:format)                cal_events#create
#            new_cal_event GET    /cal_events/new(.:format)            cal_events#new
#           edit_cal_event GET    /cal_events/:id/edit(.:format)       cal_events#edit
#                cal_event GET    /cal_events/:id(.:format)            cal_events#show
#                          PATCH  /cal_events/:id(.:format)            cal_events#update
#                          PUT    /cal_events/:id(.:format)            cal_events#update
#                          DELETE /cal_events/:id(.:format)            cal_events#destroy
```

- Not all browsers support PATCH; rails uses POST as the method in good old ```form_for``` in the view,
but keeps track that it was a PATCH.

---

Then,

```
  resources :users, :only => [:show, :index, :edit, :update] do
    resource :contact, :only => [:new, :create]
  end
```  

  Notice there is a nested resource.  This block maps to:

```
#             user_contact POST   /users/:user_id/contact(.:format)      contacts#create
#         new_user_contact GET    /users/:user_id/contact/new(.:format)  contacts#new

#                    users GET    /users(.:format)                       users#index
#                edit_user GET    /users/:id/edit(.:format)              users#edit
#                     user GET    /users/:id(.:format)                   users#show
#                          PATCH  /users/:id(.:format)                   users#update
```

---


Then, two resources with :only specifications:

```
  resources :splash_emails, :only => [:create]

  resource :forum, :only => [:show]
```

  Mapping to:

```
#            splash_emails POST   /splash_emails(.:format)               splash_emails#create
#                    forum GET    /forum(.:format)                       forums#show
```

---


Then, these are for checking off lessons completed, note the the HTTP verbs:

```
  post 'lesson_completions' => 'lesson_completions#create'
  delete 'lesson_completions/:lesson_id' => 'lesson_completions#destroy', :as => "lesson_completion"
```

  Mapping to:

```
#       lesson_completions POST   /lesson_completions(.:format)            lesson_completions#create
#       lesson_completion DELETE /lesson_completions/:lesson_id(.:format)  lesson_completions#destroy
```  

---


Then

```
  get "sitemap" => "sitemap#index", :defaults => { :format => "xml" }
```

  Mapping to:

```
#                  sitemap GET    /sitemap(.:format)                       sitemap#index {:format=>"xml"}
```  

---

Then, the courses and lessons routes 

``` ruby
  get 'curriculum' => redirect('/courses')
  get 'courses' => 'courses#index'

  # Explicitly redirect deprecated routes (301)                  # Pretty slick, eh?
  get 'courses/:course_name' => redirect('/%{course_name}')
  get 'courses/:course_name/lessons' => redirect('/%{course_name}')
  get 'courses/:course_name/lessons/:lesson_name' => redirect('/%{course_name}/%{lesson_name}')

  # Match all undefined routes as courses and/or lessons
  get ':course_name' => 'lessons#index', :as => "course"         # :as provides course_path helper
  get ':course_name' => 'lessons#index', :as => "lessons"		 # :as provides lessons_path
  get ':course_name/:lesson_name' => 'lessons#show', :as => "lesson"
```

  Mapping to:

```
#               curriculum GET    /curriculum(.:format)                                redirect(301, /courses)
#                  courses GET    /courses(.:format)                                   courses#index
#                          GET    /courses/:course_name(.:format)                      redirect(301, /%{course_name})
#                          GET    /courses/:course_name/lessons(.:format)              redirect(301, /%{course_name})
#                          GET    /courses/:course_name/lessons/:lesson_name(.:format) redirect(301, /%{course_name}/%{lesson_name})
#                   course GET    /:course_name(.:format)                              lessons#index
#                  lessons GET    /:course_name(.:format)                              lessons#index
#                   lesson GET    /:course_name/:lesson_name(.:format)                 lessons#show

``` 

  In the above, ```:as``` lets you specify a name for your route;  
  so the app now can use course_path, course_url, lessons_path, lessons_url in controllers, helpers and views.

![alt text](./img/study3.jpg "odin2")

---


### Knick - Knacks

**List of routes** - Did you know you can go to ```server/rails/info/routes``` in dev environment?

- Just like going to a bad route gives you a listing, except no error message.



**Testing Routes** - Rails offers three built-in assertions

-```assert_generates```

-```assert_recognizes```

-```assert_routing```

- or you can use [RSpec](http://rubydoc.info/gems/rspec-rails/frames)'s 
```routes_to``` matcher, 
[SO example](http://stackoverflow.com/questions/9221953/how-to-test-a-nested-resources-controller-with-rspec): 

``` ruby
it "routes to the 'show' action" do
  { get: "/foos/123/bars/456" }.
    should route_to controller: "bars",      # like assert_recognizes
      action: "show",
      foo_id: "123",
      id: "456"
end

```

- another [RSPEC](https://www.relishapp.com/rspec/rspec-rails/docs/routing-specs) example


### [CodeSchool-Rails Web APIs with Test](https://www.codeschool.com/courses/surviving-apis-with-rails)

Will teach you plenty about routing, testing them, and more...; here is a sampling:

- Starts off talking about ```only:``` and ```except:```

- **Constraints** - Enforces subdomain

```resources :thingies, constraints:{ subdomain: 'api'}   # http://api.foo.com/thingies```

- **Namespaces** - Mapping a URI pattern with a subdir to a controller in its own subdir

``` ruby
namesapce :api do
  resources :thingies, only: :index
end

# maps to 
# Prefx        Verb    URI Pattern                Controller#Action
  api_thingies GET     /api/thingies(.:format)    api/thingies#index {:subdomain=>"api"}

 ```

---

** Path Segmented Expansion ** - Just means Arguments in the URI are separated using a slash.

```
/thingies
/thingies/:id
/thingies/:id/majigies
/thingies/:id/majigies/:id
```

Most URI's will not depend on **query string parameters**, 
- So a route like ```thingies?id=1``` will route to ```Thingies#index```, and not ```Thingies#show```

But it's ok to use them in certain cases:

- **filters** - ```/thingies?food=grain```  ; sent to the index action where filter code is run

- **searches** - ```/thingies?keyword=green```

- **pagination** - ```/thingies?page=3&per_page=30```


---

And while we're here,

**match** in ```routes.rb```

- ```match '/items/:id/purchase', to: 'items#purchase'``` matches *any* HTTP verb

  Fails in Rails 4 cuz it opens it up to **Cross-site Scripting (XSS) attack**:

  ```<a href="http://yourapp.com/items/4/purchase">Click to win!</a>```

- Must specify HTTP method :

  ```post '/items/:id/purchase', to: 'items#purchase'```  or

  ```match '/items/:id/purchse', to: 'items#purchse', via: :post```,  or via: :all if need be


**Nested Resources and Concerns** - to keep code looking DRY

``` ruby
resources :messages do
  resouces :comments
  resouces : categories
  resouces : tags
end

resources :posts do
  # sames sub resources as above
end

resources :items do
  # same sub resources as above again
end

# Use concerns to turn above into below, which read better:

concern :sociable do
  resources :comments
  resources :categories
  resources :tags
end

resources :messages, concerns: :sociable
resources :posts, concerns: :sociable
resources :items, concerns: :sociable
```

- could also pass options into concern blocks (to limit actions)

- You can even wrap your 3 resources lines with the concerns into its own object, 
and **put it in its own file**!  





### Testing Your routes & controllers

- We'll encounter a bunch as we go through Hartl

- Again, take a look at those videos because they cover a range of good stuff

-





**ASCIIcasts screencasts** on routing (in Rails 3) - haven't had time to explore them

- http://asciicasts.com/episodes/203-routing-in-rails-3

- http://asciicasts.com/episodes/231-routing-walkthrough

- http://asciicasts.com/episodes/232-routing-walkthrough-part-2