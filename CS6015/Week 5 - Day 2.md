## Week 5 - Day 2
### Functions
Functions can be a value - this is done in Javascript  
They don’t have to be named (works like a lambda)  
Var my_function = Function (x) { return x +1 }  
[1,2,3].map(my_function) = [2,3,4]  
Can be done inline  
[1,2,3].map(function(x) { return x + 3 }) = [4,5,6]

(Function (x) { return x + 1})(14) = 15

Function takes argument and does this without being defined as a name. Can be stored as a variable

### MSDScript

```
_let f = _fun (x) x + 1
_in f(10)
11
```

Grammar adds this as  

```
<expr> = ...  
| _fun (<variable>) <expr> -> FunExpr // variable can be stored as a string not an VarExpr  
| <expr> ( <expr> ) -> CallExpr
```

Defining a function is the formal argument  
Calling a function is the actual argument  

```
<expr> ( <expr> ) -> in parsing the first <expr> does not have to be a <var> it can be any <expr>
```

This also means we need a new kind of value. _fun (x) x + 1 doesn’t store as a NumVal or BoolVal

We need FunVal  
When printed as a value it should come out as [function]  
It should not show what the function does

```
<val> = <number>  
| <boolean>  
| _fun ( <variable> ) <expr>
```

FunVal stores like FunExpr formal argument (can be a string) and a Expr body

```
Val FunExpr::interp() {
    New FunVal(formal_arg, body);
}

_fun (x) x + 1
-> returns [function]

_let _fun (x) x + 1 //becomes a value
_in f(10) // value gets substituted back to calculate

(_fun (x) x + 1) (10) -> 11

Val CallExpr::interp() {
    To_be_called->interp()->call(Actual_argument->interp())
}

Class Value {
    Virtual Val *call(val *actual_arg) = 0; // use subst in implementation
}
```

### Updated Grammar

```
<expr> =
<number>
| ( <expr> )
| <expr> == <expr> // lower precedence than + or * handled by parser
| <expr> + <expr>
| <expr> * <expr>
| <expr> ( <expr> ) // higher precedence than multi. This is the call expr. This should be left-associative 
| <variable>
| _let <variable> = <expr> _in <expr>
| _true
| _false
| _if <expr> _then <expr> _else <expr>
| _fun ( <variable> ) <expr>
```

### Parsing With Conditions and Comparisons

```
<expr> = <comparg>
| <comparg> == <expr>

<comparg> = <addend>
| <addend> + <comparg>

<addend> = <multicand>
| <multicand> * <addend>

<multicand> = <inner>
| <multicand> ( <expr> )

<inner> = <number
| ( <expr> )
| <variable>
| _let <variable> = <expr> _in <expr>
| _true
| _false
| _if <expr> _then <expr> _else <expr>
| _fun ( <variable> ) <expr>
```

Sudocode: 

```
parse_multicand() {
    Expr = parse_inner()
    While (peek() == ‘(‘) {
        Actual_arg = parse_expr()
        Consume )
        Expr = new CallExpr (expr, actual_arg)
    }
    Return expr;
}
```


