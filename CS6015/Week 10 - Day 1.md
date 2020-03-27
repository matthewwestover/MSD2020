## Week 10 - Day 1
Loops are complicated  
MSDScript can basically do recursive calling to “loop” through code.  
However we quickly can hit a stack overflow   

```
_let countdown = _fun(countdown) _fun(n)
_if n == 0
_then 0
_else countdown(countdown)(n + -1)
_in countdown(countdown)(1000000)
```
```
_let count = _fun(count) _fun(n)
_if n == 0
_then 0
_else 1 + count(count)(n + -1)
_in count(count)(100000)
```
C and C++ has some tail-end optimization which is unreliable to help with this. We need to code a fix.  
Basically MSDScript stack needs one frame for each of the levels of recursion

We want function calls to work in loops like algebra-style simplification  
So countdown(countdown)(N) runs in constant space for any sized N  
We also want to use all available memory not just the available stack-memory  
To do this we need to make it so ->interp() doesn’t call recursively

Currently intep works like this

```
PTR(Val) AddExpr::interp(PTR(Env) env) {
return lhs->interp(env)->add_to(rhs->interp(env));
}
```

The order of this is lhs->interp(env), rhs->interp(env) then finally the ->add_to

This can be updated to show this by:

```
PTR(Val) AddExpr::interp(PTR(Env) env) { 
PTR(Val) lhs_val = lhs->interp(env); 
PTR(Val) rhs_val = rhs->interp(env); 
return lhs_val->add_to(rhs_val);
}
```

This works by having “steps”

```
class Step {
  typedef enum {
interp_mode,
continue_mode
} mode_t;
static mode_t mode;
  static PTR(Expr) expr;
  static PTR(Env)  env;
  static PTR(Val)  val;
  static PTR(Cont) cont; /* all modes */
};
```
```
PTR(Val) interp_by_steps(PTR(Expr) e) { Step::mode = Step::interp_mode; Step::expr = e;
Step::env = Env::empty;
  Step::val = nullptr;
  Step::cont = Cont::done;
while (1) {
if (Step::mode == Step::interp_mode)
Step::expr->step_interp(); else {
      if (Step::cont == Cont::done)
        return Step::val;
else
Step::cont->step_continue(); }
} }
```

Step_interp and step_continue aren’t returning anything, they change the global variable values.  
Typically that is bad practice, but is needed for this type of application

Every Expr interp call becomes a step_interp(), and requires multiple variants of the Cont class.  
Cont class always uses a step_continue()

step_interp and step_continue should never call

* step_interp
* step_continue
* interp_by_steps
* interp
* optimize
* subst

interp_by_steps should be the only thing to call step_interp and step_continue

```
NumExpr ⇒ continue
BoolExpr ⇒ continue
VarExpr ⇒ continue 
FunExpr ⇒ continue
AddExpr ⇒ RightThenAddCont AddCont
MultExpr ⇒ RightThenMultCont MultCont
CompExpr ⇒ RightThenCompCont CompCont
CallExpr ⇒ ArgThenCallCont CallCont
IfExpr ⇒ IfBranchCont
LetExpr ⇒ LetBodyCont
```