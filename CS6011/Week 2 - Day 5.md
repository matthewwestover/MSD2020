## Week 2 - Day 5
### Objects Class
Every class type inherits from the class Object (directly or indirectly)  
Object Class has:  
```toString();``` 

* Can create unique identifier for objects. 
* Allows generation of hash functions - can search entire list of objects in one flop. 
* This defaults to printing class name and pointer address

```Equals(Object o);```  
For comparison, compares pointers. Are they pointing to the same place

```getClass();```  
Returns type of the object it is  

### Class objects
Class object has information about a class  
getFields() - list all the fields in the objects  
getMethods() - get all methods  
Used for reflection  

### Reflection
Programming term for manipulating our code as if it were data  
Write a java program that looks at other parts of the program and passes it in as an object  
Self correcting code.  
Allowing introspecting about properties of a class.  
Type Introspection - look at any object, cast to class type and get info about this object. View class properties at run time  
This allows modification for program during run time.   

Reflection is metaprogramming. We write functions that generate programs.  Typically program writes another part of the program while running.  

```
URL url = new URL(“http://www.utah.edu”);
String urlString - url.toExternalForm();
System.out.println(urlString);
```

Using class object

```
Class<?> type = Class.forName(“java.net.URL”); 
// this is pulling out the package. Searching the entire package to find a match. 
Constructor<?> constructor = type.getConstructor(String.class); 
Object instance = constructor .newInstance(“Http://www.utah.edu”); 
// this is the end of what URL url = newURL(“http://www.utah.edu”) only done at runtime. We can have user input the url at this point to run. 
Method method = type.getMethod(“toExternalForm”)
Object methodCallResult = method.invoke(instance);  
// second line creating an object from the method found. This is the String urlString - url.toExternalForm(); 
//You don’t need to know what the output of the method is, it automatically sets the object type. 
System.out.println(methodCallResult); 
// prints the class name and pointer address
```

**Why**  
Create frameworks that handle code that has not been written  
JUNIT for testing. @Test annotation does a search for all annotations of @Test to run  
This is how it can access private variables and methods, even though its not directly linked to the class  
@Override is another example. Java doesn’t know what program will do, but can search for a matching method name.  

Serialization - write data to a file and then read it back.  
Is error prone to come up with a text representation of the objects  
It is better to write code that loops over all members to object  to be sure it is written/read correctly.  
Implements Serializable interface. 0 methods in it.  
This lets you use ObjectOutputStream which has writeObject method and  ObjectInputStream readObject method that can output and input the object. - cast back into correct type. It comes in as generic type, cast to what it was

**Annotations**  
Allows library developers to use just a few keywords to act on annotations without having to do anything extra  
@Test - this tells java that this method is just a test method, not part of standard program.  
JUnit calls the getMethod class object method and invoke to call those methods.  This lets JUnit run private methods and member variables during test runtime.  
JUnit can’t know what all program that will be written in the future that will use it to test. It uses reflection to automatically get what it needs to run the test.  

Spring library  
Use @repository annotation, it knows that the class access a database. It will then be able to use read/write without having to make SQL calls.  
    Translates java to sql and back. All under the hood, and without you having to do it by hand
    
Android programing  
    Uses XML (\<Button>)  
    The XML is parsed into java objects based on the text of the XML. It creates the full java POJO (Plain old java object) from a single tag.  
    Define XML file at compile time, then when program runs it reads the XML and creates the javaobjects  
Android Room - an SQLite database that is only accessible by the app that creates it  
@Entity indicates that a class is meant to be a table in the database  
You can use @insert @delete, etc to do the SQLite calls to/from database  




