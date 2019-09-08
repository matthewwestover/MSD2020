## Week 2 - Day 5
### Arrays
C++ has built in Arrays but they are difficult to use
They have a set size, that cannot be changed, and do not react to functions as expected.  
**Vectors** are the better C++ solution to storing *lists* of data.  
Vectors don't need to know the length of the list when being created.  
Arrays have set sizes, you have to declare when created it. Cannot be updated.  
To declare ```dataType name[size];```  
Arrays can use [] to access specific locations in it, just like vectors and strings.  
```name[0]``` - access the first item in the array.  
These cannot use vector functions such as ```.size(), .front(), push_back(),```  
Arrays can forget their size at times too.  

```
Void f(int x[3]){ // No references, pass by value function, copy the input array
    x[0] = 0
    x[1] = 1
    x[2] = 2
}

Int main(){
    Int arr[3] = { 4,5,6 }
    f(arr);
}
```
This returns 0,1,2 even without passing by reference. The array updated.  
Better format for calling arrays.  
Alt format is ```std::array <type, size>``` 

Slightly better method to call arrays much more like a vector in passing the data so it doesn’t change. Difference from vector is it can’t change its size.  

###Pointers
Pointers work like references. They were how C++ use to handle references before references were added.  

* In memory stack it stores an arrow to another point in the stack. (The memory address)
* Arrow can be manipulated to point else where
* Arrow can be followed to get what is at where it is pointing
* References can’t change where they are “pointed” at. Pointers can. 

Pointer syntax is **weird**  
Vectors don’t “exist” only a vector of something  
Pointers don’t “exist” only a pointer of something  

pointeeType*

* What you are pointing to
* Needs an asterisks to make a pointer

```int *pInt;``` <- Group this way 

* Ex: int * px, py only has one pointer for px, none for py

```Float *pf;``` is a dangling pointer and is bad  
Pointers need an assignment  
Need to find where things are in memory to point the pointer at

Use Address of operator to point:
```&```
Takes something that has a memory address and tells us what it is  
Use reference a variable, or a part of an array. Not a constant or datatype  
&5 or &int doesnt work  
$a = 5 does have an address  
&v[0].rain has an address

```Float *pf = &f;``` Or ```Float *pf;```   
```Pf = &f;``` < this moves the arrow

“Dereferencing” *  
```Cout << pf << endl``` this prints the address not the value at the other end of the address  
```Cout << *pf << endl``` this prints out the value on the other side of the pointer (follow the arrow)  

```*pf = 5.5``` sets the value on the other end of the arrow  
Same as saying f = 5.5  

To follow arrow you need an asterisk or else you are talking the arrow itself  

You can swap with pointers:  

```
Int main(){
    Int x=1, y=2;
    Swap(&x, &y);
}
```
Broken (just changes arrows, doesn’t change the values of x and y)

```
Void swap (int *a, int *b){
    Int tmp = a; (give me a second arrow to the same place as a)
    A = b; (pointing to the thing b is pointing to (y))
    B = tmp; (point to the thing tmp is pointing to (x))
}
```
Functional Pointer Swap

```
Void swap (int *a, int *b){
    Int tmp = *a; (sets the int tmp to have the value that is at the end of a’s arrow)
    *a = *b; (set the value at the end of a’s arrow to be the value at the end of b’s arrow)
    *b = tmp; (set the value at the end of b’s arrow to be what is in tmp)
}
```

### Why use a pointer
Were basically references before references were a thing  
**USE REFERENCES WHEN YOU CAN**
    
* Easier to reason with
* Less mistakes than what happens with pointers
 
Pointers can be modified to point to something else, which references can’t do  
Useful with arrays  
Pointer to nullptr is a value that “doesn’t exist”  
Optional param in function. 

```
Int f (int *optional){
If(optional != nullptr)
	Use (*optional)
	Stuff
}
```

f(&myInt) <- passing a value  
f(nullptr) < points to an unusable position in memory  
If you use *nullptr the program will crash right away  
Necessary for “lower” level code functionality  
std::vector uses pointers internally  

### Pointers and Strings
“Hello world” can be stored as a string, looks like a “string” but is not a string  
This is really a "c string"  
It is really ```const char *h =“hello world”```  
Pointing at char ‘h’  
In memory ‘ello world’ is stored just after ‘h’ as individual chars  
h++ changes the arrow pointing, so now it points to the ‘e’  
Looping through h++ gives us all the chars in the “string”  
Doesn’t know when to stop  
\0 is the special marker in memory at the end of memory that tells h++ to stop looping through string char  
This is a PITA to use  

* Security issues
* Remembering to include \0

std::string has way more built into it and is better  

### Pointer Arithmetic
How to look at things in arrays/vectors/strings  
Its why arrays start at 0, you do not add anything to the pointer location  
```*(ptr + 3) == ptr[3] == 3[ptr]```  

```Int main (int argc, char** argv)```  
Pointer to char\* which is a pointer to char  
Two layers of pointers  
(Char\*) is a c-string  
Char\*\* pointer to string  
Argv is basically an array of strings  
argc is for finding the location within that array of strings  
Strings have \0 end for finding their own size  
Translates to ```std::array<std::string, argc>```  
These are the command line functions that get passed to the main function