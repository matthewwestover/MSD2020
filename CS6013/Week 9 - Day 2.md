## Week 9 - Day 2
### Multithreading
Address is a path through physical memory  
A thread is a path through virtual memory  
Each process can be thought of its own unique world where multiple threads can live

Multiple threads updating the same information means that data doesn’t update correctly.   
Threads don’t operate in a predictable order, means you can get a different output each time.   
Threads don’t have to operate on the same thing, it can be a separation of concerns. This thread does x, this thread does y.  
It is a further break from only separating on a process level

**Critical Section**: code that is shared between threads  
**Race Condition/Data Race**: all threads competing to utilize the critical section  
**Mutual Exclusion (Mutex)**: Lock the critical section -One thread enters, locks the section, unlock once done  
**Data Partitioning**: Split work between threads, then combine once threads are done  
**Atomic Instruction**: Combine Assembly code into one single instruction to be executed. Cannot cross threads if it is a single execution

### Hardware Multithreading
Hardware restrictions limit how threading can work.  
Single core vs multiple cores.   
Limits on available registers for cores to run. 

### Temporal Multithreading
Hardware can execute multithreading  
Single core CPU can only physically run a single thread at a time, you interweave the threads using fast context switches  
```T1, T3, T2, T4, T2, T1, T3, T4…```  
Not “true” multithreading, just the illusion of this

### Simultaneous Multithreading
Single core CPU fetches multiple thread instructions at once.  
This speeds up the temporal switching as the fetch doesn’t have to occur for each thread one after another.   
It requires “large” registers to handle storing multiple instructions at once. 
This relies on the superscalar CPU architecture  
Intel refers to this as hyperthreading  
This doesn’t double the speed because the same CPU is still single core, can only execute one at a time, but loads multiple for faster execution  
This speeds up temporal multithreading by 25-30%

### Multicore Multithreading
Around the mid 2000s CPUs began having multiple cores.  
With multiple cores installed on a CPU, each core can use simultaneous multithreading for true chip-level multiple threads running at the same time.  
Typically a process will split between the cores, and use that thread consistently on that individual core

```
Core 1: T1, T3, T1, T3...
Core 2: T2, T4, T2, T4...
```

This means there is true parallelism between threads operating at once.  
2 cores at 2 gHz is more powerful than 1 core at 4 gHz

Limits to how many cores are used in practice:  
Chip size - surface area for registers  
Heat it generates and power consumption  
The smaller transistors get, the more cores can fit on a chip. Currently down so small have to use an electron microscope to see (~70 silica atoms wide ~.14 nanometers)

### Graphic Processing Units (GPUs)
Massive multithreading - optimized to run thousands to millions of threads in parallel  
Each thread has a lot less memory than a CPU thread would.  
Idea for highly parallelizable applications  
Dedicated purpose. Historically used to quickly draw triangles very fast. 3D graphics then became just a bunch of triangles  
Like a mini-computer itself. Has registers, memory, cache, etc.  
Large portion of GPU programing is attempting to hide the cost of transferring from the GPU to the main memory

### Software Multithreading
Software also has its own approaches to handling multithreading  
These are managed by the OS - OS figures out how to map to the available hardware  
OS exposes two main programming models: User and Kernel threads  
Usually programmers use libraries that handle the software side of this

### Kernel Threads
“Light-weight” processes  
TCBs are kept in the OS kernel rather than inside the PCB  
Managed by system calls (switch to ring 0) - context switch is slow/expensive  
Kernel knows these threads and can optimize scheduling for you

### User Threads
Kernel knows nothing about these.  
Typically managed by a threading library  
TCBs are back in the PCB, PCBs are managed by the kernel.  
This is 100x faster than kernel threads, fast context switch, small memory footprint  
OS can mess up scheduling with the host process that contains the threads as it doesn’t know about the threads running within the thread. (Doesn’t back up each thread state, just the PCB)  
Process time-slice on the CPU is independent of the amount of threads.  
If one thread causes a page fault, the entire process blocks.  

User threads have to map to kernel threads

**Many to One**  
Multiple user threads map to kernel threads  
Complex  
Do not run in parallel

**One to One**  
Map each user level thread to a kernel thread.   
Cost of creating the thread dominates - can get no speed up from multiple threads due to this cost

**Many to Many**  
Multiplex multiple user threads to multiple kernel threads  
Most common

Threads are used for speeding up applications by executing ops in parallel.  
State of the art supercomputers all use multithreading (SMT + MMT + GPUs +…).  
Let S be the proportion of a program that is performed serially.  
Let there be N “processing units” (cores, threads, whatever).  
Amdahl’s Law establishes limits on speedup from multithreading:
```1 / (S + (1-S/N))```  
As N goes to infinity, the speedup limits at 1/S.  
Speedup is always limited by the serial part of the program, regardless of N.

### Dealing with the Race Condition
Possibilities: 

1. “Wrap” the critical section in some structure, only one thread inside it at a time
2. Have a single assembly instruction corresponding to the critical section.

The data structure is called a mutex (mutual exclusion)  
The single instruction is called an atomic instruction  

Atomic instructions are hardware-dependent. Most modern CPUs have these.  
A mutex is a software abstraction. It can be implemented in many ways, even as an atomic instruction.  
C++11 has a mutex ```(std::mutex, <mutex>). - barrier.lock(), barrier.unlock()```  
C++11 also has a software abstraction: an atomic data type ```(std::atomic, <atomic>)```  
The atomic data type is an abstraction of an atomic instruction.

