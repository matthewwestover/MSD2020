## Week 3 - Day 1
### Test Coverage
**Function** - every function called (Not always taking every path through a function)  
**Line** - every line reached (Lines can be complicated to hit all of them)  
**Statement/Expression** - each statement/expression reached  
**Branch** - every side of a conditional taken (can force you to loop through a do/while loop)  
**Decision** - all functions + all branches  
**Condition** - each boolean true and false  
**Value** - each (common) value at every possibility (not practical to do ALL values)  
**Edge** - each control transfer taken (jump from this part to this part)  
**Path** - each control combination taken (try all possible combinations of the branches)  
**Modified condition/decision** - each boolean matters. 

### Parsing

```
_let x = 5  _in x+1 = 6
(_let x =5 _in x+1) * 2 = 12
_let x =5 _in x + 1 * 2 = 7

<expr> = <addend>
    | <addend> + <expr>

<addend> = <inner>
    | <inner> * <addend>

<inner> = <number>
    | ( <expr> )
    | <variable>
    | _let <variable> = <expr> _in <expr>

_let x = 5
_in _let x = 6
    _in x + 1 = 7
```

This is lexical scope. 
Substitution of variable with number at _let:  
Same variable: don’t substitute in the body  
Different variable: substitute in the body. 

```
_let x = 5
_in _let x = x+2
    _in x + 1 = 8
```

Substitute on the right hand side, this becomes let x = 5+2 in x+1
This substitute in the rhs 

### Booleans
\_true
\_false

```
Parse_str(“_true”)->equals(new Boolean(true)) );
Parse_str(“_false”)->equals(new Boolean(false)) );
Parse_str(“_true+1”)->equals(new Add(new Boolean(true), new Number(1))) ); // This has no type checking

CHECK( (new Boolean(true))->value() == ???);
```

Values can now be a number or a boolean. We need a parent value class with subclasses for ints or booleans

```
Class Value { }

Class NumVal : public Value {
    Int val
}

Class BoolVal : public Value {
    Bool val;
}

Class Expr {
    virtual Value *value() = 0;
}

CHECK( (new Boolean(true))->value() == new BoolValue(true));
```

```
Value *Add::value() {
    Return lhs->value()->add_to(rhs->value());
}

Class Value{
    Virtual bool equals (Value *val) = 0;

    Virtual Value *add_to(Value *other_val) = 0
}

Value *NumVal::add_to(Value * other_val) {
    Numval *nv = dynamic_cast<NumVal*>(other_val;
    If (nv == nullptr)
        Throw std::runtime_error((std::string)”not a number);
    else
    Return new NumVal(rep + nv->rep);
}


Value *BoolVal::add_to(Value * other_val) {
    Throw std::runtime_error((std::string)”bools cant add");
}

Class BoolVal : public Value {
    Bool val;
}
```