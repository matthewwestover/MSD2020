## Week 8 - Day 2
### Evaluation and Substitution
```
_let x1 = 1
_in _let x2 = 2
_in _let x3 = 3
…
_in _let x100 = 100
_in x1 + x2 … + x100
```
This turns into

```
_let x2 = 2 
_in 1 + x2 ...
```

This repeats over and over creating 100^2 work as it needs to generate a new let for each iteration of xX

###Separate Variable Dictionary
```
_let x1 = 1 -> adds to dictionary x1 = 1
_in _let x2 = 2 -> adds to dictionary x2 = 2
_in _let x3 = 3 -> adds to dictionary x3 = 3
…
_in _let x100 = 100 -> adds to dictionary x100 = 100
_in x1 + x2 … + x100 -> looks in dictionary to say oh x1 = 1 and instantly go through
```

#### Just One Dictionary?
```
_let x = 1 -> adds x = 1
_in _let x = 2 -> adds x = 2
_in x -> expect to get 2
```
Adding to the dictionary either needs to add and replace from the top, or replace the original value  
Dictionary x = 2, x = 1 -> stop looking at 2

This was the common line of thinking from the 60s - mid-70s

```
_let x = 1
_in (_let x = 2 _in x) + x // first x is 2, second is 1 = 3

Dictionary
X = 2
X = 1


(_let x = 2 _in x) + x
-> (x) + x
_. (2) + x
-> 2 + 2 // still sees X = 2 at top of the dictionary
-> 4
```

This breaks because it applies everywhere. When we subst we do it within the body.  
Substitution dictionaries need to be built per body/expression, not one for everything

Pair an expression and a dictionary. A pair is called a closure  
The dictionary itself is called an environment.  
As new environments are created, add it to the front of a linked list. 

With environments

```
_let x = 1 -> creates environment with x = 1
_in (_let x = 2 _in x) + x -> this splits
(_let x = 2 _in x) gets environment with x = 2. The + x still only links with x = 1
```

To get this to work create an Env class  
EmptyEnv and ExtendedEnv. 

Envs are ONLY used by interp. Interp stops using subst. Opt still needs subst  
Mostly avoids the need to allocate closures  
There is an issue with interp on funexpr  
Body can lose its link to an Env  
FunVals need to have the Env included