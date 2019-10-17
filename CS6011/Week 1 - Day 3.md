## Week 1 - Day 3
### Java
Very similar to C++  
People were writing a lot of C++ code in the 80s/90s with lots of bugs  
Java was created to be like C++ but with checks to prevent a lot of these bugs  
Java is also created to be simpler  
Array access bounds are checked  
No delete method

* Would be a memory leak since we cannot access
* Moves from programmer responsibility to runtime
* Uses a Garbage Collector
* While program runs it checks all pointers and marks things in heap as in use
* If nothing is pointing to it, it clears it out
* Means we don't know when memory is deallocated

No copy constructors or operator = overloading  
Every variable is a pointer, nothing set in the stack, points to heap  
When you = two variables it creates a different thing on the heap  
By design no memory errors  
This limits what can be done with the language and limits the efficiency.  
C# is a later version of java by Microsoft that fixes some of the efficiency issues  
Java doesn’t compile to machine code. Compiles to java byte code. It gets run by the Java virtual machine (similar to the C++ virtual machine machine code assignment we had)

Java functions much like C++  
Some structural changes  
Everything goes inside a class  
Every file has 1 class, class name must match file name  
No header file, everything is inline as it happens.  
Multiple folders are in “packages” linking to other files has to include the package name at the top before the public class  
Java doesn’t have function. Only has methods  
Java starts with a static method to run  
```public static void main(string[] args){}```  

Trust the IDE to do things for you, Java is simpler IDEs are more powerful than C++ ones

Primitive Types No references, no pointers, in the stack

* Int
* Boolean
* double
* char
* Long
* Float

Object type stores a pointer to that thing on the heap

* String
* Array
* ArrayList
* myClass

```String[] s2 = strings``` If you edit the strings array s2 is changed as it points to the same location.   
We do have copy constructors. If we want to make an array copy we have to be explicit. Array deep copy method  
One = means point to the same thing  
== means compare pointers. It sees if two pointers point to the same location  

```
S1 = hello
S2 = hello
S1 !== S2
```
To see if equals we use ```S1.equal(S2)```  
Primitives can be compared with == as they live on stack

No operator overloading   
System.out.println() = cout  
Scanner class to get input  
```Scanner s = new Scanner(System.in)``` <- keyboard  
```Int I = s.nextInt()``` to read in next item from input
