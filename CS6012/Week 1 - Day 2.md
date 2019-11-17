## Week 1 - Day 2
### Generics
How java access levels work  

| Modifier/Level | Class | Package | Subclass | World |
|:--------------:|:-----:|:-------:|:--------:|:-----:|
|     Public     |   Y   |    Y    |     Y    |   Y   |
|    Protected   |   Y   |    Y    |     Y    |   N   |
|   No Modifer   |   Y   |    Y    |     N    |   N   |
|     Private    |   Y   |    N    |     N    |   N   |

We want a data structure that just contains “things”  
We want it to automatically grow if it gets full  
Be able to add/remove items  
We can use the <> on ArrayList to take a “generic class” to specify things for growing/adding/removing  
This is similar to templates in C++  

To specify any type:

```
ArrayList<T> {
    T storage [];
    Int capacity
    Public void add (T item) {…};
}
```

Type has to be an object. It cannot be a primitive  
In C++ templates will generate a specific new set of code at compile time and insert into the code.  
Java doesn’t do this, it doesn’t store the type at all. It has Type Erasion.  This allows Generic Programming  
The placeholder is specific. Ex. Triangle is a Shape subclass.  
ArrayList<Triangle> cannot function as ArrayList<Shape>  
This happens regardless of the input T type’s heritage  

```
public void doStuff (ArrayList<Shape>) {...}
ArrayList<Triangle> tri_list;
ArrayList<Shape> shape_list;
doStuff(tri_list);   // ILLEGAL doStuff only works on ArrayList<Shape>
doStuff(shape_list); // OK
```

Triangle can be added ArrayList<Shape> shape_list however.  
Method signatures involving a generic don't apply to subclasses 

Java uses a **Wildcard: ?**.  
<? extends Shape> refers to Shape and any subclasses.  
```doStuff(ArrayList<? Extends Shape>)```

This is an upper bounded Wildcard, anything that inherits from this class  

A lower bound Wildcard extends to anything a class inherits from
```<? super Triangle>```  
this will effect Triangle and Shape and not Circle
This means List<Triangle> and List<Circle> have List<?> as their parent

Every class is an object.  
We could in theory build these to just hold objects.  
Generics however allow for type-checking at compile time instead of runtime.  
It catches type mismatching before your code runs.  
Compile time errors are always better than a runtime error. It is harder to catch runtime errors  

Before generics:

```
ArrayList L; // implemented with Object in mind L.add(new String (“hi”));
Shape i = (Shape)L.get(0); // crash!
```

After:

```
ArrayList<String> L;
L.add(new String (“hi”));
Shape i = L.get(0); // compile error
```

Generics doesn’t work with primitives: *int, float, char, double*  
We can use a “wrapper class” to use them: *Integer, Float, Double*  
To get value ```Integer.intValue();```

Java does auto boxing for conversion:  
```L.add(5);``` Equivalent to ```L.add(new Integer(5));```  
Still better practice to specify the Integer so you don’t accidentally mix up datatypes. Little more work for clear data flow

Static methods can be generic as well  
We declare the generic type before the return type. 

```public static <T> boolean isThis()```  


You can have things take multiple generic types using different names <T1, T2> and say you want to use T1 in places and T2 in different ones. This doesn’t mean they HAVE to be different but can be.

### Comparators
There are two ways in java to sort generic objects  
We need to assign an ordering  
Comparable and Comparator  

Comparable Interfaces which has a single method compareTo(T item). 
compareTo should be consistent with equals()  

Functors = a function object. That is a class with a single method inside of it
Pass in a comparator object to a sorting function.   
Comparators have a single method compare(T left, T right); takes two arguments  and says which is greater:  
Returns -1 if left < right  
0 if they equal  
1 if left > right  