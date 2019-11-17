## Week 2 - Day 5
### Queue and Stacks
Pop = ```return list[top--]```  
This returns the top item then changes the stored top’s index value by 1

We usually assume expressions as “infix”, left to right with an unclear order of operations  
1+2\*3 = 7, but some calculators will return 9  
You have to code in additional rules to ensure it returns in the correct order of operations  
Reverse Polish notation -postfix notation  
To operands followed by an operator  
ab+  
123\*+ = (2\*3) + 1  
2\*3 is evaluated first  
To create it work the same way as finding {,(,[ ], ),} push and pop on stack  
Operand is seen, push to stack.  
When an operator is seen, right then left operands are popped, operation is evaluated, and result is pushed back to stack.  
123\*+4-  
Push 1, 2, 3 onto stack  
Pop 3,2 and multiply  
Push six to stack  
Pop 6, 1 and add  
Push 7, push 4 to stack  
Pop 4 (r) pop 7(l) and subtract  
7-4  
Final value is 3;  
Infix is 1+2\*3-4  
Due to OoO = (1+(2\*3))-4  

Queues are first in first out data structure **FIFO**  
Insert on the back, remove from the front  
Operations  
**enqueue** - adds an item to the back  
**dequeue** - removes and returns the item at the front  
These operations should be O(1);  
Queues act as a buffer - most common usage in CS  
Process jobs in the order which they arrived  
Data centers (google search, facebook logins, etc)  
Network switches( handle packets in order)  
Graph traversal algorithms  

Queues as arrays.  
Enqueue advances the back index value  
Dequeue advances the front index value  
When back reaches the end of the array you can:  
Loop around if front of array is empty  
Extra care has to be taken so front and back don't point to the same place, unless array only has one object  
To loop around and print all values in a circular call  

```
For(int I = 0; I< numElements; I++)
    Print arr[(front+i) % arr.length]
```

Using wrap around all operations are O(1) on average, traversing the whole array is O(N)   
Or grow array for more space  
You can use a combination to grow when circular fills. In the new array, copy from front to back, and remove the circular loop in the new array.  
G,h,c,d,e,f -> c,d,e,f,g,h, space  
You can then loop back around once the new space is filled and there is empty space at the front   
If numElements = length grow, else loop  

Queueing as linked lists  
Adding and removing to queues as linked lists is O(1)  
Front is head, back is tail  
No messy wrapping around, or growing arrays.  
Change pointer in the list  

Both Arrays and Linked lists are O(1) for enqueue and dequeue  
Arrays are just more complicated to do effectively  
Linked lists take more work to set up but management once done is easier  
Well implemented linked lists make stacks and queues trivial to manage  
Arrays can be faster to traverse due to being contiguous data and not split in memory  

**Priority Queue**   
Dequeue always returns item with the highest priority instead of the front  
Linked listed, when you enqueue always place it in the right index value in the list  
Dequeue still returns the front of the list as O(1) but to enqueue is O(N)  

### Midterm Review
Short answer: what’s worst, best runtime of these, how many inversions, etc  
    Ex: insertion sort average is number of items + inversions O(N+I) best is O(N) worst is O(N+N) = O(N^2)  
Big O complexity with loops to work out complexity  
    Show work for partial credit  
    Loops dont depend on each other inner\*outer, if they do show the work  
What is the output of this java code snippet?  
Large 20 point questions - compare algorithms  
Explain best, worst,average with input examples  
Study and understand the ALGORITHMS and Big O analysis  

High level review  
Highest level is sorting algorithms - never use selection sort or bubble sort, but understand them  
Selection is O(N^2) in EVERY case. It doesn’t know if it is sorted or not it has to loop through all of them  
InsertionSort is better. Best case O(N) when it is sorted and it just verifies it is sorted. Worst case is O(N^2)  
Shellsort is insertionSorts big brother. It is swap over big gaps. Recursive down to functional insertionSort,  
Picking shell sort over merge/quicksort you can mathematically prove it. Up to ~100k N^1.25 is slower growing than N log N. (Without a constant) So when you know data input size you can decide if shellsort is good  
Binary search is divide and conquer method. It works only on left or right. It assumes the array is sorted and only picks one side or the other.  
Sorting array first, is N log N + log N to search, just linear search is N which is faster. Only sort then search if you need the sorted array for something later