## Week 2 - Day 1
### Multi-File Projects
Larger Projects should be broken into smaller chunks.  
Functions should have a signature and a body.  
Program just needs a signature to be referenced by other parts of the program.  
The signature just points the program to a chunk of body code to execute.  
Because you just need the signature, you can have two functions call each other by referring to the signature above the body section  
in C++ multiple files are split into headers and code body files. 

.hpp/.hxx/.h for header files. These only contain the signatures for the functions. These have to be ```#include``` in any file that is going to use the functions listed. 

These are function declarations  
.cpp/cxx/.c for function body files.  
These are function definitions  
Most files come in pairs of .hpp and .cpp.  
Files are compiled into .o files, which are combined into one single executable 

We need to prevent excessive listings of the copy and pasted header data. putting ```#include <iostream>``` in every file adds a lot of unneeded code.  
Take steps to prevent this in the header files. 

Easiest is ```#pragma``` at the top of the header file. 

XCode does it a different way. 

```
* #ifndef X_HPP // if I haven’t seen this before {
* #define X_HPP // marks that I’ve seen it before
* // declaration (the code in the header)
* #endif //}
```

All header defintions go in that part of the header as well. It is auto generated within XCode. 

There are several common mistakes. 

* Undefined/unresolved symbol
    * Has a function signature (declaration) in header
    * No definition in a cpp file
        * Name doesn’t match
        * Param type doesn’t match
    * _Z3IntZ3IntF < mangled name issue. Run through C++filt to find the exact function issue
* Duplicate symbol
    * \#include “x.hpp” - in both x.cpp and main.cpp
    * Inline can be used in header file to fix

Command line compiling

```
clang++ -c main.cpp //compile only, don’t link. Creates main.o
clang++ -c x.cpp 
clang++ -c y.cpp
clang++ -o myProgram main.o x.o y.o // links files together. myProgram is the executable program name. Can use *.o to pull all without typing all
./myProgram // runs the program
```

Additional flags: 

```
-std=c++17 // Includes all new c++ stuff. Include in compile step
-Wall //Turns on warnings
```

If we update things, we only need to rerun the steps that involved change in the file.  
If we only edit x.cpp -> Only have to -c x.cpp and -o myProgram  
If we update a header we need to recompile more things  

### Vectors
These are included in the C++ standard library.  
They function like Arrays but are better. They are like Array++.  
C++ arrays are set length, not predictable with function output.  
Vectors let us store data of the same type in large amounts. and preform computations on them.  
We can loop through vectors, edit vectors, call specific items in vectors. 
Vectors can change in size, are predictable to call and have some built in functionality.  
 
```
#include <vector>
vector<type> name; 
```

You can nest vectors as well

```
vector<vector <string>> // This is a list of lists of strings
```

```
vector<int> grades(5); // empty vector of 5 integers named grades
```

Vectors are indexed from 0.

```
grades[3] // returns the 4th grade integer in the vector
```

v.size() provides how many items are in a vector.  
v.push_back("Steve") adds a string "Steve" to the end of the vector  
v.pop_back() removes the last item on the vector  
Vectors can be looped through to do things to each item in the vector.

```
Vector<int> grades;
Int total = 0;
for(int I = 0; i<grades.size(); I++){
	total += Grades[I]
}
```

This loop will calculate the total scores of all the grades in the vector.  

For each loop is less difficult to mess up, useful when ever you don't care what # of the loop you are on. 

```
for(int grade : grades){
	total += grade;
}
```

Names can get long with nested vectors  
Long types can get aliased to something shorter  

```Using Roster = vector<string>;``` -> ```for(Roster roster : uniRoster)```
