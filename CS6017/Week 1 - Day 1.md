## Week 1 - Day 1
### Installation
```
#install python3
$brew install python3
#for any other packages we use (e.g. notebook pandas)
$/usr/local/bin/pip3 install jupyter
$/usr/local/bin/pip3 install pandas
```

To run Jupyter Notebook run ```jupyter-notebook```

Jupyter blends markdown for text and python cells that can be run. 

### Notebooks and Python Basics

Welcome to your first Jupyter notebook! This will be our main working environment for this class.
These notes, and many others are adapted from the COMP 5360 Data Science course.

### Jupyter Notebook Basics

First, let's get familiar with Jupyter Notebooks. 

Notebooks are made up of "cells" that can contain text or code. Notebooks also show you output of the code right below a code cell. These words are written in a text cell using a simple formatting dialect called [markdown](http://jupyter-notebook.readthedocs.io/en/latest/examples/Notebook/Working%20With%20Markdown%20Cells.html). 

Double click on this cell text or press enter while the cell is selected to see how it is formatted and change it. We can make words *italic* or **bold** or add [links](http://datasciencecourse.net) or include pictures:

The content of the notebook, as you edit in your browser, is written to the `.ipynb` file we provided. 

If you want to read up on Notebooks in details check out the [excellent documentation](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

The most interesting aspect of notebooks, however, is that we can write code in the cells cells. You can use [many different programming languages](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) in Jupyter notebooks, but we'll stick to Python. So, let's try it out:


```python
print ("Hello World!")
a = 3
# the return value of the last line of a cell is the output
a;
```

    Hello World!



```python
a = 5
```

Again, we've greeted the world out there using a print statement. 

We also assigned a variable and returned it, which makes it the output of this cell. Notice that the output here is directly written into the notebook. 

You can change something in a cell and re-run it using the "run cell" button in the toolbar. 

Another cool thing about cells is that they preserve the state of what happened before. Let's initialize a couple of variables in the next cell: 


```python
age = 2
gender = "female"
name = "Datascience Cat"
smart = True
```

These variables are now available to all cell below or above **if you executed the cell**. In practice, you should never rely on a variable from a lower cell in an earlier cell. 

If you make a change to a cell, you need to execute it again. You can also batch-executed multiple cells using the "Cell" menue in the toolbar. 

Let's do something with the variables we just defined:


```python
print (name + ", age: " + str(age) + ", " + 
       gender + ", is smart: " + str(smart))
```

    Datascience Cat, age: 2, female, is smart: True


In the previous cell, we've [concatenated a couple of strings](https://docs.python.org/3.5/tutorial/introduction.html#strings) to produce one longer string using the `+` operator. Also, we had to call the `str()` function to get [string representations of these variables](https://docs.python.org/3.5/library/stdtypes.html#str).


```python
print (name, ", age: ", str(age), ", ", gender, ", is smart: " + str(smart), sep="")
```

    Datascience Cat, age: 2, female, is smart: True


#### Modes

Notebooks have two modes, a **command mode** and **edit mode**. You can see which mode you're in by the color of the cell: green means edit mode, blue means command mode. Many operations depend on your mode. You can switch into edit mode with "Enter", and get out of it with "Escape". 

#### Shortucts

While you can always use the tool-bar above, you'll be much more efficient if you use a couple of shortcuts. The most important ones are:

**`Ctrl+Enter`** runs the current cell.  
**`Shift+Enter`** runs the current cell and jumps to the next cell.   
**`Alt+Enter`** runs the cell and adds a new one below it.

In command mode:
**`h`** shows a help menu with all these commands.  
**`a`** adds a cell before the current cell.  
**`b`** adds a cell after the current cell.  
**`dd`** deletes a cell.  
**`m`** as in **m**markdown, switches a cell to markdown mode.  
**`y`** as in p**y**thon switches a cell to code.  

**Take a couple minutes to flip through the help->keyboard shortcuts menu (or press "h" in command mode).  It's well worth your time**

#### Kernels

When you [run code](http://jupyter-notebook.readthedocs.io/en/latest/examples/Notebook/Running%20Code.html), the code is actually executed in a **kernel**. You can do bad thinks to a kernel: you can make it stuck in an endless loop, crash it, corrupt it, etc. And you probably will do all of these things :). So sometimes you might have to interrupt your kernel or restart it. Use the "Kernel" menu to restart the kernel, re-run your notebook, etc.  We'll be using Python mostly for this class, but you can actually install Kernels for a bunch of different languages [see here](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels).

Also, before submitting a homework or a project, make sure to `Restart and Run All`. This will create a clean run of your project, without any side effects that you might encounter during development. We want you to submit the homeworks **with output**, and by doing that you will make sure that we actually can also execute your code properly.

#### Storing Output

Notebooks contain both, the input to a computation and the outputs. If you run a notebook, all the outputs generated by the code cells are also stored in the notebook. That way, you can look at notebooks also in non-interactive environments, like on [GitHub for this notebook](https://github.com/datascience-course/2018-datascience-lectures/blob/master/02-basic-python/lecture-02-notebook.ipynb). 

The Notebook itself is stored in a rather ugly format containing the text, code, and the output. This can sometimes be challenging when working with version control.

##### Exercise: Creating Cells, Executing Code

1. Create a new code cell below where you define variables containing your name, your age in years, and your major.
2. Create another cell that uses these variables and prints a concatenated string stating your name, major, and your age in years, months, and days (assuming today is your birthday ;)). The output should look like that:

```
Name: Science Cat, Major: Computer Science, Age: 94 years, or 1128 months, or 34310 days. 
```


```python
name = "Matt"
major = "MSD"
age = 30
months = age * 12
days = age * 365
print('Name: ' + name + ', Major: ' + major + ', Age: ' + str(age) + ' years, or ' + str(months) + ' months, or ' + str(days) + ' days.'  )
    
```

    Name: Matt, Major: MSD, Age: 30 years, or 360 months, or 10950 days.



```python
#To turn off auto closing quotes/parens, run this cell
from notebook.services.config import ConfigManager
c = ConfigManager()
c.update('notebook', {"CodeCell": {"cm_config": {"autoCloseBrackets": False}}})

```




    {'CodeCell': {'cm_config': {'autoCloseBrackets': False}}}



### Python Basics

#### Functions

In math, functions transfrom an input to an output as defined by the property of the function. 

You probably remember functions defined like this

$f(x) = x^2 + 3$

In programming, functions can do exactly this, but are also used to execute "subroutines", i.e., to execute pieces of code in various order and under various conditions. Functions in programming are very important for structuring and modularizing code. In computer science, functions are also called "procedures" and "methods" (there are subtle distinctions, but nothing we need to worry about at this time). The following Python function, for example, provides the output of the above defined function for every valid input: 



```python
def f(x):
    result = x ** 2 + 3 
    return result
```

We can now run this function with multiple input values: 


```python
print(f(2))
print(f(3))
f(5)
```

    7
    12





    28



Let's take a look at this function. The first line
```python
def f(x):
```
defines the function of name `f` using the `def` keyword. The name we use (`f` here) is largely arbitrary, but following good software engineering practices it should be something meaningful. So instead of `f`, **`square_plus_three` would be a better function name in this case**.  

After the function name follows a list of parameters, in parantheses. In this case we define that the function takes only one parameter, `x`, but we could also define multiple parameters like this:
```python 
def f(x, y, z):
```

The parameters are then available as local variables within the function.

The second line does the actual computation and assigns it to a **local variable** called `result`. 

The third line uses the `return` keyword to return the result variable. Functions have a return value, that we can assign to a variable. For example, here we could write: 

```python
my_result = f(10)
``` 

Which would assign the return value of the function to the variable `my_result`.

Note that the lines of code that belong to a function are **indented by four spaces** (you can hit tab to intend, but it will be converted to four spaces). Python defines the scope of a function using indentation. Many other programming languages use curly brackets {} to do this. A function is ended by a new line.

For example, the same function wouldn't work like this:



```python
def f(x):
    result = x ** 2 + 3
# Throws a SyntaxError becauser return isn't defined outside the function
return result
```


      File "<ipython-input-10-e3a6854301c6>", line 4
        return result
                     ^
    SyntaxError: 'return' outside function



Equally, we can't indent by too much:


```python
def f(x):
    result = x ** 2 + 3
    # Throws an IndentationError
        return result
```

#### Scope

Another critical concept when working with functions is to understand the scope of a variable. Scope defines under which circumstances a variable is accessible. For example, in the following code snippet we cannot access the variable defined inside a function:


```python
def scope_test():
    function_scope = "only readable in here"
    # Within the function, we can use the variable we have defined
    print("Within function: " + function_scope)

# calling the function, which will print     
scope_test()
```


```python
# If we try to use the function_scope variable outside of the function, we will find that it is not defined. 
# This will throw a NameError, because Python doesn't know about that variable here
print("Outside function: " + function_scope)
```

You might wonder "Why is that? Wouldn't it make sense to have access to variables wherever I need access?". The reason for scoping is that it's simply much easier to **build reliable software when we modularize code**. When we use a function, we shouldn't have to worry about its internals. 

Another practical reason is that this way we can **re-use variable names** that were used in other places. This is really important when we work with other peoples' code (e.g., libraries). If that weren't possible, we might get nasty side-effects just because the library uses a variable with the same name somewhere. 

You can, however, use variables defined in the larger scope in the sub-scope:


```python
name = "Science Cat"

def print_name_with_dr():
    print("Dr.", name)
    
print_name_with_dr()
```

This is generally not considered good practice - functions should rely on their input parameters. Otherwise it can easily lead to side effects. This would be the better approach: 


```python
# notice that we're re-using the parameter name
def print_name_with_dr(name):
    print("Dr.", name)
    
print_name_with_dr(name)
```

Finally, there is a way to define a variable within a function for use outside its scope by using the global keyword. There are reasons to do this, however, it is generally discouraged.


```python
def scope_test():
    # Think long and hard before you do this - generally you shoudln't
    global global_scope
    global_scope = "defined in the function, global scope"
    # Within the function, we can use the variable we have defined
    print("Within function: " + global_scope)

scope_test()
# Since this is defined as global we can also print the variable here
print("Outside function: " + global_scope)
```

#### Data Types and Operators

We've already covered the basic data types and operators. Now we'll recap and go into some more details.

Also, make sure to check out the complete documentation of standard types and operations.

##### Boolean

Boolean values represent truth values True and False. Booleans can be used as any other variable:


```python
my_true_var = True
print (my_true_var)
my_false_var = False
print (my_false_var)
```

True and False are reserved keywords in their capitalized form.

There are three operations defined on booleans: and, or, and not.

Operation	Result
x or y	if x is false, then y, else x
x and y	if x is false, then x, else y
not x	if x is false, then True, else False


```python
True or False
```


```python
True and False
```


```python
not True
```


```python
not False
```

##### Comparisons

Comparisons are very important in programming: they let us decide on conditional flows, which we will discuss later. To compare two entities, Python provides eight comparison operators:

Operation	Meaning
<	strictly less than

<=	less than or equal

\>	strictly greater than

\>=	greater than or equal

==	equal

!=	not equal

is	object identity

is not	negated object identity

These operators take two operands and return a boolean. We'll glance over the last two for now, but here are some examples of the others:


```python
1 < 2 
```


```python
1 <= 1
```


```python
14 == 14
```


```python
14 != 14 
```


```python
"my text" == "my text"
```


```python
"my text" == "my other text"
```


```python
"a" > "b"
```


```python
"a" < "b"
```


```python
"aa" < "aba"
```




    True




```python
"aaa" < "aa"
```




    False



We see that the operations work on numbers just as we would expect.

Strings are also compared as we'd expect

#### Numerical Data Types

Python supports three built in data types, int, float, and complex. Since Python is dynamically typed, we don't have to define the data types explicitly!

The int data type is used to to represent integers ℤZ. Python is special in the way it handles integers as it allows arbitrarily large integers, while most other programming languages reserve a certain chunk of memory for integers, which can lead to a number "overflowing". This, for example, would not work properly in C or Java:


```python
2 ** 200
```




    1606938044258990275541962092341162602522202993782792835301376



However, we can still experience overflows in Python if we work with pandas, a library we will extensively use.

Integers can be positive, zero, or negative, as you would expect.

The float datatype is used to represent real numbers ℝR. Floats, however, can not be precisely represented by a computer. Take the example of 1/31/3. Representing 1/31/3 accurately would require the computer to store an infinitely large number of 0.33333333333333333333....0.33333333333333333333.... (if a computer used a decimal number system).

Since computers use binary numbers, also seemingly simple numbers such as 0.1 cannot be accurately represented. Check out this example:


```python
.1 + .1 + .1 == .3
```




    False



What computers do is that they store approximations using a limited chunck of memory to store the number. At the same time, Python rounds the output of numbers:


```python
1 / 10
```




    0.1



This number is in fact not 0.1 but is stored in the computer as:

0.1000000000000000055511151231257827021181583404541015625

This representation, however, is rarely useful, hence the number is rounded.

The lesson that you should remember is that you CANNOT compare two float numbers with the == operator.


```python
a = .1 + .1 + .1 
b = .3
a == b
```




    False



Instead, you can do something like this:


### Compare for equality up to a constant value
a < b + 0.00001 and a > b - 0.00001
This, of course, only compares up to the 5th digit behind the comma.

A better way to do this is the isclose function from the math package.


### this is how we import a package


```python
import math 
```

here we call the isclose function that comes with the math package. 


```python
math.isclose(a, b, rel_tol=0.00001)
```




    True



Here we've also used our first package, the package math!

Packages extend the basic functionality of python. We'll work a lot with packages in the future, details will follow.

Numerical Operators
Here is a selection of operators that work on numerical data types.

| Operation | Result
| - | - |
|`x + y`	|sum of x and y	 	 
|`x - y`	|difference of x and y	 	 
|`x * y`	|product of x and y	 	 
|`x / y`	|quotient of x and y	 	 
|`x % y`	| remainder of x / y
|`-x`	| x negated	 	 
|`abs(x)` |	absolute value or magnitude of x	 
|`int(x)` |	x converted to integer	
|`float(x)` |	x converted to floating point	
|`pow(x, y)` |	x to the power y	
| `x ** y` | x to the power y

#### 3. Conditions: if-elif-else statements

We've learned how to make comparisons between items and do boolean operations. The result of these operations was usually a boolean value. 

We can now make use of these boolean values to **steer the program flow using conditions**. 

We can do that using if statements. If conditions evaluate an arbitrary expression for its boolean value and execute one branch of code if they are true, and another branch if they are false:


```python
def isOdd(x):
    # the statement within the brackets is evaluated for truth
    if (x % 2 == 1):
        # body, executed if true
        print(str(x) + " is in fact an odd number")
    else:
        # executed if false
        print(str(x) + " is an even number")

isOdd(144)
isOdd(13)
```

    144 is an even number
    13 is in fact an odd number


Notice that the **code blocks that are intended form the "body"** of the if statement, just as it did for functions.

In addition to the explicit boolean values that we can use to test for truth, most **programming languages define a range of things to be true or false**. 

By definition, 0 of any numeric type, empty sequences or lists, `none` values, etc., are considered false. Everything else is considered true.


```python
if (0):
    print("This should never happen")
else:
    print("0 is false")

undefined_var = None
if (undefined_var):
    print("This should never happen")
else:
    print("An undefined variable is false")
    
if ([]):
    print("This should never happen")
else:
    print("An empty list is false")
```

    0 is false
    An undefined variable is false
    An empty list is false


You can also **chain conditions using the `elif` statement**, which is short for else if:


```python
def smallest_factors(x):
    # notice the use of the negation and the use of 0 as false
    if(not x % 2):
        print("2 is a factor of " + str(x))  
    elif(not x % 3):     # only evaluated when if was false
        print("3 is a factor of " + str(x))
    else: # only evaluated when both if and elif were false
        print("Neither 2 nor 3 are factors of " + str(x))

smallest_factors(4)
smallest_factors(9)
smallest_factors(12)
```

    2 is a factor of 4
    3 is a factor of 9
    2 is a factor of 12


Notice that the elif (or the else) branch is not evaluated when the if branch matches. A function that prints whether both, 2 and 3 is a factor could be written like this: 


```python
def factors(x):
    # notice the use of the negation and the use of 0 as false
    if(not x % 2):
        print("2 is a factor of " + str(x))  
    if(not x % 3):     
        print("3 is a factor of " + str(x))
    if (x % 2) and (x % 3):
        print("Neither 2 nor 3 are factors of " + str(x))

factors(4)
factors(9)
factors(12)
factors(13)
```

    2 is a factor of 4
    3 is a factor of 9
    2 is a factor of 12
    3 is a factor of 12
    Neither 2 nor 3 are factors of 13


#### 4. Lists

Up to know we've worked only with basic data types such as booleans, numbers and strings. Now we'll take a look at a compound data type: lists.

**A list is a collection of items.** Another word commonly used for a list in other programming languages is an array (though there are differences between lists and arrays in many languages). 

**Lists are created with square brackets `[]` and can be accessed via an index:**


```python
beatles = ["Paul", "John", "George", "Ringo"]
# printing the whole array
print(beatles)
# printing the first element of that array, at index 0
print(beatles[0])
# third element, at index 2
print(beatles[2])
# access the last element
print(beatles[-1])
# access the one-but-last element
print(beatles[-2])
```

    ['Paul', 'John', 'George', 'Ringo']
    Paul
    George
    Ringo
    George


If we try to address an index outside of the range of an array, we get an error: 


```python
beatles[4]
```


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-24-ea23b43f225d> in <module>
    ----> 1 beatles[4]
    

    IndexError: list index out of range


Sometimes, it makes sense to pre-initialize an array of a certain size:


```python
[0] * 10
```

There is also a handy shortcut for quickly initializing lists. This uses the [range()](https://docs.python.org/3/library/functions.html#func-range) function, which we'll explore in more detail later.

We can also create **slices of an array with the slice operator `:`**

```python
a[start:end] # items start through end-1
a[start:]    # items start through the rest of the array
a[:end]      # items from the beginning through end-1
a[:]         # a copy of the whole array
```

There is also the step value, which can be used with any of the above:

```python
a[start:end:step] # start through not past end, by step
```

See [this post](http://stackoverflow.com/questions/509211/explain-pythons-slice-notation) for a good explanation on slicing.


```python
# Get the slice from 0 (included) to 2 (excluded)
beatles[:2] # this can also be written as [0:2]
```


```python
# Sclice from index 2 (3rd element) to end
beatles[2:]
```


```python
# A copy of the array 
beatles[:]
```

The slice operations return a new array, the original array is untouched: 


```python
beatles
```


```python
beatles[-1] #last item in list
```


```python
len(beatles)
```

Slicing outside of a defined range returns an empty list:


```python
beatles[4:9]
```

Strings can be treated similar to arrays with respect to indexing and slicing:


```python
paul = "Paul McCartney"
paul[0:4]
```

Lists (in contrast to strings) are mutable. 

That means **we can change the elements that are contained in a list**: 


```python
beatles[1] = "JohnYoko"
beatles
```

This does not work with strings, strings are immutable: 


```python
# This will return an error
paul[1] = "o"
```

Arrays can also be **extended with the `append()` function**:


```python
beatles.append("George Martin")
beatles
```

Lists can be **concatenated**: 


```python
zeppelin = ["Jimmy", "Robert", "John", "John"]
beatles += zeppelin
beatles
```

We can **check the length** of a list using the built-in [`len()`](https://docs.python.org/3.3/library/functions.html#len) function:


```python
len(zeppelin)
```


```python
print(zeppelin + ["bonzo jr."])
print(zeppelin)
```

Lists can also be **nested**: 


```python
# let's reset the beatles first
beatles = ["Paul", "John", "George", "Ringo"]
bands = [beatles, zeppelin]
bands
```

In fact, lists can be of hybrid data types, which, however, is something that you typically don't want to and shouldn't do:


```python
bad_bands = bands + [1, 0.3, 17, "This is bad"]
# this list contains lists, integers, floats and strings
bad_bands
```

#### 5. Loops

So far we have learned about two ways to contorl the flow of a program: functions and if-statements. Now we'll look at another important control structure: loops. A loop has a condition, and as long as that condition is true, it will continue to re-execute its body. 

There are two types of loops. For loops and while loops.

##### While loops

While loops use the `while` keyword, a condition, and the loop body:


```python
a = 1

# print numbers 0-100
while (a <= 100):
    print(a, end=", ") 
    # end is a parameter of print that defines how the string to be printed ends. 
    # By default, a newline \n is appended, which we overwrite here
    a += 1
```

What happens here? The `while` keyword indicates that this is a loop, which is followed by the **terminating condition of `b <= 100`**. As long as that condition is true, the loops body will be called again and again and again ...

Once the terminating condition evaluates to false, the code in the loop body will be skipped and the flow of execution continues below the loop. 

You might rightly guess that it's easy to write loops that don't terminate. Here is one example:
```python 
while True:
    print "Stuck"
```

This program is stuck in the loop forever (or until you interrupt it by interrupting your kernel, your computer goes off, etc.) It is hence important to take care that loops actually reach a terminating condition, and it's not always as obvious as in the previous example that this is not the case. 

But we could also **use the `break` statement to terminate a loop**:


```python
a = 1
while (True):
    print(a, end=", ") 
    a += 1
    if (a > 100):
        break
```

Here, we've moved the check of the condition into an if statement, and break if the if statement is executed. 

Similar to the `break` statement, there is also a `continue` statement, that ends evaluation of the loop body and goes back to the start of the loop in the next cycle:


```python
a = 0
while (a < 100):
    a +=1;
    # thorw brackets around all numbers divisible by 3
    if (not a % 3):
        print("[" + str(a) + "]", end=", ")
        continue # the next line isn't executed because the flow goes back to the beginning of the loop
    print(a, end=", ")
   
   
```

##### For loops

In contrast to most other programming language, Python uses for loops mainly to iterate over items of a sequence. 

It uses the following syntax:
```python
for variable in sequence:
    #body
```

The variable is then a accessible within the body of the loop.

Here is an example:


```python
for member in zeppelin: 
    print(member)
```

Of course, that works with arbitrary **slices of lists**: 


```python
for member in zeppelin[:2]:
    print(member)
```

We can iterate over **nested lists** with nested for loops: 


```python
for band in bands:
    print("Band Members: ")
    print("-------------")
    for member in band:
        print(member)
    print()
```

When you want to iterate over a sequence of numbers, use the [`range()`](https://docs.python.org/3/library/stdtypes.html#range) function. Range generates a sequence of numbers:


```python
# we create a new list with the output of the range function
list(range(5))
```


```python
# start at 0, stop at index 10, two steps
list(range(0, 10, 2))
```

Using this range function, we can now iterate of a sequence of numbers:


```python
for i in range(10): 
    print (i)
```

The range function also takes other parameters, specifically a "start", "stop" and a "step-size" parameter.


```python
for i in range (0, -20, -3):
    print(i)
```

#### 6. Recursion

Another way to control program flow is recursion. The basic idea of recursion is that a function is allowed to call itself. Here is an example for printing the numbers 0-10: 


```python
def printNumber(current, limit):
    print(current)
    if current < limit:
        printNumber(current + 1, limit)
```


```python
printNumber(0, 10)
```

Note that we have implemented looping / iteration behavior without actually using a loop! However, recursion can be used for more than just loops; it is very well suited, for example, to operate on trees and graphs.

We can also use return values in recursive functions. In the following, the recursive call is in the return statement. Here, the evaluation stack goes all the way to 10, after which the return doesn't contain another recursive call, terminating the recursion. Then all the functions return in the order in which they were called and build the string:


```python
def getNumberString(current, limit):
    if current <= limit: 
        return str(current) + "," + getNumberString(current+1, limit)
    
    return ""
```


```python
getNumberString(0, 10)
```

#### 7. Revisiting Lists: List Comprehension

Now that we know about loops, we can also take a look at [list comprehension](https://docs.python.org/3.5/tutorial/datastructures.html#list-comprehensions). List comprehension can be used to initialize and transform arrays. 




```python
# _ is customary for a variable name if you don't need it
[0 for _ in range(10)]
```


```python
["John" for _ in range(10)]
```


```python
# we can also make  use of values we iterate over
[i for i in range(10)]
```

We can, for example use functions in place of a variable. Here we initialize an array of random numbers in the unit interval:


```python
import random
rands = [random.random() for _ in range(10)]
rands
```

You can also use list comprehension to create a list based on another list:


```python
[x*10 for x in rands]
```


```python
[x for x in rands if str(x)[2] == '1']
```

### Pandas
### Lecture 5 - Dictionaries, Series

In this lecture we will, after a quick recap, introduce more data structures: sets, dictionaries, and series. While sets and dictionaries are built-in Python data structures, series (and dataframes, which we'll cover in the next lab) are part of the [pandas library](http://pandas.pydata.org/) tailored to data science applications.

#### If-Statements Recap

* By using if-elif-else statements we can realize conditional flow in a program. 
* If an elif take a parameter that is tested for truth. The parameter can be a boolean (`True` or `False`), or any other data type. 
    * For numerical data types, 0 and None evaluates to false, everything else to true.
    * For lists, strings, dictionaries, an empty container evaluates to false.

See the [documentation](https://docs.python.org/3/library/stdtypes.html#truth-value-testing) to understand what is considered true and false.


```python
def factors(x):
    # notice the use of the negation and the use of 0 as false
    if(not x % 2):
        print("2 is a factor of " + str(x))  
    elif(not x % 3):     # only evaluated when if was false
        print("3 is a factor of " + str(x))
    else: # only evaluated when both if and elif were false
        print("Neither 2 nor 3 are factors of " + str(x))

factors(4)
factors(9)
factors(13)
```

    2 is a factor of 4
    3 is a factor of 9
    Neither 2 nor 3 are factors of 13



#### Lists Recap

**A list is a collection of items.**   
**Lists are created with square brackets `[]` and can be accessed via an index:**


```python
beatles = ["Paul", "John", "George", "Ringo"]
# printing the whole array
print(beatles)
# printing the first element of that array, at index 0
print(beatles[0])
# third element, at index 3
print(beatles[3])
# access the one-but-last element
print(beatles[-2])
```

    ['Paul', 'John', 'George', 'Ringo']
    Paul
    Ringo
    George


We can also create **slices of an array with the slice operator `:`**

```python
a[start:end] # items start through end-1
a[start:]    # items start through the rest of the array
a[:end]      # items from the beginning through end-1
a[:]         # a copy of the whole array
```
There is also the step value, which can be used with any of the above:

```python
a[start:end:step] # start through not past end, by step
```

See [this post](for a good explanation on slicing).


```python
# Get the slice from 0 (included) to 2 (excluded)
beatles[:2] # this can also be written as [0:2]
```




    ['Paul', 'John']



The slice operation returns a new array, the original array is untouched: 


```python
beatles
```




    ['Paul', 'John', 'George', 'Ringo']



**We can change the elements that are contained in a list**: 


```python
beatles[1] = "JohnYoko"
beatles
```




    ['Paul', 'JohnYoko', 'George', 'Ringo']



Lists can also be **extended in-place with the `append()` function**:


```python
beatles.append("George Martin")
beatles
```




    ['Paul', 'JohnYoko', 'George', 'Ringo', 'George Martin']



Lists can be **concatenated**: 


```python
zeppelin = ["Jimmy", "Robert", "John", "John"]
beatles += zeppelin
beatles
```




    ['Paul',
     'JohnYoko',
     'George',
     'Ringo',
     'George Martin',
     'Jimmy',
     'Robert',
     'John',
     'John']



We can **check the length** of a list:


```python
len(zeppelin)
```




    4



Lists can also be **nested**: 


```python
# let's reset the beatles first
beatles = ["Paul", "John", "George", "Ringo"]
bands = [beatles, zeppelin]
bands
```




    [['Paul', 'John', 'George', 'Ringo'], ['Jimmy', 'Robert', 'John', 'John']]



##### While Loop Recap

While loops use the `while` keyword, a condition, and the loop body:


```python
a, b = 1, 1
while (b < 1000):
    print(b, end=", ") 
    # end is a parameter of print that defines how the string to be printed ends. 
    # By default, a newline \n is appended, which we overwrite here
    temp = b
    b += a
    a = temp
    # a better way of writing this is using simultaneous assignment: 
    # a, b = b, a + b
```

    1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 

This continues, until the terminating condition is reached. 

We can also **use the `break` statement to terminate a loop**: 


```python
a, b = 1, 1
while (True):
    print(b, end=", ") 
    a, b = b, a + b
    if (b > 1000):
        break
```

    1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 

##### For Loop Recap


For loops are mainly used to iterate over items of a sequence. 


```python
for member in zeppelin: 
    print(member)
```

    Jimmy
    Robert
    John
    John


When you want to iterate over a sequence of numbers, use the [`range()`](https://docs.python.org/3/library/stdtypes.html#range) function. Range generates a sequence of numbers:


```python
for i in range(10): 
    print (i)
```

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9


#### 1. Tuples

[Tuples](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences) are a list-like data structure that are, in contrast to lists **immutable**. 

The purpose of tuples is to store objects of different types. Remember that Lists should only contain homogeneous data; Tuples are designed for the heterogeneous case. 

Also, Tuples have practical implications for performance and HashTables, which we will discull later. 


```python
person = "Alex", 1981, "Computer Science"
person
```




    ('Alex', 1981, 'Computer Science')



Initialization with brackets is prefered, since it's more explicit:


```python
person = ("Alex", 1981, "Austria")
person
```




    ('Alex', 1981, 'Austria')



We can access them just like arrays: 


```python
person[1]
```




    1981



We cannot, however change values. This throws a **TypeError**.


```python
# throws TypeError
person[1] = 1985
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-17-443e0becffb3> in <module>
          1 # throws TypeError
    ----> 2 person[1] = 1985
    

    TypeError: 'tuple' object does not support item assignment


Arbitrary objects can be part of a tuple:


```python
train_schedule = ("Train 1", [9,11])
# this works because we're modifying the mutable array within the immuatable tuple.
train_schedule[1][0] = 15
train_schedule
```




    ('Train 1', [15, 11])



Of course, that includes tuples:


```python
train_schedule = ("Train 1", (9,11))
# this doesn't work
# train_schedule[1][0] = 15
train_schedule
```




    ('Train 1', (9, 11))




```python
a = 1
b = 2
(a, b) = (b, a)
a, b
```




    (2, 1)



##### Aside: Functions with Multiple Return Values

Consider the following code:


```python
def multiply(a, b, c):
    return (a*b), (a*c), (b*c), (a*b*c)
```

Here, we return multiple return values - something that's not possible in most programming languages! But it's very convenient. 

Let's try it out:


```python
multiply(3, 7, 11)
```




    (21, 33, 77, 231)



The round brackets indicate what's going on: what is returned, is in fact, a tuple!

We can use this return value to assign multiple variables at the same time:


```python
ab, ac, bc, abc = multiply(3, 7, 11)
print(ab, ac, bc, abc)
```

    21 33 77 231


To do this, no function is necessary. We can just do the following:


```python
what, i_s, going, on = "this", "is", "really", "nice" # use () to be more explicit
print(what, i_s, going, on)
```

    this is really nice


#### 2. Sets

A [set](https://docs.python.org/3/tutorial/datastructures.html#sets) is a mutable collection, similar to a list, however, it is
 * **not ordered**, and
 * **cannot contain the same element twice**

Here is an example:


```python
# Initialize a set with {}
beatles = {"John", "Paul", "Ringo", "George"}
beatles
```




    {'George', 'John', 'Paul', 'Ringo'}




```python
# Initialize the set with an array
usernames = set(["Jimmy", "Robert", "John", "John"])
usernames
```




    {'Jimmy', 'John', 'Robert'}



We've initialized the set `usernames` with an array of names. We have chose a set, because we don't want to have duplicate user names. However, **in the second example, the array included a duplicate - John was specified twice**. We can see, however, that **John was added to the set only once**.

Also note the order of the elements in the first set: Initialized with:
```python
{"John", "Paul", "Ringo", "George"}
```
But the output isn't ordered: 
```python
{"John", "Paul", "Ringo", "George"}
```

Sets are great for various tasks. For example, they can be used to remove duplicate entries from lists. Most importantly, they let you very efficiently check whether an element already exists. 

A set works based on a mathematical function that produces a "hash code". This hash code is then used as an index to an array. For example, "Jimmy" could hash to the value 13, and accordingly, Jimmy would be put at the 13th index of an array. When we want to test whether "Jimmy" is already in a set, we simply compute the hash, which will again produce 13, and then look up whether something is stored at index 13. 

We can check whether a set contains a value using the `in` keyword:


```python
"Jimmy" in usernames
```




    True




```python
"Ringo" in usernames
```




    False



We can add values using the add function on a set:


```python
usernames.add("JohnB")
usernames
```




    {'Jimmy', 'John', 'JohnB', 'Robert'}



And remove elements with the remove function: 


```python
usernames.remove("John")
usernames
```




    {'Jimmy', 'JohnB', 'Robert'}



If the set doesn't contain a key we want to remove, it will throw a `KeyError`.


```python
usernames.remove("Joseph")
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-32-7da8667bc580> in <module>
    ----> 1 usernames.remove("Joseph")
    

    KeyError: 'Joseph'


To prevent that, it is advisable to first check whether a set actually contains a value, if you're not 100% sure: 


```python
if ("Joseph" in usernames):
    usernames.remove("Joseph")
```

We can iterate over the values of a set. Note, however, that no guarantee about the order of the set is made. 


```python
for name in usernames:
    print (name)
```

    JohnB
    Robert
    Jimmy


Make sure to check out the [documentation](https://docs.python.org/3.5/library/stdtypes.html#set) to see what else a set can do. 

#### Exercise 2: Sets

Write a function that finds the overlap of two sets and prints them. Initialize two sets, e.g., with values {13, 25, 37, 45, 13} and {14, 25, 38, 8, 45} and call this function with them.


```python
def setIntersect(a, b):
    return set([x for x in a if x in b])

set1 = set([13, 25, 37, 45, 13])
print(set1)
set2 = set([14, 25, 38, 8, 45])
print(setIntersect(set1, set2))
```

    {45, 25, 37, 13}
    {25, 45}


#### 3. Dictionaries

[Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) are related to sets, but are more powerful: in addition to the key used to identify an element in a set, dictionaries also store a value associated with a key. Other terms commonly used for dictionaries are *associative arrays*, *(hash) maps*, and *hash tables*. 

Here is a simple example:


```python
musicians = {"John":"Zeppelin", "Jimmy":"Zeppelin", "Paul":"Beatles", "Ringo":"Beatles"}
musicians
```




    {'John': 'Zeppelin',
     'Jimmy': 'Zeppelin',
     'Paul': 'Beatles',
     'Ringo': 'Beatles'}



As we can see, a dictionary can be created with curly brackets and a list of key-value pairs, separated by a `:`. Here, the names are the keys, the bands are the values. 

There are other ways of creating a dictionary. Here, we pass a list of tuples to the dictionary, but we could also pass a list of lists.


```python
more_musicians = dict([("Thom", "Radiohead"), ("Dave", "Foo Fighters")])
more_musicians
```




    {'Thom': 'Radiohead', 'Dave': 'Foo Fighters'}



Of course, a dictionary can be of any data type. Here is an example with int as keys, floats as values:


```python
numbers = {3:1.45, 4:1.32, 19:9.97, 6:9.99}
numbers
```




    {3: 1.45, 4: 1.32, 19: 9.97, 6: 9.99}



Note that it's generally not a good idea to use floats as keys, as they are stored only as approximations.

Dict elements are accessed just as elements in a list, with square brackets, but instead of the index, we pass in the key: 


```python
numbers[3]
```




    1.45




```python
musicians["John"]
```




    'Zeppelin'



We can add elements to a dict:


```python
musicians["Thom"] = "Radiohead"
musicians
```




    {'John': 'Zeppelin',
     'Jimmy': 'Zeppelin',
     'Paul': 'Beatles',
     'Ringo': 'Beatles',
     'Thom': 'Radiohead'}



And remove them using the `del` keyword:


```python
del musicians["Thom"]
musicians
```




    {'John': 'Zeppelin',
     'Jimmy': 'Zeppelin',
     'Paul': 'Beatles',
     'Ringo': 'Beatles'}



Again, we have to worry about key errors. If we want to remove Thom again, we'd get a `KeyError`.


```python
del musicians["Thom"]
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-43-4d3b8f26b654> in <module>
    ----> 1 del musicians["Thom"]
    

    KeyError: 'Thom'


We can access a list of keys and values separately: 


```python
musicians.keys()
```




    dict_keys(['John', 'Jimmy', 'Paul', 'Ringo'])



Notice that the result is not a list or a set, but a [view object](https://docs.python.org/3/library/stdtypes.html#dict-views). A view object always is updated when the dictionary is changed, and we can use it to iterate over a dictionary. 


```python
for musician in musicians.keys():
    print(musician)
```

    John
    Jimmy
    Paul
    Ringo


This also works with `values()` and `items()`:


```python
musicians.values()
```




    dict_values(['Zeppelin', 'Zeppelin', 'Beatles', 'Beatles'])




```python
musicians.items()
```




    dict_items([('John', 'Zeppelin'), ('Jimmy', 'Zeppelin'), ('Paul', 'Beatles'), ('Ringo', 'Beatles')])



The latter is especially handy for iterating over the key-value pairs in a dictionary:


```python
# notice that we iterate over the tuples and have the elements of the tuple assigned to k and v, respectively.
for k, v in musicians.items():
    print (k + ", " + v)
```

    John, Zeppelin
    Jimmy, Zeppelin
    Paul, Beatles
    Ringo, Beatles


Another way to write the previous expression would be like this: 


```python
for k in musicians.keys():
    print(k + ", " +  musicians[k])
```

    John, Zeppelin
    Jimmy, Zeppelin
    Paul, Beatles
    Ringo, Beatles


Make sure to check out [the dictionary documentation](https://docs.python.org/3/library/stdtypes.html#typesmapping) for more info. 


```python
for k in musicians:
    print(k)
```

    John
    Jimmy
    Paul
    Ringo


##### Exercise 3: Dictionaries

 * Create a dictionary with two-letter codes of two of US states and the full names, e.g., UT: Utah, NY: New York
 * After initially creating the dictionary, add two more states to the dictionary.
 * Create a second dictionary that maps the state codes to an array of cities in that state, e.g., UT: [Salt Lake City, Ogden, Provo, St. George]. 
 * Write a function that takes a state code and prints the full name of the state and lists the cities in that state.

#### 4. Working with Modules

While we briefly touched on modules (remember the `import math` statement), we haven't really talked about what a module is. Modules are used, ugh, to modularize code. You can write a module by simply creating a `.py` file. We won't be writing many modules ourselves, but we will use them extensively.

To import a module simply write

```python
import module_name
```

You can then use functions defined in the module with the `.` notation. Here's an example:


```python
import math
math.sqrt(9)
```




    3.0



We can also use the `from` notation to import specific functions from a package and add them directly to the namespace:


```python
from math import log10
# notice that this is NOT accessed via math.log10()
log10(3)
```




    0.47712125471966244



You can also bulk-import all functions of a module into your local namespace, however, this is **strongly discouraged**, as it can lead to name-clashes and makes your code unreadable eventually.


```python
from math import * 
log2(3)
```




    1.584962500721156



Finally, we can redefine the name of a module. This is useful to define a shorthand for long library names.


```python
import math as m 
m.sqrt(13)
```




    3.605551275463989



#### 5. The Pandas Library: Series

Pandas is a popular library for manipulating vectors, tables, and time series. We will frequently use Pandas data structures instead of the built-in python data structures, as they provide much richer functionality. Also, Pandas is **fast**, which makes working with large datasets easier.  Check out the official pandas website at [http://pandas.pydata.org/](http://pandas.pydata.org/).

This tutorial is partially based on the [excellent book by Matt Harrison](https://www.amazon.com/Learning-Pandas-Library-Analysis-Visualization-ebook/dp/B01GIE03GW/).

Pandas provides three data structures: 

 * the **series**, which represents a single column of data similar to a python list
 * the **data frame**, which represents multiple series of data
 * the **panel**, which represents multiple data frames
 
We'll mostly work with series and data frames and largely ignore panels. Today we will stick to series. 

Pandas should already be part of your anaconda installation. If not, simply run:

```
$ conda install pandas
```

To make pandas available, we'll import the module into this notebook. It is customary to import pandas as `pd`:


```python
import pandas as pd
```

Series are the most fundamental data structure in pandas. Let's create two simple series based on an arrays:


```python

bands = pd.Series(["Stones", "Beatles", "Zeppelin", "Pink Floyd"])
bands
```




    0        Stones
    1       Beatles
    2      Zeppelin
    3    Pink Floyd
    dtype: object




```python
founded = pd.Series([1962, 1960, 1968, 1965])
founded
```




    0    1962
    1    1960
    2    1968
    3    1965
    dtype: int64



When we output these objects we can see an index, also called an axis, which by default is an integer sequence starting at 0, and the associated values. 

| Index | Value | 
| - | - |
| 0  |        Stones
|1   |    Beatles
|2  |    Zeppelin
|3 |    Pink Floyd

Pandas also tells us the data type of the values, `object` for the first series - in this case, this is a string, `int64` (a 64-bit integer) for the second.

Notice that `int64` is not a Python datatype, but a C integer of 64 bit length - which, unlike Python integers - can overflow!

We can also use other data types as indices, in which case the series behaves a lot like a dictionary:


```python
# the data is the first parameter, the index is given by the index keyword
bands_founded = pd.Series([1962, 1960, 1968, 1965, 2012],
                          index=["Stones", "Beatles", "Zeppelin", "Pink Floyd", "Pink Floyd"], 
                          name="Bands founded")
bands_founded
```




    Stones        1962
    Beatles       1960
    Zeppelin      1968
    Pink Floyd    1965
    Pink Floyd    2012
    Name: Bands founded, dtype: int64



| Index | Value | 
| - | - |
| Stones     |    1962
| Beatles    |    1960
| Zeppelin     |  1968
| Pink Floyd |    1965
| Pink Floyd |    2012

Here we see something interesting: We've used the same index (Pink Floyd) twice, once for the original founding of the band, and once for the re-union starting in 2012. Also, the order of the entries is preserved. 

A series is, so to speak, both, a list and a dictionary! 

We can access the values of an array by printing the member `values`.


```python
bands.values
```




    array(['Stones', 'Beatles', 'Zeppelin', 'Pink Floyd'], dtype=object)



And we can look at how the index is composed:


```python
bands.index
```




    RangeIndex(start=0, stop=4, step=1)



What we see here is that this isn't an explicit list, but rather a set of rules!

Let's compare this to the index where we used explicit labels:


```python
bands_founded.index
```




    Index(['Stones', 'Beatles', 'Zeppelin', 'Pink Floyd', 'Pink Floyd'], dtype='object')




```python
bands_founded.values
```




    array([1962, 1960, 1968, 1965, 2012])



We can access individual entries as we'd access an array or a dictionary:


```python
bands[0]
```




    'Stones'




```python
bands_founded["Beatles"]
```




    1960



There is also a method for looking up a value:


```python
bands_founded.get("Stones")
```




    1962



Note that these access methods are as fast as a dictionary lookup (compared to a lookup in a list).

That works also with arrays of labels, in which case the return type is a series, not a single value.


```python
bands_founded.get(["Stones", "Beatles"])
```




    Stones     1962
    Beatles    1960
    Name: Bands founded, dtype: int64



Notice that when we access data with multiple indices, we don't get a simple data type, as in the above cases, but instead get another series back:


```python
bands_founded["Pink Floyd"]
```




    Pink Floyd    1965
    Pink Floyd    2012
    Name: Bands founded, dtype: int64



Series also have indexers for label-based access: [`loc`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html)


```python
# And one more way for looking up a value:
bands_founded.loc["Stones"]
# this is equivalent to 
# bands_founded["Stones"]
```




    1962



Related to the `loc` indexer is the [`iloc`](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.DataFrame.iloc.html) indexer. However, instead of an index, `iloc` operates purely on position, not on index labels: 


```python
bands_founded.iloc[1]
```




    1960



Notice that there is also an `ix` indexer, which, however, is deprecated and should not be used.

These ways of accessing slices of a dataset (`loc`, `iloc`), will make more sense when we use dataframes instead of series - in dataframes, `loc` and `iloc` operate on the rows, whereas square brackets operate on the columns.

##### Iterating

Iteration works as you would expect: 


```python
for band in bands:
    print(band)
```

    Stones
    Beatles
    Zeppelin
    Pink Floyd



```python
for band, founded in bands_founded.items():
    print(band + ", " + str(founded))
```

    Stones, 1962
    Beatles, 1960
    Zeppelin, 1968
    Pink Floyd, 1965
    Pink Floyd, 2012


##### Updating
Updating works largely as expected, however, you have to be careful when updating series with duplicate indices:


```python
bands[2] = "The Doors"
bands
```




    0        Stones
    1       Beatles
    2     The Doors
    3    Pink Floyd
    dtype: object



We can add a new item by direclty assigning it to a new index.


```python
bands[4] = "Zeppelin"
bands
```




    0        Stones
    1       Beatles
    2     The Doors
    3    Pink Floyd
    4      Zeppelin
    dtype: object



Note that the indices don't have to be sequential.


```python
bands[17] = "The Who"
bands
```




    0         Stones
    1        Beatles
    2      The Doors
    3     Pink Floyd
    4       Zeppelin
    17       The Who
    dtype: object



We can also use a function to set the value:


```python
bands.at[9] = "Hendrix"
bands
```




    0         Stones
    1        Beatles
    2      The Doors
    3     Pink Floyd
    4       Zeppelin
    17       The Who
    9        Hendrix
    dtype: object



When we update based on an index that occurs more than once, all instances are updated:


```python
bands_founded["Pink Floyd"] = 2015
bands_founded
```




    Stones        1962
    Beatles       1960
    Zeppelin      1968
    Pink Floyd    2015
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



A way to update a specific entry when an index is used multiple time is to use the `iloc` indexer. We can use the `iloc` array to set values based purely on position. However, all of this is rather ugly.


```python
bands_founded.iloc[3] = 1965
bands_founded
```




    Stones        1962
    Beatles       1960
    Zeppelin      1968
    Pink Floyd    1965
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



##### Deleting 

Deleting is rarely done with pandas data structures, instead filters and masks are used. It's possible based on indices:


```python
del bands_founded["Stones"]
bands_founded
```




    Beatles       1960
    Zeppelin      1968
    Pink Floyd    1965
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



##### Indexing and slicing

Indexing and slicing works largely like in normal python, but instead of just directly using the bracket notations, it is recommended to use `iloc` for indexing by position and `loc` for indexing by index. 


```python
# slicing by position
bands_founded.iloc[1:3]
```




    Zeppelin      1968
    Pink Floyd    1965
    Name: Bands founded, dtype: int64



When slicing by index, the last value specified is *included*, which differs from regular Python slicing behavior:


```python
# slicing by index
bands_founded.loc["Zeppelin" : "Pink Floyd"]
```




    Zeppelin      1968
    Pink Floyd    1965
    Pink Floyd    2015
    Name: Bands founded, dtype: int64




```python
# Note that index 17 is included
bands.loc[1:17]
```




    1        Beatles
    2      The Doors
    3     Pink Floyd
    4       Zeppelin
    17       The Who
    dtype: object



Both, `iloc` and `loc` can be used with arrays, which isn't possible in vanilla Python:


```python
bands_founded.iloc[[0,3]]
```




    Beatles       1960
    Pink Floyd    2015
    Name: Bands founded, dtype: int64




```python
bands_founded.loc[["Beatles", "Pink Floyd"]]
```




    Beatles       1960
    Pink Floyd    1965
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



And, all these variants can also be used with boolean arrays, which we will soon find out to be very helpful:


```python
bands_founded
```




    Beatles       1960
    Zeppelin      1968
    Pink Floyd    1965
    Pink Floyd    2015
    Name: Bands founded, dtype: int64




```python
bands_founded.loc[[True, False, False, True]]
```




    Beatles       1960
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



##### Masking and Filtering

With pandas we can create boolean arrays that we can use to mask and filter a dataset. In the following expression, we'll create a new array that has "True" for every band formed after 1964:


```python
mask = bands_founded > 1964
mask
```




    Beatles       False
    Zeppelin       True
    Pink Floyd     True
    Pink Floyd     True
    Name: Bands founded, dtype: bool



This uses a technique called **broadcasting**. We can use broadcasting with various operations:


```python
# Not particularly useful for this dataset..
founding_months = bands_founded * 12
founding_months
```




    Beatles       23520
    Zeppelin      23616
    Pink Floyd    23580
    Pink Floyd    24180
    Name: Bands founded, dtype: int64



We can use a boolean mask to filter a series, as we've seen before:


```python
# applying the mask to the original array
# note that almost all of those operations return a new copy and don't modify in place
bands_founded[mask]
```




    Zeppelin      1968
    Pink Floyd    1965
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



The short form here would be:


```python
bands_founded[bands_founded > 1967]
```




    Zeppelin      1968
    Pink Floyd    2015
    Name: Bands founded, dtype: int64



**This is a super useful thing.  We'll use it to select rows from tables a lot!**

#### Exploring a Series

There are various way we can explore a series. We can count the number of non-null values: 


```python
numbers = pd.Series([1962, 1960, 1968, 1965, 2012, None, 2016])
numbers.count()
```




    6




```python
numbers
```




    0    1962.0
    1    1960.0
    2    1968.0
    3    1965.0
    4    2012.0
    5       NaN
    6    2016.0
    dtype: float64



We can get the sum, mean, median of a series:


```python
numbers.sum()
```




    11883.0




```python
numbers.mean()
```




    1980.5




```python
numbers.median()
```




    1966.5



We can also get an overview of the statistical properties of a series: 


```python
numbers.describe()
```




    count       6.000000
    mean     1980.500000
    std        26.120873
    min      1960.000000
    25%      1962.750000
    50%      1966.500000
    75%      2001.000000
    max      2016.000000
    dtype: float64



Note that None/NaN values are ignored here. We can drop all NaN values if we desire:


```python
numbers = numbers.dropna()
numbers
```




    0    1962.0
    1    1960.0
    2    1968.0
    3    1965.0
    4    2012.0
    6    2016.0
    dtype: float64



This works also for non-numerical data. Of course, we get different measures:


```python
bands.describe()
```




    count             7
    unique            7
    top       The Doors
    freq              1
    dtype: object



Other useful methods are asking for a specific quantile, the minimum, the maximum, etc. 


```python
numbers.quantile(0.25)
```




    1962.75




```python
numbers.max()
```




    2016.0




```python
numbers.min()
```




    1960.0



#### Sorting 

We can sort a series:


```python
numbers.sort_values()
```




    1    1960.0
    0    1962.0
    3    1965.0
    2    1968.0
    4    2012.0
    6    2016.0
    dtype: float64




```python
sorted_numbers = numbers.sort_values(ascending=False)
sorted_numbers
```




    6    2016.0
    4    2012.0
    2    1968.0
    3    1965.0
    0    1962.0
    1    1960.0
    dtype: float64



Note that the indices remain constant. We can **reset the indices**:


```python
# If we don't specify drop to be true, the previous indices are preserved in a separte column
sorted_numbers = sorted_numbers.reset_index(drop=True)
sorted_numbers
```




    0    2016.0
    1    2012.0
    2    1968.0
    3    1965.0
    4    1962.0
    5    1960.0
    dtype: float64



We can also sort by the index:


```python
# mix up the indices first
new_sorted_numbers = numbers.sort_values()
print(new_sorted_numbers)
new_sorted_numbers.sort_index()
```

    1    1960.0
    0    1962.0
    3    1965.0
    2    1968.0
    4    2012.0
    6    2016.0
    dtype: float64





    0    1962.0
    1    1960.0
    2    1968.0
    3    1965.0
    4    2012.0
    6    2016.0
    dtype: float64



#### Applying a Function

Often, we will want to apply a function to all values of a Series. We can do that with the map function:


```python
import datetime

# Convert an integer year into a date, assuming Jan 1 as day and month.
def to_date(year):
    return datetime.date(int(year), 1, 1)
    
new_sorted_numbers.map(to_date)
```




    1    1960-01-01
    0    1962-01-01
    3    1965-01-01
    2    1968-01-01
    4    2012-01-01
    6    2016-01-01
    dtype: object



This is an incredibly powerful concept that you can use to modify series in sophisticated ways.

Another way to use the map function is to pass in a dictionary that is then applied to matching objects: 


```python
new_sorted_numbers.map({1965:1945, 2012:1999, 1968:"What"})
```




    1     NaN
    0     NaN
    3    1945
    2    What
    4    1999
    6     NaN
    dtype: object



#### Conclusion

Series (and data frames, which we will tackle in the next lab) are incredibly powerful. We've only covered a small part of the features here. Make sure to also check out resources such as the [10 minutes to pandas guide](http://pandas.pydata.org/pandas-docs/stable/10min.html).