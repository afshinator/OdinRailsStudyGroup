## Ruby on Rails Points to Ponder 

#### Week 2 - Rails Study Group


-[Week 2 transcript](https://github.com/afshinator/OdinRailsStudyGroup/blob/master/week2-transcript.md)

-[Week 2 Hangout Video - coming soon]()


---

### Why Rails?

- There's a variety of web technology frameworks out there.  How many of you even considered learning .Net or
    Spring, or Laravel?   What?? you never even heard of them?

- Here's some [good discussion around learning **Django vs. Rails**](http://www.quora.com/Ruby-vs-Python/Which-should-I-learn-Django-or-Rails)


// Show screen shot of [Node homepage](http://nodejs.org/)
- Let's talk about **Node.js**, its javascript on the server (instead of in a browser), plus
 
    Express.js is a web application server that runs on Node.  It has yet to take over the world... however,

	*Node.js* shines in real-time web applications employing push technology over websockets. Why is that interesting? For over 20 years on the web we've had this paradigm of the stateless request-response architecture (REST) based on the client initiating requests to the server as the model for how things should work; Node web applications can have real-time, two-way connections, where either the client or server can initiate communication, allowing them to exchange data at will. Additionally, it’s all based on the open web stack (HTML, CSS and JS)... [read more here](http://www.toptal.com/nodejs/why-the-hell-would-i-use-node-js)

	Using non-blocking, and event-driven I/O, it remains lightweight and **efficient in the face of data-intensive real-time applications** that run across distributed devices.  
    
    It's definitely a rising star among technologies, another reason being that javascript itself continues to rise.

// Show screen shot of [Rails homepage](http://rubyonrails.org/)

- **Rails** lets you build and change web apps quickly.  Its open source, well-documented, has a thriving & hip
	community, runs on Ruby (which is known as a programmer friendly language), lets you interface well with the front-end, often developed using the modern test-driven agile paradigm, and [more secure than most](http://youtu.be/2Ex8EEv-WPs). 

	It's a mature, stable product that has proven its value.  As opposed to node, **it shines in cases where the
	business logic aspects of the application are heaviest** *(examples: e-commerce, membership sites, content management, custom database solutions, ...)*


##### Very interesting: Check out [Technology Radar](http://www.thoughtworks.com/radar/#/) for an interesting angle on current tech trends.

---

### What are some well-known sites written in Rails (at least at some point)?

	shopify.com
	scribd.com
	twitter.com (was)
	hulu.com
	yp.com
	slideshare.com
	zendesk.com
	github.com
	ask.fm

---

<a name="http"></a>
### Take a look at [Week 1 Points to Ponder](https://github.com/afshinator/OdinRailsStudyGroup/blob/master/week1-pointsToPonder.md)

### Take a look at the [HTTP & REST review](https://github.com/afshinator/OdinRailsStudyGroup/blob/master/week2-HTTP.md)

// The rest of this is for advanced beginners

### Ruby-isms    

- [Hashes](http://richonrails.com/articles/working-with-ruby-hashes), symbols, and procs - get used to them, they're everywhere


- **Common idioms all over rails**:

	Colon placement can be confusing.  Must pay attention to placement. Here is an example from ```routes.rb``` :

	```resources :posts```

	- Whats is :posts?  Its a symbol.  We're passing a symbol to the resources method.

	Another example from routes.rb

	```root to: "welcome#index"```

	- Here, 'root' is a method being called with a hash with key 'to' and value "welcome#index" string.

	Now in a view : 

	```<%= form_for :post, url: posts_path do |f| %>```

	- form_for is a method being passed :post symbol, and a hash with key 'url',
	but first ```posts_path``` 'helper' method is being passed a block, which resolves to the value
	for key value 'url'.
	
	And another : 

	```render text: params[:post].inspect```

	- ```render``` is a method being passed a hash which has key 'text', the value of which is the result of running the ```inspect``` method on the params hash, whose key value :post is being accessed!  Got that?


