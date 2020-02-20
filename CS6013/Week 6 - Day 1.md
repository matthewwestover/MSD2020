## Week 6 - Day 1
### Memory Virtualization
Multiple solutions to visualization  
Time Sharing: Slow  
Static Relocation: too inflexible  
Base & Bound: simple but not amazing

#### Base and Bound
Check process’s virtual address against an upper limit set by the OS if its out of bounds, break.  
Take the virtual address the process knows of, add it to the “base” assigned by the OS  
This gives a physical address in memory for the process.   
This gives a lot of empty space in memory unused because of this.  
Each process has a open set of space for it to grow into as part of its “bounds” that cannot be used by anything else.  
This has to be contiguous in the physical memory.  
Bound Register literally prevents processes from using each others’ physical memory addresses.  

**Advantages**

* Protected (access/no access) across address space
* Supports dynamic relocation
* Simple inexpensive implementation - few registers little logic in MMU
* Fast - add and compare in parallel

**Disadvantages**

* Each process must be allocated contiguously physically in memory
    * Must allocate memory memory that may not be used by the process (free space between stack and heap)
* No partial sharing: cannot share limited parts of address space.
    * Data needs to get copied between processes instead of just looking at the same location

#### Segmentation
Divide address space into logical segments  
Each address corresponds to logical entity in address space  
Code, stack, heap would be their own segments. These can get split up in physical memory  
Each segment can independently get placed, grow and shrink, be protected (separate write/read/execute)  
Often each segment has a read/write bit. So one segment can read the data of another segment. This is needed as they are split in memory for a single process

To designate a particular segment:  
Explicit: use part of the virtual address where the top bits are the segment number, low bits are the offset within the segment  
Each segment gets its own base and bound, bound is the limit for the segment not the process

**Example**  
In virtual memory:  
[CODE] 0kb - 2kb  
[HEAP] 4kb - 7kb  
[STACK] 14kb-16kb  
In physical:  
[CODE] starts at 32kb, heap comes right after.  
[HEAP]34kb  

Virtual address is 100, in physical that is 32kb+100 = 32868

Heap's virtual address is 4kb  
Virtual address 4200. Inside Heap (starts at 4kb) = 104 bytes in the heap  
Heap starts at 34kb  
34kb + 104 = 34920 physical address  

Virtual Address 6200 = 6kb + some in heap  
Heap starts at 4096  
Offset = 2104  
34kb + 2104 = 36920 physical 

**Advantages**

* Enables sparse allocation of address space
    * Stack and heap can grow independently
    * Heap: if no free memory, then sbrk()
* Protections for different segments
    * Read-only status for code
* Enables sharing of selected segments
* Supports dynamic relocation of each segment

**Disadvantages**

* External Fragmentation
    * Memory is full of little chunks of free space, but none of them are large enough for a single new segment.
    * Memory allocation requests may be denied.
* May still be really wasteful.
    * What if the heap is barely used, but it gets its own large segment?
    * Wasted space, contributes to fragmentation, and is not really fine-grained.