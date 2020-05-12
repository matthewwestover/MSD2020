## Week 13 - Day 3
### CUDA
SIMT - Single Instruction Multiple Threads  
CUDA uses the so-called SIMT execution model  
Coined by Nvidia  
Combines SIMD execution within a Block (on an SM) with SPMD execution across Blocks (distributed across SMs)  
Warp = a set of consecutive threads in a block that are scheduled for SIMD execution

Parallelizing Code:

* Allocate input and output data on GPU
* Copy input vectors to GPU
* Launch NUMTHREADS * NUMBLOCKS instances of thread program
* Copy output vector from GPU to host

Threads can access the same memory  
Global memory and shared memory within an SM can be freely accessed by multiple threads  
Requires appropriate sequencing of memory accesses across threads to same location if at least one access is a write  

If threads need to access the same memory location  
Dependence on sum across iterations/threads. 
Load/increment/store must be done atomically to preserve sequential meaning  
Add Synchronization:

* Memory-based: Protect specific memory locations by sequencing their updates
* Control-based: What are threads doing? Stop them at certain points

Definitions:

* Atomicity: a set of operations is atomic if either they all execute or none executes. Thus, there is no way to see the results of a partial execution.
* Mutual exclusion: at most one thread can execute the code at any time
* Barrier: forces threads to stop and wait until all threads have arrived at some point in code, and typically at the same point

Atomic Instructions might be expensive  
Use rarely and judiciously  
Some overhead to use beyond memory latency – involves memory controller  
Slow in the presence of contention – Contention in this case refers to multiple threads trying to perform atomic accesses to the same memory location at the same time

CUDA was designed to incremental develop parallelism - Much like OpenMP does

#### Looping  
Loops without dependencies become kernels!  
If there’s a double loop and one loop carries a dependency, the latter stays intact inside the kernel.  
Identify loops without dependencies and convert to thread and block indices.  
Avoid if statements at the thread level.  
If statements at the warp level are usually okay.  
Warp = 32 threads  
In CUDA, context switching occurs at warp level!  

Common CUDA Coding Pattern:

* Load data into shared memory
* Synchronize (if necessary)
* Operate on data in shared memory
* Synchronize (if necessary)
* Write intermediate results to global memory
* Repeat until done

CUDA programs are written in terms of blocks of threads.  
32 threads = a warp. Use blocks that are multiples of warp size.  
Avoid if statements except at the warp level.  
Threads across blocks can’t see each other.  
Shared memory is much faster than global memory (~100x lower latency).  
Registers are faster than shared memory