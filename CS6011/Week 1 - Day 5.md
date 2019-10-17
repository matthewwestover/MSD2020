## Week 1 - Day 5
### Static Methods
Java does not have functions, everything is a method inside of a class  
By default a method needs an object to run -object.method();  
We don’t always want to call an object   
Like in main function, there is no main object to start running the program  

Java uses *Static Methods* instead of adding functions  
These go inside the class object.  

```
Class myClass {
    Public static int method(){ } // no object, no implied this.method();
}
```

To call a static method you just use methodName();
No need to attach a created object. 

```
Math m = new Math();
m.sqrt(2); // this is unneeded
Math.sqrt(2); // Single line to get the value
```

Passing a class into a static method is legal but doesn’t make sense 
 
```
Public static void method(myClass mc){} // legal but stupid
public method(){} // this is simpler and does the same
```

### Static Variables
Makes variable more or less a constant for use everywhere  
One copy of the variable in the program  

```
Private static int x;
Public static final double PI = 3.1415; // only one copy of PI in the whole program, it can’t be changed ever, and everyone can use it from the math class. 
```

Without a final it can be changed by anyone who has access to it.  
Final means it is constant. Setting a public static without one is dangerous.  

***Don't make things static by mistake. If something says it doesn’t work in a static function, it might just need an object attached to the variable first***

### Error Handling
Things can go wrong. Java has two major methods for handling bundled together into *Exceptions*  
**Throwing and Catching**  
We don't want to just crash the program. That is annoying for users.  

In C++ we had:  
```Int stoi(string s)```  
If we pass a string xyz, this cannot be converted to an int  
We need to know the return value, and if it worked or not  
Lots of shitty options to do this: create a struct, return a pointer to the return point, return an option value  

Java has exceptions instead  
It is basically saying an error happened and handling it can happen in different places in the code   
An error occurs, the program has to notice something went wrong then “throws” to the handling of it  

```
If(bad)
    Throw new Exception(“broke”); // exceptions are objects, you can pass stuff to repair issue (like array index out of bounds, try using a different index size, etc)
```

Throw works like return, code doesn’t go any further in code of the method that broke   
Java then looks to catch what is thrown. It looks for the closest catch.  
If we write code that might fail, wrap it in a try  

```
Try{
    methodThatMightFail();
} catch(exception e){
    //do something
} catch(exception e2) {
    // do something else
}
```

When an error is thrown, it leaves each level until it finds a try block with a catch that matches the exception  
If no try block is found, it eventually ends up in main and crashes  

If a block of code can have an error, but doesn’t catch it. You declare in the the method “throws”

```
Public method() throws IOException{
    //code to run
}
```

Exceptions have methods  
```PrintStackTrace();``` shows the chain of method calls that broke when it exception reaches main without a catch  
```Message();``` provided the error message of what broke  

Exceptions errors can come from bugs(*Write the code better to prevent*) -> you should fix these  
There are ones not your fault  
Ex: file not found, user might have asked for something not there  
User tries to make a fraction that doesn’t work, not our fault they did something not allowed  
Exceptions come in two types  
*Checked and unchecked*  
**Checked** are items outside of programmer control that should be handled gracefully.  
**Unchecked** are things that programmers should fix in their code

*Catch or declare rule*  
Any method that might create or pass through a method that can be checked, it must handle it   
Either catch it, or state “hey you might get an error with this cause I might get an error with this”  

```
Public void myMethod() throws ExceptionType { //throws means it might throw. 
	// Body of code 
}
```

As we develop just declare throw in the method line to get program working  Assume things sent are normal  

Once everything works, looks at what errors happen and try to handle in a second pass of the code.  

Worst thing you can do is try catch and not do anything in the catch block of code. Always at least print out the error or stacktrace  