## Week 3 - Day 3
### Hash Tables

|                    |         Searching        |         Insertion        |         Deletion         |                Notes                |
|:------------------:|:------------------------:|:------------------------:|:------------------------:|:-----------------------------------:|
|  Arrays/ArrayList  | Random Access (Constant) | Linear                   | Linear                   | Must know size  ahead of time       |
|     LinkedList     | Linear                   | Linear, or  O(1) on ends | Linear, or  O(1) on ends | Call allocate new  items on demands |
| Binary Search Tree | Log n                    | Log n                    | Log n                    | Must be balanced                    |
|       Stacks       | Constant                 | Constant                 | Constant                 | Access limited to  top              |
|       Queues       | Constant                 | Constant                 | Constant                 | Access limited to  front/back       |

We want something that is fast/constant like stacks and queues but NOT limited to the access point  

#### Exercise
Create a data set that CAN hold numbers ```0 - MAX_INT```

##### Naive solution
Use array[MAX_INT] to create it  
Initialize every value to -1  
When inserting a number n, put it in at array[n]  
When searching for n see if value at array[n] is -1 or not  
When deleting a number n  - set array[n] to -1. 
This is all constant action. 
But is super unpractical, allocating THAT much memory, especially given so much is unused

We can use smaller arrays. Map indices to range of the smaller array  
Using mod operator (%) is guaranteed to return a number between 0 and n-1  
Numbers 0-100 In an array[10]  
Inserting 12, is outside the bounds of 10. 12 % 10 = 2, insert 12 at array[2]  
90 % 10 = 0, insert 90 at array[0]  
46 %10 = 6, insert 46 at array[6]  
If you get a duplicate mod value (insert 22 wants to go to array[2]) there are options to handle this  
This is known as a collision. Usually just insert at the next open index  

This is a **hash** array  
You create a “hash” equation to generate a integer value that can be small enough to fit inside your array  
A hash method is a method that generates an integer index for any given object  
Using a hash table is much like using a stack, but no limit on access point to “top”/“front". 

|            | Searching | Insertion | Deletion |                    Notes                   |
|:----------:|:---------:|:---------:|:--------:|:------------------------------------------:|
| Hash Table | Constant  | Constant  | Constant | Magic?  (Careful choosing of hash methods) |

If we have a string, getting the integer value can be difficult  
Taking just the length of the string can cause a lot of collisions  
We want to spread them out as much as possible to avoid collisions  

A general solution needs to be applicable to all general objects, not just specifically an integer or string  
How do we generate a hash value for the Circle class? It can be difficult to establish how we get our hash value  

The underlying data structure to a hash table is just an array  
Often objects gets a hash method to generate a integer, then run through the mod % hash to get the final index value  
Empty spots in the array are set to null  
Use hash value to instantly loop up the index of any item  
Insert,Delete,Find is O(1). Unless calculating the hash value is very expensive to generate for each instance.  

Once an array data set is full, you have to grow it and copy data over.  

### Hash Function
A function that takes any item as an input and produces an integer as output  
The same object should always return the same number when run through the function (cannot be random)  
If object1.equals(object2) it must return the same integer for both objects  
Good hash functions return evenly distributed numbers for the input items  
It is not required that two non-equal objects have different values. It can be possible for two different objects to result in the same integer value  

Hash functions are single direction  
We should not be able to reconstruct the input object based on the hash integer  
“Cat” -> 3  
3 -> ???  

### Collisions 
When non-equal objects map to the same index  
Adding 12,15,17,49,90 based on % 10  
Adding 42 would return 2, which is the same location 12 is currently at  
Multiple ways to handle it  
**Linear Probing** - walk forward one index by one index until you find an empty index  
Contains(42) should return true  
Start at 2 and go forward until you find a empty index. If you found it in that set of data, it is true  
If adding at end of array, loop around to find the next open spot   
On insert, using wraparound until you find an empty space  
On search use wraparound until you find the object (true) and empty spot found (false) whole array searched (false)  
**Quadratic Probing**  
**Separate Chaining**  

When you grow the array, you should rehash all values with mod new array size and reinsert them all into the new array.  

Sometimes we delete values that come before other values that would have the same index  
If we have 12, and 42 in the array, and delete 12  
Contains(42) will point to an empty spot because due to linear probing we had to place it one index in further  
Instead of deleting the item and setting the index value to null, set a flag saying the value there is deleted  
**Lazy deletion** - have a bit array that marks the index value as deleted  
Bit[size of array] - bits of 0 and 1 and see if at index value is 0 or 1  
HashMap is a wrapper around both the array of data and array of bits marking deletion  
Update both when things are added/deleted  
Ignore things set as deleted  
Creates three states:  
**Empty** - nothing has been there at all  
**Active** - filled and value is current  
**Deleted** - filled but value is ignored for contains

When we do a contains , use bit array to ignore values flagged as deleted  
When we grow array of data, use bit array to not copy over values flagged as deleted

If there are no collisions, performance is O(1), but this degrades as table data grows, and collisions occur  
To determine the real cost, define the load factor (λ) - the fraction of the table that is full  

```O<=λ<=1λ```

If each prove into the table, the probability that spot is occupied is λ  

The average number of cells examined on an insert is 1/(1-λ)  
If λ = 0.5, average of 2 cells is examine on each insert  
If a table is very “empty” it might indicate that the hash is grouping items too closely together, might be wasting memory space