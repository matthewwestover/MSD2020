## Week 2 - Day 2
### Method/Function Good Practices
Parameters should have an order they are passed in  
Input > Modify > Output > Errors  
Too many arguments often means we should have created an object that keeps those parameters together 
Clear names  
Have “routines” do one thing specifically  
Global variables are bad  
Changing input variables is bad  
Dont use macros if you dont have to - telling you the language isn’t doing something for you  
Example: #define A(x) x++  
Call A(w) does x++  
This is an issue. Textural substitution is bad. A(w+z) becomes w+z++  
There are good reasons for using macros. We have used some such as catch2 TEST_CASE

How to organize code in C++  
Using the calc parser as an example  
Put classes into a new .cpp and .hpp files    
Include the header in the files that need the classes  
Headers dont have implementation. Only the signatures (about half of the expression code)  
The class .cpp gets things like Class::Class constructors and Class:method implementation  
Main.cpp can have #catch_config_runner and call the run catch in main(). This means the test cases in all other cpp files get run  
Input std::cin uses control d as end of file to exit

```
New number(10) -> subst(“dog”, 3)-> equals->
f(3) = 10 = -> new Number (10)
New Variable(“fish”) -> subst(“dog”, 3)->equals(new Variable(“fish”)
New variable(“dog” -> subst(“dog”,3) -> equals(new Number(3)
New Add(new Number(2), new Variable(“dog”) -> subst(“dog”,3)->equals(new Add(new Number(2), new Number(3))
```