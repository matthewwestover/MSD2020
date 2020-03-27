## Week 7 - Day 1
### Writing Specifications
A specification is about observed behavior of a system  
If the specification is for a library, it talks about functions and classes, but not for loops  
If the specification is for a program, it talks about I/O, command lines, and GUI elements, but not functions and classes  
Specification ALL about what is seen in the context it is used. It doesn’t matter what’s inside that gets what you see.  
It can be useful to define the terms in the specification much like a function is defined

Why is it important?  
A specification is like a legal document  
If a behavior is not defined then the client cannot rely on it  
Ex. Homework - if the details about what is due is fuzzy, it makes it harder to code it.  
If some behavior is over-specified, then a producer does not have enough flexibility  
ex. Can limit the program later, or make it too difficult to follow. 
To assess a specification read it adversarially  
Look for exploits, look for loop holes

Examples support a specification, but cannot be a specification  
Examples clarify intent  
Examples cannot cover all cases, so they cannot replace a more general specification  
If an example is removed, the program should still work the same way per the specification  
Try to make an example that breaks the specification, then refine the specification

Specification is a prose document, but can contain formal elements  
A grammar is a common element  
Also can be sequence diagrams, flow charts, etc.  
Use standard notation and terminology as much as possible. Avoid pseudocode 