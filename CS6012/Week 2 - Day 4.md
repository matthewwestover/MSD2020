## Week 2 - Day 4
### Stacks
A data structure where the insertion and removal is restricted to the “top” or “end” of the list  
It is also called “First in, last out” **FILO**  
Insertion is always adds items to the end  
Deletion always removes items from the end  
Push add item to top of stack  
Pop remove item from top of stack and return its value;  
Peek returns top value, but doesn’t do anything to it  
Consecutive calls of pop, remove in reverse order as added  
Useful to think of stacks as standing upright  
Push pop peek must all be **O(1)**  
Using an array to implement, all work as O(1), however pushing can reach end of array size, so growing can be O(N) when array needs to be grown  
Using a linked list to implement

* To push make a new item, make it the new head and link to the prior head  
* To remove, set the head pointer to the .next() of the current head  
    
Stacks cannot be used for ordering/sorting data. But has quick add/remove times  
No resizing is ever required. No wasted empty space like in an array  
Pop, Push, Peek is always O(1);  

**Symbol Matching**  
Java requires every {, [, ( to have a matching }, ], ) to close it out.  
Stack checking of these can be used to verify they all match.  
Look at each character in the file.  
Every time a left brace is found, pushing to the stack  
Any time there is a right brace, pop the stack, if the returned symbol isn’t a match, the symbols are unbalanced  
Once all characters have been read, if there is anything on the stack, the symbols are unbalanced