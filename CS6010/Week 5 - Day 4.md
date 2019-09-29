## Week 5 - Day 4
### Trace Stack and Heap Memory
Int x =3  
Int \*y = new int;  
Int \*z = y; (to figure out where it is pointing, remove * and that’s what has to be on the other end)  
\*z = x   
*Stack has X frame which is int 3*  
*Stack gets Y frame which is int \* pointer to heap frame of int*  
*Stack gets Z frame with int \* pointer which points to the same heap frame int as y*   
*\*z sets what’s at it end pointer, thus the heap int becomes 3 from x.*  

Int \*\*w = &z  
*Stack gets W frame of int \*\* that points to Z frame on the stack*

(\*\*w)++  
*Dereference twice - points to z frame then to heap frame, increases value becomes 4*  

z++  
*Changes the z stack frame to point to a point on heap one up from the heap int 4. We don’t know what is in that address*

Delete y  
*Deallocates the heap memory for everything*  

### Constructors
Default: ```Constructor();```  
Copy: ```Constructor(const Constructor&);```  

* To use becomes Constructor(Constructor A&); 
* Has to be const ref, because this is how it knows to copy, can’t make a copy without copying otherwise
* Infinite recursion

Destructor: ```~Constructor();```  

Set equal has two options:  

```
operator=(const Constructor& ) // Can see if there is enough space and if there is, uses that
Constructor& operator=(Constructor); // Makes a copy this allocates new memory and deallocates old memory
{
C c; default constructor
C c2(c); copy constructor by value
{
C c3 = c; calls constructor to make new one
C3 = c2 this is operator = 
} // calling ~c3(); to destroy it. Dies at end of bracket
} // calls other two destructors in reverse order (~c2(); ~c();)
```

### Operator Overloading
Write one like ==, when we do !=, can use == to write it. (! ==)  
Operators can be overloaded as const as well

### Templates
Fill in the blank <>  
Messy so be careful to ensure you include it as correct  

### Cache Memory
Access memory in order when possible, as cache can speed up and make program faster  

### Documentation and Testing
To make a library there are things we need to let other people use.   
Knowing how to start with library  

* Tutorials and Forums
* Documentation has thorough notes and info on every class, function, etc. 

Good Documentation is KEY for a public use code library to live.  
Library should do what it says it will do. This requires good testing  
There are tools to help manage these  
Documentation is created automatically by program that reads updates to code database and updates as needed  
It can even create images for things like code hierarchy  
There are several programs that can do this  
Doxygen is a popular open source program for creating documentation  
Use /// or /** **/ to include comments on code when Doxygen generates   documentation  
Any “public” info in classes, functions etc show up in documentation  

doxygen -g doxyfile (generates the config file for the documentation)  
Edit doxyfile as needed  
Run in top of project structure - same level as the cmakelist file.  
doxygen doxyfile (creates the documentation)  
Checkin doxyfile to git, but not the generated documentation  

Automated Testing is super useful  
Assert is simple for testing  
Crashes as soon as there is a test failure, hard to see if multiple things fail  
Output can be difficult to read  

Better framework for testing is catch2  
Output is better  
Doesn’t break so you can have multiple test fails  
Runs as a separate thing than inside the main program  
Set up header as “asserts” and run. Auto generates the main.cpp file.  
Just needs to be pointed at the the program to test files. 
Good tests ensure that if you change something/move something the code doesn’t break  
Create test folder and put catch2 header and a tests.cpp/.hpp  
Add new test executable to cmake list  
Link the library   
Target_compile_options (testSFML PUBLIC -std=c++17). 
Tests PUBLIC -stdc+=17  

Make in build folder and run the ./test application that is built  

Refactoring code to clean up and make it easier to add  
Things to refractor include renaming variables, moving code to independent  functions or into loops, multiple related variables passed around can go into a struct, remove commented out code not in use, etc  
Make sure program works before refactoring  
Go in small steps and commit every time you change things to have checkpoints  
Xcode has simple refactoring automation rename a variable, pull out into function  
C++ can be hard to refactor  

