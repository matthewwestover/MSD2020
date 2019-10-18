## Week 3 - Day 3
### Websockets
AJAX is nice, lets us use single page applications  
Only update the parts we care about  
Nothing special on server side, same as a normal HTTP request  
AJAX is limited:  

* Text based - possibly large headers, waste of space
* Limited to HTTP protocols (Get, Post)
    * These are limited as is. Get data and submit forms. Anything else is not using HTTP effectively
* All requests have to be done from the client which waits for a response
* To get constant updates have to do “polling” basically ask “is there anything new?” Constantly 

We need more open sources:

* Client and server both should be able to initiate sending data
* Persistent connection needed to make that work
* Have more than functionality beyond Get and Post.
* Data can be more freeform
* Reuse the HTTP processing to make simpler

This is where WebSockets come in. They are similar to java sockets  
Send more than just bytes like in java sockets. Can send actual messages or bytes  
To start, begin with a HTTP request  
“Graceful failure” - server doesn’t support websockets just returns a 404 error  
Stateful protocol - connection stays open until one of the end points close it  

WebSocket API  
Similar to AJAX  
```let ws = new WebSocket(url);```  
URL is not a normal HTTP:// address.   
ws:// + location.host (ws://localhost:8080, or ws://IPADDRESS)  
Can have suffixes. ws://localhost:8080/chat  

Use listeners for reacting to this.
 
* onopen    
* onclose
* onerror
* onmessage    

To send a message ws.send(message) // usually do strings, things like json  
When done with socket can call ws.close() // don’t always need to do this, when your browser closes a page websockets are closed.  
Can have a web socket that only sends from client, with no response from server  
Can be cheaper than AJAX as connection is persistent and context is remembered, data needed in HTTP header only needs to be sent once when connection opens and not at every request from the client.  
This is mainly useful for things like chat clients, online gaming  

Calculator server app uses looking at the url query string. calculate?x=3&y=4 sent to server, server returns 7.  
Common method for small bits of data.  
Also add use a web socket as Number “space” number to get answer back from server.  
Server is just addition  

### Midterm Review
HTML and CSS - basic syntax and concepts  
What Tags, attributes, ids, selectors are  
Network Layers  

* Application
* Transport - connect program to program, adds “port” to the ip to specify what program
* Network - connect machine to machine, network card to network card
* Link
* Physical

HTTP - application layer  
TCP - transport layer  
IP - network layer  
Java uses sockets for networking on transport layer

Java and OOP  
Interfaces  and Inheritance  
Method Overriding  
Abstract classes  
How does GUI programming use these things - JavaFX  
Basic JavaFX classes - layout classes (multiple types of panes), shapes, all are nodes  
JavaFX uses inheritance to get these node classes to work  
Eventlistener is the big JavaFX interface - lets us write our own classes to react  
Exceptions - try, catch, throw, throws  
Throws is in declaration meaning a method MIGHT create an exception  
Throw is when an exception actually happens  
Reflection - ability to look at code at runtime and use it to write new code at runtime  
This is how JUNIT works - inspects code to test.  
Class object is the main reflection object. Contains all methods, fields, etc.  Class object lets us inspect any object we create.  
All classes inherit from object class. Object class includes tostring() and equals()

Javascript  
Dynamic typing (variables can be whatever they need to be) Can get variables that are not what we expect because of it, but less restrictive  
DOM Elements  
Callback and listeners  
AJAX and WebSockets  