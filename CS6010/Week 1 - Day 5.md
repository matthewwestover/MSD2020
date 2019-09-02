## Week 1 - Day 5
### Functions
Functions are named pieces of code  
This is useful for documentation purposes  
Makes it clear what is happening  
Can be repeated as needed

This helps keep code DRY (Dont Repeat Yourself)  

Functions get defined then called in other parts of the code.  

Syntax is functionName(); 

Some functions require additional parameters/arguements which are placed in ().  

```std:sqrt(4)``` -> returns the square root of 4  

Pretty much anything can go in functions except other function definetions.  

Function signatures are the top line definitions of a function

```type name(parameters)``` line

Functions are the fundamental component of programming.  
Functions can be checked piece by piece as programs growing.  

##### Defining a Function

```
returnType functionName(parameters){
    //body
}
```

Parameters need state what kind of data it is expected to be and a reference name for the data.  
Parameters are like “secret” variables and only found within the function definition. They cannot be directly accessed outside of the function by other code.  

```
double sqrt(double x){
    x = x/2;
    return x;
}
```

Return exits the function and allows you to set the return variable as a seperate variable in the main code. 

If there is no return value, use type *void* for function type.  

Closing } implies a return. 

```
Void Greet(string name){
    Cout << “hello” << name;
} //return;
```

### Memory
Data is organized in the call stack  
While a function is running a new chunk of memory is "pushed" to the stack. 

* Pushing is adding to stack 
* Popping is removing from stack
 
The function "frame" is added to the stack.  
Once the function ends, the frame is removed.  
Each frame has a return address, which is an inaccessible part of memory telling the stack where to return in the main program list. 

When a frame is removed, the data remains, but the label is removed. This means its possible for the data to show back up later. This is why you you should intitialize variables when you use them.  

Stack frame variable names are local only. They can’t refer to other function stack frames.  

Calling the same function inside itself (recursion) can create a *stack overflow*  

This is not the same as an infinite loop. This eventually fills the RAM and crashes the program.  

Recursion has its uses, not always a bad thing.  

### Contracts, pre-, post-conditions
* These are “rules” that govern a function
* Preconditions are what must be true for a function to run
* Postcondition this is what must be true when the function finish
* Together they form a contract
* Documentation of these is useful

### Testing
Use assert

```
\#Include <assert> 
Create test functions 
Test1(){
Assert(x==2)
}
runAllTests(){
Test1();
Test2();
}

Main(){
 runAllTests() <- comment out when done with testing
}
```

Example Verify string input is int:

```
For(int I = o; I < input.size(); I++){
    Char c = input[I]
    If ( c < ‘0’ || c > ‘9’ >{
        Return 1;
    };
};

Int num = stoi(input);
```