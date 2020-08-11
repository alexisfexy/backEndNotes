

# REST APIs Overview 

*LinkedIn Learning: Learing REST APIs*

General: allows you to separate presentation fo content from content itself

**The RESTful librarian:**

- think of a REST API like a library with books, a librarian, & people who want to borrow + add + update + remove those books 
- librarian = post
- request = GET 
- creates a representation of the data, then bundles it with a header containing metadata like resource ID, hyperlinks, etc. 
- PUT = wants to change something, so applied to content recieved in the original response - sent back as a put request using orginal ID 





Client makes request, returns it and request header 



**What is a REST API?** 

- REST: Representational State Transfer (data architecture)
  - recieves verbs & returns structures / research  (JSON or HTML )

Web site v. Web APPLICATION: 

- each page is a current state 
- when the visitor loads it, all the compents are downloaded 
- application sends a URI represeting the next state to be transferred 
- representational state is tranferred as a data opject 
- so you make single page applications on the web 



all the back adn forth is controlled by an API:
**API:** 

- application program interface
- set of features & rules to enable interaction between software & other items / hardware 
- for REST: collection of tools to work and access REST resources through UIs and GETS, PULLS, PUTS, etc 
  - it is the language the people use to talk to the "librarian"



**URL v. URI**

URI: Universal Resource Identifier

- Compact sequence of character to identify - 
- generic method for naming / locating web resource
- literally any sequence of characters that identify a resource are considreed a URI 
- eg:
  - https://google.com
  - mailto:sample@email.com

URL: Universal Resource Locator 

- SUBSET of the URI
- identifies resource AND how to access the resource
  - gives you the method like HTTP & FTP 
- all URLs are URIs!



URN: Universal Resource Name

- also a subset of the URI
- all uri under URN scheme and any other URI with property of a name
- name identifier like a name of  
- eg.
  -  URN: is a unqiue name of a person, like "Alexis Fexy" - anyone who knows me knows who you are referring even if you dont knwo where i am
  - URL: your actual physical location
- URN: 
  - explicit: like urn:oasis:name:speci.... 
  - or a resource name: linkedin.com/learning/instructors
- a URN can be an URL but doesnt have to be 

![Screen Shot 2020-08-10 at 6.35.09 PM](/Users/alexi474/Library/Application Support/typora-user-images/Screen Shot 2020-08-10 at 6.35.09 PM.png)





REST APIS: 

usually refer to everything as a URI since it is an umbrella term



**The 6 Constraints of REST**

- group of *constraints*



1. <u>Client Server Architecture:</u> 

   1. ensures proper separation of concerns 
      1. client manages user interface concers
      2. server: data storage concerns
      3. separation between content & its presentation + interaction

2. <u>Statelessness</u>

   1. No client context + information, AKA state, can be stored on the server between requests
      1. client is incharge of keeping track of session stats adn requests 
   2. Server can pass on storage stuff to a database (like a service token)

3. <u>Cacheabilty:</u> 

   1. def: storing responses for set of time
   2. must be marked at cacheable or noncacheable 

4. Layerd System:

   1. client cannot know if they are connected to server or intermediary (like a CDN or mirror)

   ![Screen Shot 2020-08-10 at 6.46.01 PM](/Users/alexi474/Library/Application Support/typora-user-images/Screen Shot 2020-08-10 at 6.46.01 PM.png)



5. <u>Code on Demand:</u>
   1. servers can transfer executable code like Js & compiled components to clients 
6. <u>Uniform Interface:</u> 
   1. Resource id in request
      1. URI will say what they are looking for
      2. eg. author/mortens/bio is requesting the bio as its resource 
   2. Resource manipulation thru represenations
      1. you could have it as sql table in the server 
      2. but return as JSON or HTML or something 
      3. Manipulation: 
         1. they can modify or delete the resource 
   3. Self-descriptive message
      1. for both sending & recieving data 
   4. Hypermedia as engine of application state 
      1. once the client has access to the rest resource it should be able to discover all available resources methods thru hyperlinks provided  



**REST & HTTP Relation**

- you often send your requests though HTTP 

- not linked, just a convient pariing

  

**REST<u>ful</u> API** 

- if you use REST with HTTP that is what makes it "FUL"
- when a REST service runs on the web over HTTP to give access to a web resource then it is a RESTful API 
  - the platform makes it FUL!!! 



Reqres.in for a sample REST API 

- RESTful API

postman or insomia are good e



