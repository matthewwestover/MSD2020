## Week 11 - Day 1
### Design Patterns    
Before coding it is often good to think about what “pattern” your program will follow. This way you end up with a general idea of what you need to do.  
Ex. Interp pattern  
We have an Expr class with subclasses that can interp()  
This is a natural recursive interpreting format

Design Patterns by “The Gang of Four” - class based object oriented book  
One language’s pattern might be another languages construct  
Such as iterator - this started as a pattern but Java and C++ have constructs for it

Patterns can be described as creational, structural, or behavioral  
**Creational** - how objects are created  
**Structural** - how different objects are combined  
**Behavioral** - how methods are implemented

### Singleton Pattern
Some classes should only be instantiated once  
EX: GUI object should only be created once for the application  
There is a private constructor for the object and then a public “getter” that always returns the same object  
Why not just a global function? This pattern allows subclassing as well as easier to break to multiple object creation if needed later. 

### Builder Pattern
Some objects have lots of arguments for construction  
Create a class that “builds” the object. It gathers all the arguments needed  
Takes what is mandatory and then has additional things as optional  
You “create” the builder object then once ready, use it to make the object you really want 

### Factory Pattern
Some classes change, and how they get instantiated changes  
With this create a class that has methods to generate each portion you want (new objects not just flags like builder pattern)  
This also makes it easier to create subclasses for the factory.  
Turns a compile time decision into a run time

### Dependency Injection Pattern
Object that you use might change  
Parameterize components instead of hard coding values in  
Typically there is a “dictionary” that returns the object want by looking up 

### Decorator Pattern
Different combinations create too many subclasses  
This creates a subclass between other subclasses. This basically allows chaining options for different subclasses while only have the generic “do this” method

### Facade Pattern
Complex classes and subclass dont need to exposed to the client/user  
One class is exposed while accessing multiple more complex classes 

### Observer Pattern
Many independent reactions to other things happening (inputing a form, calculating sales tax, and total for example)  
Each field creates an observer in a vector. The thing that is changing gets a notify() to alert all the observers. 

### State Pattern
We need to know if certain things are “ready”, “incomplete”, “error”.  
Add a dispatcher to indicate based on which “state” the object is currently

### Visitor Pattern
There can be excessive edits to add new operations.  
EX. Our interpreter is spread out into many subclasses, which is easy to add new ones.  
However to add a new method means updating all the subclasses as well. It is a trade off.   
You can create a “visitor” to help facility adding/updating all the already established code.  
Just have to add “visit()” to each subclass.  
Then create a new visitor for each new method to add. It localizes it to one space where the new methods are being added.