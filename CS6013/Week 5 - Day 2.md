## Week 5 - Day 2
### Virtual Memory - Address Translation

Uniprogramming - one process runs at a time  
This slows things down, processes can destroy the OS  
We want want multiprogramming where each process thinks it is the only the process running

Multiprogramming Goals:

* Transparency
    * Process not aware that memory is shared
    * Works regardless of number and/or location of process
* Protection
    * Secrecy: cannot read data of OS or other processes
    * Integrity: cannot change data of OS or other processes
* Efficiency
    * Do not waste memory resources (minimize fragmentation)
* Sharing
    * Cooperating processes can share portions of address space

Abstraction: **Address Space**  
Address space: A process’ set of addresses (map to bytes of memory)  
This is the PCB - stack, heap, code, memory for data  
This is the set of addresses a process owns.  
Physical address are in physical memory  
Virtual address are made up addresses  
Processes never owns physical addresses - only virtual addresses that map to physical addresses  
Address space has static and dynamic component  
Static: code and come global variables  
Dynamic: stack and heap  

### Dynamic Memory
Why do we need Dynamic Memory?  
Processes dont know the amount of memory needed at compile. 
Assume the worse when allocating memory statically  
Allocate for worst case; storage usage is inefficient  
If process is recursive, it won’t know how many times it has to be nested  
There are complex data structures: lists and trees

**Stack** - memory is freed in opposite order from how it was allocated

```
Alloc(a), Alloc(b), Alloc(C)
Free(c)
Alloc(d) // d goes into where c was
free(d), free(b), free (a)
```

Has a stack top pointer - the rsv (esv) register pointer  
Allocate increases pointer address  
Free decreases pointer address  
There is no fragmentation - impossible to have empty space bordered by occupied memory on top and bottom  
It is automatic, the compiler adjusts the stack pointer on entry/exit and calls to alloc/free

Stacks are used by the OS for procedure call frames. Every function gets chunks, local variables, etc.

**Heap** - memory region where alloc/free are explicit.   
Memory goes where it can fit, so there are chunks of empty space - **fragmented**  
Advantage - alloc lifetime is independent of call/return and works for all data structure  
Disadvantages - alloc can be slow(cant just move to the next address), end up with small chunks of empty space, forget to free areas of memory, can free memory more than once  
OS gives big chunks of free memory to process, and the library manages individuals allocations

**Memory Access** - code compiles to assembly. 
Addresses in assembly give things like address 0x10, 0x13  
These are listed in the PCB.  
There is a OS table that maps these commands from the PCB to the actual physical memory

#### Management
There are multiple ways to manage virtual memory access - especially tricky with multiple processes running at the same time. 
Multiple processes could produce the same “address” to use:
##### Time Sharing 

* Primitive approach, not really used unless on specific focused hardware
* Similar to how the OS virtualizes CPU
    * Take current running stuff save to disk/load from disk when starting up
* OS give illusion fo many virtual CPUs by saving SPU registers to memory when a process isn’t running
* This is a very slow process as it is constantly writing and reading on disk. Disk memory is significantly slow than ram
* Could give illusion of many virtual memories by saving memory to disk when process isn’t running
* Works great for CPU, not for Memory
* Memory needs space sharing - memory is divided across multiple processes

##### Static Relocation

* OS rewrites programs before loading in memory
    * Change program assembly addresses and split it based on process
        * 0x10, 0x13 becomes [0x1010, 0x1030] and [0x3010 and 0x3030]
* Lots of processes run out of memory
* Cannot move the memory for a process once it has been set. Process might run out of space and cannot grow further
    * Loading a program to memory doesn’t know how much memory it really needs to finish running

##### Dynamic Relocation
We want to run processes at runtime and protect processes from overriding each other. 
This requires hardware support: Memory Management Unit (MMU)  
MMU dynamically changes ever process address at every memory reference  
Process generates logical or virtual addresses  
Memory hardware uses physical/real addresses  
The MMU is a specialize CPU chip component - as physical hardware it is very fast in mapping the processes virtual to the physical parts in memory

#### Base and Bound
* All addresses are automatically translated by MMU - hardware does the translation
* OS sets up translation with privileged registers
    * Indicates where the physical address
* User space processes cannot access these registers that map the physical addresses. 
* On memory access of every user process:
    * MMU compares the virtual address to its bounds register (not physical but the max size of the process memory)
        * If process needs 1 kb the bounds register is at 1024. Process needs 0-1024 memory
    * If the virtual address is outside its bounds limit it generates an error
    * MMU adds the base register(where memory for the process starts) to the virtual address to form the physical address in memory
        * Process thinks it has 0-1024 but its physical memory could be 4 kb in. Its 0 is at 4097
* Process thinks it has 0 - end of memory needed. It is actually base + 0 - end of memory needed