## Week 5 - Day 1
### HW 3 Feedback for MSDScript
Call it an interpreter not a parser  
Dont mutate this should return new object  

Subst shouldn’t call optimize  
Subst shouldn’t call interp  
Interp should't call optimize  
Interp and optimize should call subst  

Parse shouldn’t call any of these, they shouldn’t call parse

Specific Suggestions:

```
`substitute`: `7` for `y` in `_let y = y _in y`, which should produce `_let y = 7 _in y`. 
`optimize`: `_let x = 1 _in 2` should simplify to just `2`. 

`AddExpr::optimize` and `MultExpr::optimize`, it's better to use `olhs` instead of calling `lhs->optimize()` a second time, and ditto for `orhs`.

Numbers aren’t truthy. Error out in is_true()
Comparisons return _true, _false
5 == 5 = _true
_true == _false = _false
_true == 5 = _false
2 == 1 + 1 = _true // technically a NumVal == AddExpr, but it should return as the value of both sides
```

### Updated Grammar

```
<expr> =
<number>
| ( <expr> )
| <expr> == <expr> // lower precedence than + or * handled by parser
| <expr> + <expr>
| <expr> * <expr>
| <variable>
| _let <variable> = <expr> _in <expr>
| _true
| _false
| _if <expr> _then <expr> _else <expr>
```

### Parsing With Conditions and Comparisons

```
<expr> = <comparg>
| <comparg> == <expr>

<comparg> = <addend>
| <addend> + <comparg>

<addend> = <inner>
| <inner> * <addend>

<inner> = <number
| ( <expr> )
| <variable>
| _let <variable> = <expr> _in <expr>
| _true
| _false
| _if <expr> _then <expr> _else <expr>
```

### Defensive Programming
Should return errors even on crash to let the user know what happened.  
Raise exception, catch exception - let user know - exit gracefully.  
Can think of exceptions as errors on a simple level  
Generally don’t want to substitute values in to avoid errors, sometimes it is necessary.  
Part of an always running program/script - like javascript  
Assertions can help restrict and find errors  
Ex assert(next_ch == expect)  
It can be left in the code even in production. Removing it shouldn’t break the program either  
Lets you verify locally if code is doing what it should be

It is tempting to try-catch without doing anything in catch. Catching should at least print an error about an issue arising.  
Catching an exception should only occur in small chunks so there aren’t overlap of multiple things throwing the same thing caught the same way. 

Parser part of the script should be very defensive. It accepts any random characters  
It shouldn’t just crash - random testing can be helpful to check this. 
Pre/post conditions are good