## Week 2 - Day 2
### Overriding Methods

```
Public class A {
    Public void method(){print(“A”);
}

Class B extends A{
    @Override
    Public void method(){print(“B”);
}

Class C extends B{
    @Override
    Public void method(){print(“C”);
    Public f();{….}
}

A a = new A();
a.method(); // prints A

C c = new C();
c.method(); // prints C

B b = new C();
b.method(); // prints C
b.f(); // can't do that it is stored as a type B object
```

Version of the method depends on the type of object not the type of variable  
We are creating a new C object, storing it as a B variable  
Variable is the “lower” bound of what can be called. Even when its object is a child of it.  
So anything in C that is in B it can use, anything in C that is NOT in B it cannot

```
C c = new B();
    c.f(); // can’t work. We would expect c to call a C function, but it is a B object that doesn’t have access to f();
```

To figure out what method to use, it starts in the object type class then goes to the parent, until it finds a functional version. So you can have a child not have a method written inside of it, but inside one of its parents

**Polymorphism** - don’t know what a method will do until called  
The classes automatically sort through method to find the appropriate method to call  
Classes Car and SportsCar can have a ```.drive();``` method, 
```c.drive()``` figures it out based on the object that it is. 

This is the point of object oriented programming. The objects figure out what things to run as they occur.

Java implicitly extends every object as an **Object**.  
```Class A {}``` technically is ```Class A extends Object{}```

Object has several methods that have default implementation that is mediocre:  
```String .toString();```
    Prints class name @ this memory address. 
```Boolean .equals(Object o);```
    Default: ```this == o; ```  
    Does this *point* to the same thing. 
Both useful but often we want to override these anyway.
```@Override equals();``` to see if values are the same instead of pointer. 
String object has a built in override equals(); to compare character by character

```
Class Fraction{
    @Override
    Boolean equals(Object rhs){   
    // Fraction rhs conflicts with Override as Object uses an Object for equals method. Has to be object rhs
    // Convert rhs to a fraction by casting
        Fraction frhs = (Fraction) rhs; // doesn’t change the object, changing the type of the pointer. Point to same spot just saying we expect something different to be there. 
        Return toDouble() == fhrs.toDouble();
    }
}
```

This isn’t safe to do. We dont know the object will be a fraction compatible.  You can pass anything into the method.  
Can wrap all in a try-catch to handle the exception thrown  
We can also see if types are the same, if not return false.  
```If(!(rhs instanceof Fraction)) {return false}; ```

Bad version of handling instance of No overrides. All classes have unique methods:

```
Public class A {
    Public void a(){print(“A”);
}
Class B extends A{
    Public void b(){print(“B”);
}
Class C extends B{
    Public void c(){print(“C”);
}

A a = ?
If( a instanceof A){} // doesn’t help. All classes are instances of A. 
If(a instanceof C){ // have to go to the lowest level so we can decide what to do. 
    C c = (C) a;
    c.c();
}
```

If you write code like this you are doing it wrong. Overriding a single method based on the class type is the better way to do.  
Only use instanceof only when you are FORCED to take a more general type and you have to call a method that isn’t in the general type

#### Interface vs inheritance? 
If the classes are related use inheritance (car - coupe - sportscar)  
If the classes just use similar methods but are topically unrelated use interface  (car - dinosaur - sandwich)  

**Overloading** is resolved at compile time. The compiler looks at the variable types, what version of the methods to run  
**Overriding** is resolved at runtime based on the objects used  

```super.method();``` calls the parent method. Used when we override in the child class but want to use the parents version. 

If you want to use a grandparent method we cant use super, generally better to write the grandparent as a ```final method()``` (which is locked and cant be overridden) then in the class you are using just call the method on the object

### JavaFx
Uses interfaces and inheritance all over the place

A *node* is a thing in the Gui. Holds stuff. Everything is inherited from node in the GUI

Window has a stage which displays a scene, which has a parent which contains the nodes  
Typically only one stage, but you can have multiple scenes that load different pages. 
A scene has a single parent which has many node objects  

