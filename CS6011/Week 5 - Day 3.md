## Week 5 - Day 3
### Review
Stuff from Midterm:  

```
ArrayList<Circle> Circles
ArrayList<Rect> Rects
For <var c: circles> c.drawCircle();
For<var r: rects> r.drawRect();
```

It would be good to unify these items, as both are just drawing the shape.   
We can use a inheritance from a Shape class, or implement a shape interface  
Shape would need to include a ```draw()``` method  
Code can become

```
ArrayList<Shape> Shapes;
For(var s : shapes) s.draw();
```

Extends is inheritance and implements is interfaces. Extends come first in class signature

```
Class Tupperware extends foodcontainer implements Microwaveable{
    Public Tupperware(Food f){ //foodcontainer has a food member var, we shouldn’t declare a second one here.
        this.f = f; // this doesn’t work, food containers is private, and we can’t set the FC private member var here
        Super(f); // this sets the food into the FC member var and how it should be done
    }
    Public void reheat(){ microwave(); }
    Public void microwave(){ }
}
```

```
FC fc = new Tupperware(food); //This is allowed, TW is a type of FC. 
Fc.microwave(); // FC can contain foil, microwavable is bad here. 
fc.reheat(); // one solution to fix
// Or change FC fc = new Tupperware() to TW tw = new Tupperware();
```

Classes cannot throw exceptions ```class fridge throws HungryException``` doesn’t work  
Only functions (methods and constructors)  
This is a declare or catch issue. Can only do one or the other. Try/Catch or state the method throws.  
```FC snack() throws HE```  
We cant have snack() contain a try/catch for the HE inside of it.  
It can try/catch things like an arrayindexoutofboundsexeception which isn’t the best solution  

```
If(ArrayList.empty()){
    Throw new HE();
}
```

Reflection sudo code

```
Public String toJS(){
    String s = “{ “;
    // without reflection we have to update this with any member var we use and any time we add one, come back and add it here
    // better to do reflection
    For each field
        S += fieldname + ": " + fieldvalue
    Return s + “ }”;
}
```

Websockets allow server to send message to client, AJAX is just a response from the server due to the client requesting it. Websockets are bidirectional

Since Midterm:  
Threads are a continuously running function   
They let us have parallel sockets and method calls at once    
Threads .start(); to run  
Threads .join() thread calling it waits for the other thread that is running to finish and return back. 
Call thread t.join() in the main method, main pauses until thread t finishes its thing and returns back to main  
It is hard to write threads because they “share” data. Their order is fairly arbitrary so we need to control when writing data so the data stays between threads.  
Data “tears” so you can read in the middle of a write where half is the old data half is the new data and it screws up everything  
To correct this: synchronize on methods to allow only one thread for that method  
We can also eliminate the sharing. Each thread gets its own chunk of the data and stores a end result, then combine together at the end for the whole data set.

Server programming  
ServerSocket stays open and listens on accept(); it waits and returns a socket which is the open connection between server and client  
Sockets have get input stream and get output stream which allows us to read/write through the socket  
We can use threads to handle multiple clients on a server as listening to multiple threads each with a socket, or have a single thread listen to multiple sockets.  
Threads for clients requires more syncing between the methods. Threads aren’t free, It takes more to switch between them. One thread for all clients is more complicated but can be more efficient.  

WebSockets need “handshakes” to establish the connection. Turns from HTTP requests into something else. “Fails gracefully”. Uses a message frame for encoding/decoding incoming/outgoing messages. Includes length which can be split and is dynamic how many bytes you need in the message so you don’t waste a huge chunk for small messages

Android uses Activities, Intent, and Views  
Intents allow data movement between activities. Views are the specific things on screen in an activity  
Android forces threads. We can’t do complex stuff in the main thread, only UI stuff. This lets UI run fast for updates while stuff happens in the background. 

Gradle is for library management and building a deployable application  
Docker creates containers that gives predictable environments for application running. Containers can be deployed for a reliable usage from everyone.  

### NodeJS  
Create a server in JS  
JS vs Java on server development  

Front end development on the server was easier. We didn’t have to decode or create unique headers. Things were quick and easy with .send(); and then instantly respond with an event listener. Java backend required more setup and decoding and bit manipulation. Front end had that built into the browser. We could also interact with it while its running. We didn’t have to worry about threads. We just cared about our own stuff and our single connection to the server. 

People eventually realized that JS, and JS in chrome is really fast. Chrome uses V8 as its JS engine. They also realized things had to be written twice, both front end and back end (things like data validation). 

NodeJS is Chrome’s V8 engine and some additional libraries to allow for server development  
NodeJS servers with library support allows servers to be a lot shorter in code than Java

Make a new folder and inside run ```npm init```  
Some important parts of this is the “entry point” question which is where the “main” method is  
This returns a packages.json that has all the dependancies  

Then start editing main entry point  
If we need a new library use ```npm install name``` this will download it and add it to the package.json. 
When we run it do ```node myFile.js```

Node is defaulted to single threaded  
It doesn’t necessarily run top to bottom. Have to keep in mind when things happen from main run  
Libraries are stored as ‘modules’ and we import it as a variable name and to use it, do variableName.method(); 

```
var http = require(‘http’)
http.createServer(….);
```

Websocket library can wrap around the HTTP server and “steal” whenever there is web socket stuff

