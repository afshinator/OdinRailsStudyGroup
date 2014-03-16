## Rails Study Group Week 2 Assignment Review

---

### [Rails Guides - Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html)

- [Finished product - official version](https://github.com/rails/docrails/tree/master/guides/code/getting_started)

- [my version with annotate gem](https://github.com/afshinator/RailsGuides-BlogTutorial)

- The [edge version](https://github.com/rails/rails/blob/master/guides/source/getting_started.md)
 of this tutorial is cleaned up quite a bit, follow that instead.

- I thought this was a really good overview for beginning Rails ; doesn't go to deep, but you're still making a full app.

- Keep this app in your github account so you can fork and tweak it at will. 

----

### [JumpStart Lab - Blogger2](http://tutorials.jumpstartlab.com/projects/blogger.html)

- [my version using Postgres](https://github.com/afshinator/blogJumpStart)

- I think of this tutorial as a more succint Hartl tutorial, with no testing, and some interesting gems.

- This got into it pretty thick: *multiple models with a variety of associations*, authentication, ...

- **It tells you to use ```bin/rails``` and ```bin/rake```**; this tutorial is the only one that I've seen address that...

  "The ```rails``` command is used for generating new projects, and the ```bin/rails``` command is used for controlling Rails."
  
  **My take on what that's about :**
  
  You can have multiple versions of Ruby and Rails installed. Running ```rails``` command outside of your created project dir will use your system's default version, use ```which rails``` to see that path.

    Once you've created your new project and are inside that dir, ```'bin/rails'``` will specifically run the script in the projects bin directory.  I believe this is so that not only can you have multiple versions of rails installed, you can also be sure the specified version for your project is executed when you're inside that projects directory.

  This is the only tutorial I've seen that addresses this; its not going to be an issue for noobs who at least this point won't be making project with Rails 3, etc...

- #### If you were not able to finish this project, I think it's sufficient for now that you finish at the RailsGuides'.  There is plenty of stuff we have to do and we'll be going over the material in this tutorial in depth further in the course.

----


### [annotate_models gem](https://github.com/ctran/annotate_models)
- **What does it do?** The same information you'd find in ```db/schema.rb``` but put as comments in your models
for convenience.   Also with ```annonate -r```, the same output as ```rake
routes``` but put as comments in your ```routes.rb```.
- Ok, it wasn't too deep!  But it's not something thats taking db space, and it 
doesn't exist in production, so... *pure wholesome fun!*


----

### [Setting up a Rails app for TDD](http://www.startuprocket.com/blog/how-to-setup-a-rails-app-for-test-driven-and-behavior-driven-development-with-rspec-and-capybara-webkit) article.

- What I wanted for you to get out of this article was a very quick intro to the some terminology and gems involved in testing; especially as Hartl explores it: RSpec, Capybara, Factory-girl.
- It uses Postgres and a bunch of other stuff that we aren't dealing with right now.  



