## Week 2 - Day 3
### References
Without references it can be impossible to pass data to a function and edit it for later use.

ex: Swapping variables.

```
void swap(int x, int y){
    int swap = x;
    x = y;
    y = swap;
}
```

These values cannot be returned out of the function for use elsewhere  
The memory stack does not remain, it is popped off the stack without editing the main function memory stack variables 
What we want is to change what a and b is in main, not the x and y in the swap function
Without references we are passing by value. This "copies" the variable data into its own stack of memory, changes it, then when the memory is removed, the copy is deleted. 

**References** are how we do this  
References are "nicknames" that refer to the original variable in the stack  
References dont get their own slots in the memory stack  
References use the ```&``` symbol attached to the data type in the function signature  

Using references we can update the swap function to actually change the variables. 

```
void swap(int& x, int& y){
    int swap = x;
    x = y;
    y = swap;
}
```

This is passing by reference instead of by value.  
From a programming perspective, the reference and the original variable are one and the same.  
When the reference is created, we have to specify what exactly it is going to refer to. In the above example, it is specifically referencing the first passed int as x and second one as y, allowing us to change the variables a and b with a ```swap(a,b)``` call.  

int x can be declared but not defined until later, int& x, has to be defined at the time of being declared. 

These do no have to occur inside a function signature either. 

```
string s = “Hello”
for (int I = 0; I < s.size(); I++){
    s[I] = toupper(s[I]);
}
```
This code will not output string s as "HELLO" as the uppercased character only lives in the for loop. 

It can be referenced however and used in a for each loop to update as: 

```
for(char& c : s){
	c = toupper(c);
}
```

Passing by reference does not create a "copy" of the data we are utilizing, which is good for memory. However it does make it easy to update the data even if we aren't meaning to.

Passing a **const reference** however will allow you to look at the data, without copying it, but not be able to update the data in the function actions. It will throw an error if you try to update. The *const* is a constant and cannot be changed inside what it is being referenced by. 

```
bool contains(const vector<int>& v, int x){
    For(int x : v){
        return true;
    }
    return false; 
}
```

All said and done there are three main ways to call a variable:   
**Pass by value** creates a copy of the variable in its own memory stack, does not update the variable in the main memory stack when editted. This can be heavy on memory, but has uses. 

***Pass by reference*** does not create a copy of the variable, it utilizes the variable in the main memory stack and allows us to edit it as needed. 

**Pass by const reference** does not copy the variable to save on memory, but will not allow us to edit the variable. 