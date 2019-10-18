## Week 3 - Day 1
### Javascript
Interactive website  
New servers use javascript as well  
0 relation to java  
Variables do not have values associated with them  
Values do   
3 Is an int, “hello” is a string.  
Both can be stored in the same variable at different times  
```let x = 3;```  
```x = “hello”```  
```x = [1,2,3];```. 

```x=3;``` and ```var x =3;``` are older syntax for declaring. x is global which is usually not something we want.  

Called “dynamic typing” common in ‘script’ languages  
Kind of similar to interfaces in java. Java was restricted had to announce it was using it and fully implement it.  
Javascript is a lot more open for this.  
Static types allow the compiler to find bugs, it knows pre run time.  
Javascript you have to follow the path to reach that type then see it occur in the runtime  
But it allows more freedom.  
Freedom vs safety  
Only one number time - **double**  

Try to keep a variable limited to one type regardless. But it is occasionally useful  

Two methods for declaring functions

```
Function fName(param1, param2){
};
```

Functions can be stored as a variable. 

```
let fName = function(param1, param2){
};
``` 

Both do the same thing.  

Functions can be declared anywhere, including inside other functions  
This scopes the function, only lives inside the function it is nested inside of  
Functions can be passed as a parameter  

```
Function double(a){
    Return a + a; // works with numbers and strings
};
```

Typescript is like javascript but you add type names for verification.  
Function parameters are dynamic.  
You don’t have to provide all the arguments in the input. Any not passed params they are set to nil.  
Too many arguments, any extra are ignored.  
You can get all the params as an array from the arguments[#] call. This lets you loop through the arguments if needed.  

Objects are also very flexible  
It is like a map/dictionary 
 
```
Let x ={}; // empty object
x.a = 1;
x.b = “hello”
x.c = [1,2,3];
```

Totally dynamic and able to be used however you want.  
Classes are a recent addition to javascript but “classic” javascript is usually just an object like this.  
Methods are object fields with a function stored  
```x.method = function(){console.log(“hello”);}```

```x[“d”] = x; //stored as a pointer```

There are object literals as well. This is a JSON file

```
Let x = {
    a : 2,
    B: “hello"
};
```

To run in browser nest a script tag inside the html tags

```
<html>
<script type=“text/javascript” src=“myScript.js”> </script>
</html>
```

Console output is in the web browser developer tools.  

### The Dom
**Document Object Model**  
Web document is a big object  
Interact with the things within the object to change the page  
This is the “document’ object. It contains all the html nodes  
Document is a tree  
Html with a head and a body node which have their own nodes  
Each node is more or less a html tag. You can change the node or things around it  
We can select nodes, navigate through nodes (including looping, moving up/down), modify nodes, add nodes, remove nodes  

To do this use methods of the document to grab different elements of the page. 

```Document.getElementsByTagName(“tag”);```  
```document.getElementByID(“id”);```  
```document.getElementsByClassName(“class”);```  
```document.querySelector(“CSS Query”);```  


Node object has parent and children fields.  
```Node.style.background = “color" // lets you set styles```  
Better way is to have a CSS file and add/remove class names to the node  
```Node.innerHTML = “hello <a href=“http”> link </a>” ```  
lets you put in new html text. 
To keep what is there and add, or not use strings  
```document.createElement(“tag”)```  
```element.appendChild(put the createElement as the last item in the element node  you are appending to)```  

  
