## Week 4 - Day 1
Currently Parsing involves just ints, and the expressions use to_value, simplify, subst (which helps to_value, and simplify)  
We want to expand int to be any Value - ints/values  
To_value becomes interp because its not just returning an int value  
Simplify becomes optimize  
Eventual goal is to make this more a programming language not just a math calculator

It is possible in Xcode to refactor -> rename to updated methods  
Refactoring is a higher level thinking not just “fixing”

Parse vs interpreter vs optimizer  
Parse - must succeed on any input that fits grammar, must error any input that doesn’t fit  
To_value/interp/evaluate - must produce a simple value, might report an error, might not complete  
Simplify/optimize - never reports an error, always competes, reset has the same interp behavior, but might be faster

```
_let x = 3 _in x
3 is both the interp and optimize result

x
Interp gives an error, no longer returns as 0 for an undefined var
Optimize returns just VarExpr(x)

x + ( 8 * 9)
Interp gives an error
Optimize returns x + 72

_let y = 3 _in x + y
Interp gives an error
Optimize returns x + 3

_if 1+2 == 3
_then 100
_else 0
Interp returns 100

_let double = _fun (x) x + x
_in double(4) + double(10)
Interp return 28

_let fib = _fun (fib)
    _fun (x)
        _if x == 0
        _then 1
        _else _if x == 1
        _then 1
        _ else fib(fib) (x +-1)
        + fib(fib) (x +-2)
_in fib(fib)(10)
Interp returns 89

_let slow = _fun (x) …
_in _let = slow(1) + slow(2)
_in y*y

BoolVal::add_to{ throw std::runtime_error(“cannot add booleans”);

Val *rhs_val = rhs_interp();
Expr * new_body = body->subst(name, rhs_val)
Return new_body ->Interpol();
```