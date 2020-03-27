## Week 8 - Day 1
### Refactoring
Is not re-writing. It is ensuring one then that currently works, works more effectively  
Still run tests to verify each change doesn’t break things  
Basically rearrange the internals, but the outside of the program should remain the same  
Debugging, adding new features, etc isn’t refactoring  
Make sure to save/back up program prior to refactoring  
Each refactor should be a small fix, then commit, test  
Then move into other refactors.  
Some refactors require refactoring other things. Still focus on small changes  
Don’t be afraid to take bad code that works and make it better. Though sometimes its better to just document. The more recent the code is, the better it is to just rewrite to fix. 

### Allocation
Memory cares about allocation of new variables  
The interp uses as much memory as it needs. But we aren’t doing anything to free up unneeded stuff  
We are using a lot of memory for “big” programs  
Fib 28 takes over a gig of ram  
Mac can run ```leaks -atExit — program url```  
Need to set up leaks for it to work, code has to be in a lib folder.

```ln -s $/Applications/Xcode.app/Contents/Developer/usr/lib/libLeaksAtExit.dylib /usr/local/lib```

Fib leaks with and AddExpr and two NumExprs  
We can use deconstructors to call delete on lhs and rhs  
Everything has to be owned by one object. Should not return “this” as it creates a memory issue with the delete command  
Subst return this breaks need to add a clone() for the body. But that is tedious 

To share we can do reference counting  
Keep track of an object and how many things own it.  
This means we CAN return this; on objects instead of making a new one.  

```
Int refcount;
Void ref() {refcount++};
Void unref() {
    Refcount—
    If refcount == 0 -> delete
}
```

This still means calling ref() and unref() all the time across our classes  
This makes its super buggy. Forget to unref there is a leak. Forget to ref to begin with it can’t run properly and leaks.

We want to make the creation and removal automatically.  
RAII - Resource Acquisition Is Initialization  
Take a resource and tie to the declaration of an object. As soon as an object is done get rid of it  
Can be done with an ExprBox wrapper  
It holds and Expr, and it does ref() for us. When it deconstructs it derefs()
You take an expr out of the box only when being used in that most.

This takes a lot of work to add. We have to add ExprBox, ValBox, etc and place it inside all exprs.  
Each needs a ref and unref and refcounter to deconstruct  
Must use .expr and .set to use the exprs insides the boxes

C++ has standard library support for reference counting   
```Std::shared_ptr<T>``` - type of a box that holds a T\*, can be used like T\*. Use as a type instead of a T\*.  
Can replace all \*Expr with ```std::shared_ptr<Type>```  
```Std::make_Shared<T>(arg,…,arg)```  
Creates a box that holds a new T with the given args instead of new T (arg,…,arg)  
These do not need a refcount field cause it is stored in std::shared_ptr. 
Casting shared pointers for the equals()s requires ```dynamic_pointer_cast<T>(e)``` instead of ```dynamic_cast(T*)(e)```

Replacing (subst) cannot do a return this or just wrapping a shared_ptr around this  
You have to use a subclass on Exprs that enables the shared_from_this() method to return in place of “this”. 

We have to change locally a bunch of things
T \* to ```std::shared_ptr<T>```
New T(arg) to ```std::make_shared<T>(arg)```
etc. 

This is a lot of work
Can convert things via a macro

```
#if 1
# define NEW(T) new T // change all new in code to NEW(T) and it will automatically place new
# define PTR(T) 
```

Conversions via macros  
Less verbose  
Can be incremental conversion  
Supports future changes  
However, language becomes a variant of C++. 

This all greatly lowers memory usage of the program. 
However malloc and free are costly so it slows it down. 