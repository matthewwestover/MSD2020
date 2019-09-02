## Week 1 - Day 2
### Hardware
Processor is central  

* Gives access to Monitor, Keyboard, Mouse 
* Permanment Memory Storage
* RAM (Random Access Memory)

Programming is mostly the Processor -> RAM connection  
We use Processor to access other parts of the computer  
Program is what being interacted with, OS interacts with the rest  

Programming is manipulating what is in RAM and running different executable commands

CPU

* Does Math
* Changes to Perm Memory
* Talk through OS to I/O

Programs are lists of single executable actions. Can be used in basic logic  
Commands are compiled into binary commands, the core of what all computers execute  
Binary is incredibly difficult code in for humans. This is the lowest level code
Binary is the only way for computers to actually do anything  
We write in human understandable languages that is converted to the binary by the computer  
Normal English is poor choice. Verbose, open to interpetation, context matters  

This led to creation of programming languages

* C, C#, C++, Java, etc.

These languages are very structured and specific  
Unambigious syntax  
The Compiler translates to binary  

Compiler is a bootstrap issue, can’t write higher level without a compiler, so how do you write compiler. Gradual increase in complexity over time based on previous version compiler. 

### Errors

* Happens to everyone 
* Always will happen to everyone
* Can take longer to find then to fix
* Syntax - incorrect "grammar" for the language 
* Logic Errors - grammar is correct, but output is not as expected
* Compiler tells you what is wrong with syntax errors
* Logic errors compile and run no problem
* Syntax is much easier to fix once you find where the error is located

### C++
Programs are lists of **statements** and **definitions**  
Matter of "do something" vs what "something is"  

**Statements**: 

* Math 
* Display something on the screen
* Get user input
* "Do Stuff"

Definitions are adding new *words* to the language to use

```;``` All statements and definitions use this to end.  
```// Commment``` All comments are ignored by the compiler. Used by programmers to leave more information and context.  
```#include <iostream>``` Headers literally copy and paste other code into sections to be used. Copying over happens in compiling

```cout``` - console output  
```cin``` - console input  
```std``` - standard library  
```"This is a string"``` - text that is input/output, but not actual code  
```endl``` - new line command  

### Ram
Core Fundamental for Programming  
How it works is important to know  
RAM is a list of bytes (1s and 0s)  
These are 8 digits of binary - Basically an integer from 0 to 255  
Each byte has an address. Numbered in order from 0  
Humans like names not numbers for remembering things  
Compiler handles RAM, addresses, accessing, and changing values stored there  
RAM is a 3 Column Table

* Name
* Type
* Value

Integer = 4 bytes, [-2^31, 2^31-1]  
Float = scientific notation, moves the decimal - 4 bytes  
Double is bigger float - 8 bytes  
Boolean = true/false 1 byte even though it is really only 1 bit of info (1 vs 0)  
Character = single letter = 1 byte  
String = text = byte size is odd  

In C++ “names” in RAM are identifiers

Variable names start with a letter, can contain digits and underscores  
Capitalization matters  
```name =/= Name```  

Stay consistent in naming convention  
use camelCase or snake_case  

Variables are chunks of RAM with a name and "type" associated  
We have to tell the compiler what to call and reference for each variable  
They get *defined* before use.

Declaring adds the word to the dictionary: 
 
* int x;
* float f;
* bool myBoolean;

Create an assignment at the same time to define what the word means:

* int x = 5;
* float f = 3.02;
* bool myBoolean = true;

Be descreiptive with variable names  
If using abbreviations, still be clear, dont be lazy.  

```x = x + 5``` is valid  
```x + 5 = x``` is not valid  
```=``` is an assignment
```==``` is if something is equivalent  

Compiler can automatically handle +,-,*,/  
% is "modulo" division with whatever is the remainder  
^ is a bitwise exclusive or. It is not the two the power of  
```1/2 = 0``` integers don't have floating digits and gets truncated to 0  
```1/2.0 = 0.5``` compiler returns a float because one number is a float  
```x = x + 5``` is common  
``` x+= 5``` is the same as above  
can use any of the common math operators (+, -, \*, /)  
```x++``` adds one to current value of x. Very common.  
```std::cout << “hello world, my age is ” << userAge;``` outputs to console hello world, my age is 29.  

```"\n"``` or ```std::endl``` returns a new line  
