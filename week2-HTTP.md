### HTTP & REST Review 

#### From Week 2 Rails Study Group


-[Week 2 transcript](https://github.com/afshinator/OdinRailsStudyGroup/blob/master/week2-transcript.md)

-[Week 2 Hangout Video](https://www.youtube.com/watch?v=6wg4DbSWzSs)

---

- Last week we quickly went over difference between GET & POST

- I didn't do a good job of reviewing the basics of HTTP, lets go into a little more detail




#### HTTP Basics

- **HTTP is a protocol** for data communication on the internet.

- Are REST and HTTP the same thing?

	No.	REST is a set of constraints that ensure a scalable, fault-tolerant and easily extendible system.  RESTful APIs almost always use HTTP as the transport layer, but they dont have to.

	RESTful applications like Rails use HTTP requests to post data (create and/or update), read data (e.g., make queries), and delete data. 

- HTTP is one of many protocols that runs over the internet, for example FTP (which is primarily used for file transfer) is another.  Unlike these other protocols, the purpose of HTTP is to view websites.  

- In HTTP, there are two different roles: **server and client**. In general, the client always initiates the conversation; the server replies.

- HTTP messages are composed of text, although the message body can also contain other media

	Being text based makes it easy to monitor:  Like using the browser dev tools Network tab.

- **HTTP messages are made of a header and a body.**; the body can be empty; otherwise it'll 
contain data you want to transmit

// See [Figs 10.1 & 10.2 from this page](http://trafficserver.readthedocs.org/en/latest/sdk/http-headers.en.html)
// for pictures of the HTTP Request Headers, and what is in them.

- The header contains metadata, such as encoding information, & the verb...

	example of request to theodinproject.com HTTP request header

GET / HTTP/1.1[CRLF]
Host: theodinproject.com[CRLF]
Connection: close[CRLF]
User-Agent: Web-sniffer/1.0.47 (+http://web-sniffer.net/)[CRLF]
Accept-Charset: ISO-8859-1,UTF-8;q=0.7,*;q=0.7[CRLF]
Cache-Control: no-cache[CRLF]
Accept-Language: de,en;q=0.7,en-us;q=0.3[CRLF]
Referer: http://web-sniffer.net/[CRLF]	

- you can view the whole conversation with **curl**

	// Example of curl -v theodinproject.com returning a status 301
	// curl some other sites.

- You can see the host is included in the header separately from the url path, right at top of the request header


### Putting REST in the picture


- Following RESTful practices, URLs should be as precise as needed; everything needed to uniquely identify a resource should be in the URL. You should not need to include data identifying the resource in the request. This way, URLs act as a complete map of all the data your application handles.

- So then how do you specify an action?  Given the same url, how do you distinguish between getting something and 
say, creating something?

#### HTTP Verbs tell the server what to do with the data identified by the URL

	// See picture [HTTP methods are enforced](https://peepcode.com/products/rest-for-rails-2) towards bottom of page 
	// showing mapping of verb & url mapping to different actions

- Each request specifies a certain verb/method in the request header
	-GET - used each time you click a link or type an URL in the address bar; tells the server to get data identified by the URL to the client.  The only one here that never results in modification of resources.

	-POST - Used in REST to create a new resource. The only action where multiple identical calls will change the data more than once, i.e. not idempotent.

	-PUT/PATCH - edit/update a current resource, 

	-DELETE - deletes a specified resources	

	{ GET, PUT, HEAD, & DELETE are idempotent - repeated calls with the same body and url will only result in just the first call.  (can't delete the same resource twice, ...)}

- There are other HTTP Verbs you wont often encounter 
	-HEAD - same as GET but returns only headers & status line, no document body

	-OPTIONS - returns the HTTP methods that the server supports

	-CONNECT - Converts the request connection to a transparent TCP/IP tunnel, Usually is it used for SSL connections (HTTPS), though it can be used with HTTP too

	-TRACE - for testing, echo back what was sent


- Can you use AJAX calls in a RESTful way?

	A trick question.  AJAX call is still just another request to a web server (usually used to avoid the page-refresh involved in getting a whole new page); so you choose to follow the RESTful protocol - or not.


#### Lets review points from Week 1 specifically on GET vs POST

- **How is a GET request different from a POST request?** 

+ Both are HTTP request methods, but

+ GET - Requests data from a specified resource; query strings (name/value pairs) is sent in the URL of a GET request

+ POST - Submits data to be processed to a specified resource; query strings (name/value pairs) sent in the HTTP message body of a POST request		

+ GET requests can be cached; POST are never cached

+ GET requests stay in the browser history; POST do not remain in browser history

+ GET reqs can be bookmarked; POST reqs cannot

+ GET reqs shouldnt be used for sensitive data, have length restriction, should only be used to retrieve data
	Internet Explorer enforces a maximum URL length of 2,083 characters.

+ POST reqs have no restrictions on data length


* [Read Thoughtbot on HTTP Request in Rails Apps](http://robots.thoughtbot.com/back-to-basics-http-requests)