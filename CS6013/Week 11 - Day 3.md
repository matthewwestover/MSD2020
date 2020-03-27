## Week 11 - Day 3
### Lock Free Data Structures
Very few people study lock free data structures. Mainly done for research purposes. Very very complicated to pull off

There are problems with locks  
Deadlock - freezes out system.  
Priority inversion: low-priority processes hold a lock required by a higher-priority process.  
Convoying:

* Several processes need locks in roughly similar order. 
* A slow-to-finish-process gets one first.
* All other processes slow to speed of the first one.

This leads to trying to program without locks  
Thread-safe access to share data without mutexes (locks).  
Requires specialized hardware support.  
Reasoning: mutual exclusion is a way to prevent interruption during important operations.  
We could also make those operations atomic.  
So we need hardware support for atomic instructions.  
We also need software constructs to leverage this support.

Designing lock-free algorithms is hard.  
Instead, leverage software and hardware to design lock- free data structures.  
Most powerful and relevant primitive is **compare-and- swap** (CAS).  
Also often called compare-and-exchange.   
xchg instruction on x86.

### Compare and Swap
```
int cas(int *ptr, int expected, int new){ 
    if(*ptr == expected) {
        *ptr = new;
        return 1; 
    }
    return 0; 
}
```

Compare \*ptr to expected. If equal, set \*ptr to new. Return 1.  
Else return 0.  
Depending on platform, may return the old value instead.  
It is built in, not something you implement yourself.

### Lock Free Counter 
```
void add(int *val, int amt) { 
    do {
        int old = *val;
    } while(CAS (val, old, old+amt) ==0);
}
```

* First time through, *val=old, so CAS(val,old,old+amt) 
* Sets *val to old+amt.
* Returns 1.
* This will always increment val by old+amt!

### Lock Free Stack Push
```
void push(Node* in){
    do{
        Node* next = head;
        in->next = next; 
    } while(!cas(&head, next, in));
}
```

If only one thread was running this, the CAS instruction does the following check:  
Is \*(&head) == next?  
If so, \*(&head) = in, return 1  
Else, return 0

### Look Free Stack Pop
```
Node* pop(){ 
    do{
        Node* ret = head;
        Node* next = ret->next; 
    } while(!CAS(&head, ret, next)); 
    return ret;
}
```

### ABA Problem
Lock-free CAS can lead to the ABA problem

* Thread 1 looks at shared variable, finds that it is A.
* Thread 1 calculates something based on fact that val is A.
* Thread 2 executes, change variable to B.
* If Thread 1 wakes up, CAS will fail and thread 1 retries. Works as intended.
* Thread 2 changes variable back to A.
* Thread 1 never sees the change to B, just sees that the value is now A.

ABA with Stack Push

* Stack has top->A->B->C
* Thread 1 calls pop() but is interrupted after getting B= A.next.
* Thread 2 takes over and pops A, discards it.
* Stack is now top->B->C
* Thread 2 pops B and deletes B.
* Stack is now top->C
* Memory manager recycles A, and Thread 2 pushes A back onto stack.
* Stack is now top->A->C
* Thread 1 still thinks A->next is B, but B is deleted. Program has undefined behavior.

Issue is because pushes and pops are both allowed to happen concurrently.

**Solution**  
Keep an “update count” or a “reference count” with each node indicating how often the node has been modified. Often called an ABA counter.  
Write a garbage collector that doesn’t recycle the memory “too soon” - lazy collection  
Expose a “double-length” CAS instruction. First half holds the usual value  
Second half holds update count. Check is done in in hardware.  
In practice, for a stack, a mutex is a more elegant solution.

### Lock Free Linked List
Insertion node N after node P

```
do{
    n->next = p->next;
    Node *old_next = p->next;
} while(!CAS(&p->next,old_next,n));
```

This causes an issue with concurrent insertion and deletion  
Linked list is A->B->C.  
Thread 1 is deleting B.  
Concurrently, Thread 2 tries to insert E after B. -> B.next -> E  
B gets deleted.  
B is no longer on the list  
E has no where to go

### Problems with Lock-Free Approach
Have to worry about order of operations.  
A little more than in the mutex case.  
While loop with CAS resembles a spinlock.  
Every time you fail the CAS instruction, you do some meaningless operations.  
Thread starvation is still a possibility.

### Summary
Lock-based data structures could induce deadlock.

* Must break 1 of 4 conditions for deadlock.
* The lock.

Could attempt to go lock-free.

* Must deal with ABA problem.
* May not outperform lock-based version.
* Relies on similar hardware support to locks.

Why even go lock-free?

* Potential for improved performance, esp. on multicore systems
* Deadlock is bad (but so is ABA).