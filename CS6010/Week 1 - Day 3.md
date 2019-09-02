## Week 1 - Day 3
### If Statements
Programs become more complicated as things become conditional on current data.  
Different instructions are needed for different actions.  

**If Statements** let us control the flow of instructions.  
If statements can be nested within each other  

```
If(boolean expression: true or false) {
    The “body”: Code to execute if boolean is true
}
```
Basically if 'condition' is true, do this  

Boolean variables are always true/false  
integer >= # is a boolean statement  

To see if something is equal to another ```==```  
To see if something is *not* equal to another ```!=```  

Cannot be "h" == 'h' as it is comparing a string vs a character  
Comparing strings is done in *dictionary* order.

You can have multiple booleans in an if statement  
```&&``` 'and' if both conditions are true  
```||``` 'or' if one or other other condition is true  
```!=``` exclusive 'or'. one has to be true, the other false, not both  
  
  
|Y|X|!X|X&&Y|XllY|X!=Y|
|:---:|:---:|:---:|:---:|:---:|:---:|
|T|T|F|T|T|F|
|T|F|T|F|T|T|
|F|T|F|F|T|T|
|F|F|T|F|F|F|

##### Morgan's Rule
```!(x&&y) == (!x || !y)```  
```!(x||y) == (!x && !y)```  

If we want to display something as one or the other we use if/else statements

```
Char letterGrade = ?; 
A = hooray
B = nice
C = uhoh
Otherwise -> you’re grounded

If(letterGrade == 'A') {
    Cout << “Hooray”;
} else { // Didn’t get an A
    If(letterGrade == 'B'){
        Cout << “Nice”;
    } else { // Didn’t get an A or a B
        If(letterGrade == 'C'){
            Cout << “uhoh";
        } else { // Didn’t get an A, B, or C
            Cout << “You’re grounded”;
        };
    };
};
```

This can be refractored:  

```
If(letterGrade == ‘A’) {  
    Cout << “Hooray”;  
} else If(letterGrade == ‘B’) {  
    Cout << “Nice”;  
} else if(letterGrade == ‘C’ {  
    Cout << “uhoh”;  
} else {  
    Cout << “you’re grounded”;  
};  
```

### Loops
Sometimes we need to repeat the same code multiple times.  
We 'loop' through the same code over and over.  
We can get stuck in an infinite loop.  
We have to create a "catch" to make the condition false.  

Loops operate while a condition is true. C++ has while loops and for loops.  

For Loops are more common but more confusing syntax.  

```
while(boolean){
	// body of code
}
```

Xcode has a stop function for infinite loops  
'control - c' in terminal to stop  
While loops are error prone, needs the stop inside the body of code. For loops are better.  

```
For(initialization; condition; update){
	// body of code
} 
```

Typically seen as:  

```
for(int i = 0, i < loop amount, i++){
	// body of code
}
```

Flow is initialization -> condition -> body -> update -> condition.  

Do while loop is a thing to ensure hitting loop at least once  

```
Do {
Code  body
} while(cond);
```

Variables only "live" inside the {} they are found within. Once a closing bracket is found, the variable dies.  
As such, variables can live just within the if/else statement, or loops. 

Exception ia for( int i=0, I<10, I++) counts as within brackets  

**Variables should have the shortest life possible to clear out memory stack. Less errors to occur the short the life is.**  