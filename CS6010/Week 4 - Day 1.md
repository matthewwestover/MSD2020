## Week 4 - Day 1
### Stack and Heap
One of the most important topics for good developers. Difficult concept even for many experienced computer science

### Memory
We get memory in the stack when we call the function

#### Stack
Mostly deal with *values with names*  
Automatic Memory Handling  
Allocation: calling function creates memory in the stack  
Deallocate: return a function removes memory from the stack  
Pros:

* Automatic, easy, can’t “forget” to deallocate
* Fast way of allocating memory (pointer indicates top of stack, just moves up and down to assign and remove)

Cons: 

* No control over member once function returns (lifetime of data is tied to lifetime of function)
* Compiler handles the allocation, thus size needs to be known at time of compiling
    * This is why arrays are locked in size. Need to know in advance to call. 
        * Vectors don’t use stack memory to store content 

#### Heap
Messy pile of memory different from the stack (volatile memory organization)  
RAM that isn’t the stack  
Allows for larger chunks of memory with longer lifespans  
Mostly deal with pointers with addresses (no names)  
Stack has pointer address for heap  
 
Allocate: specifically ask for it  
Deallocate: specifically release it  

One to ask for memory  
one to deallocate

New - allocates memory  
Delete - deallocates memory (bad name choice here doesn't delete it in heap)  

```
new TypeOfData // typeOfData* dp
    int* pInt = new int;
    double* dArr = new double[5]; // returns an array of 5 doubles

delete pointerToHeap // marks data as unused. Does not actually clear memory. 
```

Pointer still exists: 

* Just tells os that data area can be overwritten
* We can still use the pointer to point elsewhere
* Good tip is to take pointToHep = nullptr; 
	* crashes if you try to access helps prevents bugs

```
Int* x;
Int y = 0;
x = new int;
*x = y;
delete x;
```

Stack frame includes x and y. 
X starts as a pointer, pointing somewhere, we don’t know what   
Y = 0 in stack frame  
x then becomes a pointer to a section of heap memory  
Heap has an allotment of int, we don’t know what it is.  
*x dereferences the pointer, follows arrow, setting what’s at the end to y  
Heap is an allotment of int with value y, which is 0  
Heap is set as not used, Value and arrow remain  

*x = 2 // The heap sets 2 at the point 0 was set (Most likely). Even with the ‘delete’ x command already run. Something else might point here later, which is why this is bad. 

x is still a pointer in stack, can be pointed somewhere else even after ‘delete x’

If you do:
```
Delete x;
x = nullptr;
*x = 2; 
```

Program will crash instead, with a segment fault
Null = int 0 while nullptr is a pointer

Return a pointer to the heap in a function allows data to be stored longer than the lifespan of the function  
If we store x = new int[size], this stores an array of ints in memory with unknown size. x pointer is to the first item in the heap memory pool  

To delete reference to an array of data is ```delete [ ] x; // empty brackets, delete auto figures out the size. ```

This is how a vector works. It creates an array in heap and calls / removes it as needed. This is how dynamically sized data works. 

To grow, a new array is called as a bigger one allocated in heap. Old data is copied over, old data is then no longer pointed at. Vectors do this automatically.  

* Vectors keep track of capacity and current size.  
* Capacity is always bigger than current size.  
* Capacity is doubled every time vector has to increase in size. 

```
Int size;
Int* x = new int[size];
Int* newArr = new int[size*2]; // creates new bigger array
For(int I = 0 …. Size){
    newArr[i] = x[i]; // copy data from old array into new square brackets are dereferencing automatically. Equiv to *(newArr + i)
}
Delete [] x; // tell compiler we aren’t going to use the x[] heap of data. 
x = newArr; // x pointer now points to newArr in heap
newArr = nullptr; // only x is now pointing at the new bigger Array. Auto points to the front of the new array
Size = size*2
```

If you can put memory on the stack put it on the stack  
Easier to handle and manage  
Cheaper in allocating memory  
Less error prone  

Heap is used for separating lifetime of data from function and we cannot just return the value right away  
Heap is also used for if we don’t know how much memory we need from the start  

For now vector and string are mainly what we see that use heap. Generally use standard library instead of making your own  

### Errors

Heap is error prone. C++ has no checks on memory calls. Address sanitizer is super useful for catching heap errors  

Memory Leak = memory allocated, never deallocated in code (use new but never delete)   
Least “dangerous” of the heap errors   
Int* x = new int; // memory leak, no deallocation   
Slows things down. Longer program runs, the bigger problem
New should always match with delete  
    Can be hard in branching code to ensure a delete is always used   
Ownership “whose job is it to delete” can be tricky  
No “arrows” to heap memory that is marked as in use  

Use after free = use after delete  
Free is the C equivalent of C++ delete  
Attempt to use the pointer to heap memory after stating we no longer want to use that heap memory 

```
Int* x = new int;
Delete x;
Cout << *x; 
```

Cannot look at the memory we already said we want to no longer use   
If calling the *x is close to the delete, it might work, but address sanitizer will catch  
Often reason for security breaks  

Out of bounds access - try to get into above or below where a heap memory slot is.  
myArray[-1] might work or might crash  

```
Int* f( ){
 Int x;
 Return &x;
}
```

Pointing to stack memory  
This compiles but is bad. Returning an address to an int. x only lives inside the function, but we are pointing to the point in stack frame for x.  

###Structs
String s = “hello”;  
S is in the stack. “Hello” is in the heap as we don’t know how big it is until we create it.  
S in the stack has char* that points to ‘h’ in heap and a size  

Vectors have a start point, a int used and a int capacity  
    
    