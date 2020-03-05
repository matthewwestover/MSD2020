## Week 1 - Day 2
### Parsing
Can split the calc for addition and multiplication into a main driver calc function, a calc_expr and a calc_addend based on the provided grammar.  
Also write a number or paren

Calc expr calls calc addend

```
calc_expr = 
Num = calc_addend 
peek == + num += calc_expr

Calc_addend
```

If not static include them in the header file. 

```
<expr> = <number> 
| <expr> + <expr>
| <expr> * <expr>
```

Instead of parsing to return a full number we want to generate a tree that breaks apart the incoming bits. This lets us introduce variables to the calculator that are only established after compiler.  
The calculator is an “interpreter” while the parser stores the entire set of data coming in

When fixing bugs write a test that fails due to the bug. Fixing the bug makes it pass the test

```
Class Expr {
    Public:
    Virtual bool equals(Expr *e) = 0; //This is for test purposes. Lets us say one expr == expr
    // virtual means let me redeclare this in a subset class
}; 
Class Number: Public Expr {
    Public: 
    int val: 
    Number(int val) {
        This->val = val;
    }
    Bool equal(Expr *e) {
        Number *n = dynamic_cast<Number*>(e); //tell me if this is a Number. If it isn’t it returns NULL
        If(n == NULL)
            Return false;
        Else
            Return val == n->val
    }
};
Class Add: Public Expr {
    Public:
    Expr *lhs;
    Expr *rhs;
    Add(expr *lhs, expr *rhs){
        This->lhs = lhs;
        This->rhs = rhs;
    }
    Bool equals(Expr *e) {
        Add *a = dynamic_cast<Add*>(e); //tell me if this is a Number. If it isn’t it returns NULL
        If(a == NULL)
            Return false;
        else
            Return(lhs->equals(a->lhs) && rhs->equals(a->rhs));
    }
};
Class Mult: Public Expr{
    Public:
    Expr *lhs;
    Expr *rhs;
    Mult(expr *lhs, expr *rhs){
        This->lhs = lhs;
        This->rhs = rhs;
    }
    Bool equals(Expr *e) {
        Mult *m = dynamic_cast<Mult*>(e); //tell me if this is a Number. If it isn’t it returns NULL
        If(m == NULL)
            Return false;
        else
            Return(lhs->equals(m->lhs) && rhs->equals(m->rhs));
    }
};
```

42 => new Number(42)  
42+2 => new Add(new Number(42), new Number(2));  
((42)+2) => new Add(new Number(42), new Number(2));  

new Number(5)->equals(new Number(5))  

Tests should cover every classes, with multiple attempts on each “method” returning different options (true and false, etc.)  
You want to cover every “path” possible  
There are tools that help do this

Implement: Expr *parse(std::istream &in);  
Test like: CHECK( parse_str(“42+2) -> equals(new Add(new Number(42), new Number(2))) );