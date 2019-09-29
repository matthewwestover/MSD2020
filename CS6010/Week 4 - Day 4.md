## Week 4 - Day 4
### Templates
Currently we are limited to specific variable types when called.  
Vector class creation is currently set to only accept ints  
Using templates we can specify different variable types to store inside the vector. 
<> allow us to do this.  
**<> Templates**  
Code we write where we fill in later on. Usually a type.  
*Fill in the blank code*  

```
Void print (int x){
    Cout << x << endl;
}

Void print (string x){
    Cout << x << endl;
}
```

Same basic code, only differs in “type”. We don’t want to write a million overload functions for all possible types  
We also don’t know all types, new ones can be created.  
We need to create a print function that allows us to “fill in the blank”  

```
Template<typename T>;
Void print(T x){
    Cout << x << Lendl; 
}
```

```Print<string>(“hello world”);``` vs ```print(“hello world”)``` not the same. “Hello world” is a cstring, can be converted to char*  
Compiler will auto use as a char*. This prevents unnecessary type conversions.  

```
Template <typename T>
T read() {
    T ret;
    Cin >> ret;
Return ret; 
}
```

Int x = read(); Compiler doesn’t know what type to read, and can’t figure it out based on parameters. You have to specify  
Int x = read<int> (); instead.  

```
T double(T x){ // multiple methods to accomplish doubling input. 
    Return x*2; // works with ints doubles longs. This is most natural to use. 
    Return x + x; // works with above and strings. This is the best option in this case if you knew strings were important for doubling. 
    Return x << 1; // works with ints if you pass cout you would print 1. 
}
```

T remains the same in the entire file it is passed into. We can declare multiple templates to allow different types

```
Template< typename T, typename U, typename V>;
V add(T t, U u){
Return t+u;
}
```

Templates in classes

```
Template <typename T>
Class myClass{
Public: 
    T method();
    Void method2(T t);
Private: 
    Vector<t> v;
}
```

Template for method functionality outside of the class.

``` 
Template <typename t>; // this goes above EVERY SINGLE METHOD
T myClass<T>::Method(){
    // body does stuff
    Return T t;
}
```

```
Template <typename T>;
myClass<T>::myClass(){
    //Constructor code
}

myClass<int> c;
```

Can pass T as reference and const reference (const T& t); 

### Errors
Compiler doesn’t check code until T is set. Then it sees if T makes sense.  
Assumes the input will work no matter what.  
Auto complete in Xcode gets difficult.  