VScode: REST Client



## Request



Methods + Resource URI 

Methods: 

- GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD



```python
GET https://site.com/wp-json/wp/v2/posts

GET https://site.com/wp-json/wp/v2/posts/ HTTP/1.1
Host: appsite.dev ##content type 
Content-Type:
application/json ..user agent string
Authorization: Basic dG9tOn
Cache-Control: no-cace


```



Send data TO the REST API 

- more complex because you need to send the actual dat a

  ```python
  POST /wp-json/v2/post HTTP 1.1 
  ## the structure of the data must match the content type defined in the request 
  # here the content type is set to application/json aka data nees to be marked as JSON too 
  Host: appsite.dev #content type 
  Content-Type: application/json #ser agent string to get access to the stuff  
  Authorization: Basic dG9tOn
  Cache-Control: no-cache
  
  {
  "title": "She creates new task",
  "content": "here is teh content"
  "author": 10,
  }
  
  
  ```

- in javascript the request woudl look like such: 

  - ```javascript
    var xhr = new XMLHttpRequest();
    xhr.open("GET", 
             "https://site.com", true);
    xhr.onload = function(){
      console.log(xhr.responseText);
    }
    xhr.send();
    ```



 **Discovery**

- figuring out what resources + methods you can get 

- OPTIONS request = gives you every available resource and method 



**Resource**

- "<u>Resource</u>": any info that can be named, is a resource

  - a resource is a conceptual mapping to set of entity not teh entity that corresponds to that mapping

  - aka TELL ME WHERE

  - ```
    Verb: 
    GET
    
    Resource:
    Bookcase:cubby:4:red
    
    Media-Type:
    Book
    ```



Resource can be categorized as: 

1. Collection: "all red books" 
   1. a colelction of items!!
2. Singleton: "Red book in cubby number 4"
   1. we also can narrow these collections to singletons "bookcase/books/red/4"

- <u>"Representation"</u>: you are not accessign teh actual data of the acess- you are getting COPY! 
  - eg. the data may be stored as xml but the representation served to a client could be JSON, etc.





**Methods / Verbs**

- Tell there server what you want 

1. GET: get what is at the end of the address and send it to me 
   1. success: 200 OK
   2. failure:  404 Not Found
2. POST:  create new resource + add it to a collection (e.g submit form on webpage, add a new product)
   1. Success: 201 Created
   2. Failure: 
      1. 401 Unauthorized
         1. you dont have authorization to do this
      2. 409 Conflict
         1. resource already exists 
      3. 404 Not Found
3. PUT: update data at an exisiting resource by <u>replacing</u> all of the content 
   1. contains the id of the resource & new content 
   2. Success: 200 OK
   3. Failure: 
      1. 204 No content present at the server (if sent to singleton)
      2. 401 Unauthorized
         1.  you dont have authorization to do this
      3. 404 NOt Found 
      4. 405 Method Not allowed (if sent to a collection, because put only can update a singleton resource )
4. PATCH: modify an exisiting resource (not replacing everything)
   1. same *statuses* as put! 
5. DELETE: only can be used for singleton 
   1. Success: 200
   2. Failure;
      1. 401: unauthorized
      2. 404: Not Found
6. OPTION: returns description of communication options for target resource
   1. Sucess: 200 OK 
7. HEAD: returns just head section of that response 
   1. success: 200 OK
   2. failure:  404 Not Found





## Response

response comes with a "head section" & whatever content is provided 



**Status Messages:**

- 1xx Information: status of the server, etc. 
  - rarely seen
- 2xx Success: 
  - 200 Okay
  - 201 Created
  - 204 No Content 
- 3xx Redirection: given new URI to follow to get to the resource
  - 301 Moved Permanently (use this uri for all future stuff)
  - 302/303 found this at other URL 
  - 307 Temporary redirection 
  - 308 resume incomplete  (permanent)
  - note: account for all these variants to make it clear!!
- 4xx Client Eror:
  - 400 Bad Request (malformed, too large, etc. )
  - 401 Unauthroized
  - 403 Forbidden (they might not be logged in)
  - 404 Not Found
  - 405 Method not allowed 
    - eg. using POST on a resource that can only recieve GET requests
  - and a TON other 4xx !! 
- 5xx Server Error: 
  - 500 internal server error 
  - 502 bad gateway
  - 503 service unavailable (server overloaded, or temp unavailable)



## Request / Response Pairs 

See 4th chapter to see examples 