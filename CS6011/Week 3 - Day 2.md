## Week 3 - Day 2
### Event Listeners
Javascript allows for interaction on webpage. We can listen for events just like in other languages.  

Older way -> ```<img src=“pic.jpg” onclick=“javascriptFunction();” />```  
This is done in the HTML document, with the function written in the javascript file

We can do it within the javascript code as well.  
myImg (get from different methods, like get by tag name, class name, etc.)  
```myImg.onClick = doStuff;```  
Don’t use () we dont want to call it and return it until the click happens  

Javascript can return functions

```
onClick = makeClickListener(“you clicked an image”);
Function makeClickListener(message) {
    Return function(){
        Console.log(message);
    }
}
```

This calls the click listener on loading then every time you click, the message is output to the console. Is a bit weird but totally valid and has use cases.   (Mainly for managing context)  
You can only have one listener with the onClick formatting  
To have multiple is ```myImage.addEventListener(“click(event type)”, doStuffFunction);```

Anon functions are also common in this format  
```myImg(“click”, function(){console.log(“clicked”);});```   

```window.onload = function(){};```  
This prevents javascript from running until all the HTML and CSS have loaded.  
Good to wrap around your javascript so you don’t run things with elements missing.  

```window.setTimeout = function(){};```  
This will run code after a delay of time 

```window.setInterval = function(){};```  
This runs the function after every passage of time. Time is measured in milliseconds

### AJAX
How we talk to the server. Acronymn is outdated  
Asynchronous Javascript and XML  
More like AJAJ - asynchronous javascript and json  
We can request extra stuff without reloading the whole page. This is how pages update without reloading (opening an email in gmail doesn’t load a new page, still just on www.gmail.com)  
Asynchronous - we make a request, do stuff, and wait with listeners for the response back. This lets other stuff happen while the request is made.  Synchronous programing halts the program until the function call is finished.  

XMLHttpRequest object to load stuff from a server without reloading. 
Listeners react when we get stuff back. 

*AJAX lifecycle*

```
Let xhr = XMLHttpRequest();
xhr.open(“GET”, url); // can also send PUT and POST request like the http server request headers.
xhr.addEventListener(“load”, whatToDoWhenLoaded);
xhr.addEventListener(“error”, whatToDoWhenError);
xhr.send();
```

### Javascript Animation
Canvas = like SFML  
We control each frame. It is a erase/draw loop  

SVG - scalable vector graphics  
Add/manipulate elements  
Automatically drawn  
Amazing tool for layout and such  

Canvas is a tag in HTML. (\<Canvas>)  

```
Let canvas = getElementByID(‘canvas)[0];
Let ctx = canvas.getContext(‘2d’);
ctx.clearRect(0,0, myWidth, myHeight) erases what is in window
ctx.fillRect(20,20,100,100); draw a rectangle
requestAnimationFrame() pass a callback to be executed when time to redraw the screen (60 times a second)
```

SVG is more powerful but more work. 