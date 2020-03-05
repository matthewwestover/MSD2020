## Week 6 - Day 1
### Debugging
Learn to debug by debugging a lot  
Use commit messages to be specific about changes  
Commit for changing small chunks  
It can then be used as a note about bugs encountered  
Regression testing - find a bug write a test to generate that bug consistently, then becomes part of test suite  
Rubber duck - saying it out loud helps explain it - reminds you had issues like this before so you can check your git messages  
Lint - checks for formatting 

### MSDScript


f(x) = x*x  
f(2)  
F is bound for the whole slide  
x is bound for the context it is used  

Our notation is 
```_let f = _fun (x) x*x _ in f(2) = 4```
Parser runs and creates a LetExpr with a FunExpr and runs it _in with a CallExpr

CallExpr needs to call subst to result in a ```(_fun (x) x*x) (2)``` -> type this in the program also should do this
```->call() -> 2*2```

Y = 8
f(x) = x*y
f(2) = 16

```
_let y = 8 
_in _let f = _fun (x) x*y
_in f(2)
Parse creates a LetExpr with a LetExpr with a FunExpr
This runs a subst where there is a LetExpr with a FunExpr that can run to a CallExpr
_let f = _fun (x) x*8
_in f(2)
(_fun (x) x*8) (2)
2*8 = 16
```

Interp calls substs. Optimize never calls substs
Optimize don’t try to call it even if the argument is there

### Local Binding vs Functions

```
_let x = 1
_in x+2
```

```(_fun (x) x+2) (1)``` 
Both reduce to 1+2
Functions can do something that Let cannot do. Functions can be based as a variable to be used elsewhere. 
Any \_let can be done as a function
```_let <var> = <rhs.expr> _in <body.expr> always = (_function(<var>) <body.expr>) (<rhs.expr>)```

Let is just easier to read. Let is now redundant in our language but it is just convenient
Let could even be removed at the parsing level - very deep level - a “macro”

### Multiple Arguments vs Currying

```
f(x, y) = x*x + y*y
f(2,3)
_let f = _fun (x) _fun (y) x*x + y*y
_in f (2) (3)
```

Nested arguments becomes a function inside a function and a call by a call

```
_let f = (_fun (x)
    (_fun (y)
        x*x + y*y))
_in (f(2)) (3)
```

```
((_fun (x)
    (_fun (y)
        x*x + y*y)) (2)) (3)
(_fun (y)
    2*2 + y*y) (3)
2*2 + 3*3
13
```

### Partial Application

```
_let add = _fun (x)
    _fun (y)
        x+y
_in _let addFive = add(5)
_in addFive(10)

_let addFIve = _fun (y)
    5 + y
In addFive(10)
```

Even with nested functions we can call one then the other later. In this case it solved X then solved y. 

### Recursive Functions

```
_let factorial = _fun (x)
    _if x == 1
    _then 1
    _else x * factorial(x+ -1) // factorial is only visible after the _in. This breaks
_in factorial (5) //factorial is visible here to be used
```

For this to work we need to hand factorial to factorial to work

```
_let factrl = _fun (factrl)
    _fun (x)
        _if x == 1
        _then 1
        _else x * (factrl(factrl) (x + -1)
_in factrl(factrl) (5) 
```

Can become \_in \_let factorial = \_fun (x)
    factrl(factrl) (x)
\_in factorial (5)

Can become \_in \_let factorial =  factrl(factrl) (x)
\_in factorial (5)

