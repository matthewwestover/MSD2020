## Week 3 - Day 2
### Testing
SQLite has 662 times as many lines of testing vs coding  
Regression - keep old tests to ensure once something is fixed it didn’t break anything. Also make sure to add a test case to reproduce the error so it can be continually tested for going for  

Bug occurs -> Create Test -> Fail Test -> Fix Bug -> Pass Test  

Tests are a permanent part of the code base  

Unit tests cover what might go wrong. This is what we have written in our current file set ups  
These tend to be white box clean tests, things we can thing about / write to test

Generated tests can find things you didn’t thing about. This automatically runs the program with many inputs

Tests generation needs:  
A way to generate inputs  
A driver to send the inputs and receive outputs  
A way to decide whether the output are good

Fuzzing:  
Sending randomly generated inputs - typically text  
Minimal result validation - did it crash?  
Throw random crap at the program and see what it does  
This won’t generate valid inputs however  
One bag of tricks to use

The generated text needs to generate things along the lines of the grammar  
Randomly pick a case  
If it is a number - randomly pick a number  
For variable generate a random one - in let it would be difficult to generate randomly the same on both sides  
For others recure for nested   

Set all numbers to 0 any output should be 0  
Generate the expression simplify and feed it back in, should return same result  
Mangle the string to get slightly different but both passable and non-passable input

Exec_program - takes an array of strings and the input strings  
Returns an exit code, the output, the error strings