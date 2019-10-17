## Week 2 - Day 1
### Interfaces
Classes can be unique but share common components, methods, member variables  
An **interface** is a set of *methods* that a class can implement that is shared with other classes  
These methods can be implemented differently in each class, but can allow you to call a method on some object that you don't know what class it is at compile time  

```
Public interface Drawable{
    // Methods an object needs to be drawable
    Void draw(); // nothing about how it works, just that it has to exist in an object to be drawable
}
```

Interface on its own doesn’t do anything. Just a list of method signatures that don't do anything   
We then write classes that contain the listed methods. The methods can be done differently in each  
But by having it, all classes with a draw method become drawable  

```
Public class circle implements drawable{
    Private int radius, x, y;
    Void draw(){
        //body
    }
}

Public class square implements drawable{
    Private int x,y,w,h;
    Void draw(){
        //body
    }
}
```

To draw these  without the ```implements drawable``` we need an ArrayList<circle> circles and ArrayList<square> squares then do a for each loop in each to x.draw();

With ```implements drawable```  we can make an single array list of drawable objects

```
ArrayList<drawable> shapes = new Arraylist<>
For(var c: shapes){ c.draw(); }
```

This is all somewhat similar to templates in C++   
Allows method calls for different types   
The methods can have implementation <- this is not figured out at compile time   
To make this work, we have to work with pointers. In java everything is a pointer already so we dont have to think about it as we do it. This is why we didn’t do it in C++ (but we could).

Can have array lists of different classes, return different classes, etc as long as they share the same interface.

Anytime we declare a method that is declared from the implements interface line we need to tell the compiler to override  
Put @Override above method definition in the class  
This is a check to make sure it compiles correctly  
Ex: ```int draw()``` instead of ```void draw()``` will break because it doesn’t match    

Interface is just a **promise to have methods** we can have many interfaces. 
All interface methods MUST be present in the class to be implemented to the class  
List as ```implements interface1, interface2``` for multiple different interfaces  

Interfaces cannot overload a method with: 

```
Int draw();
Void draw();
```

This breaks. However we can overload by passing a var:

```
Void draw();
Void draw(int); 
```

Parameters have to be different to work. Otherwise the compiler Cannot tell the difference. It doesn’t know what happens with return type. 

### Inheritance
Interfaces guarantee what methods a class will have  
Often these methods are done similarly  
Classes can inherit the method instead of writing over and over  
We have *base* (parent) classes and *derived* (child) classes  
Inheritance “is - a” relation ship
Child class is a parent class
Ex: 

```
Public class exception{
    Printstacktrace();
    String message();
}

// IOException is a more specific exception, but it “is-a” exception still. 
Public class IOException{
    Private int index;
    Public int getIndex {return index;)
    Printstacktrace(); // same as the exception class
    String message();  // same as the exception class
}
```

IOException can inherit the ```printstacktrace()``` and ```message()``` methods as they are the same. No reason to write twice.  

To say the IOException inherits we use extends
```public class IOException extends Exception{}```

We can still override an inherited method to allow that method to be functionally different.

```
@override 
String message();
```

To use the parent's method in modifying the child's method use ````super.method()``` 
Update IOException message:

```
message(){
    Return super.message + “index: “ + index; // this returns Exception message + the index
}
```

You can only inherit from one parent class  
You can interface with as many interfaces as you want as long as method names don't clash  
You can chain inheritance. ```Super.method()``` is just the immediate parent class  
If we had ```IOChild extends IOException``` We can't use the Exception class ```message()``` to create a new overridden one, only the IOException one. 

Private variables in a parent are still only accessible in the parent. The child has the private variable but cannot name it to manipulate it   

Use ```protected``` to keep variable "private" but accessible to children classes. Things should remain private unless you are designing a class to be a parent level of inheritance.

Keeping things private reduces surface area of the code, which reduces possible bugs  

Children classes often need to use constructor for parent as part of their own construction  
Use the super caller again  
```Super(arguments for construction)```  
If you don't call super it just calls the default constructor for the parent. 

Inheritance can use the same code for methods and member variables. Interfaces allow you to have similarly named methods that have different implementation. 