## Week 12 - Day 2
### Mutex vs Barrier
A mutex ensures only one thread executes a block of code (a critical section).

* Necessarily prevents other threads from entering
* Synonymous with “lock”
* Typically acquired (locked) and released (unlocked) explicitly by programmer
* Enables concurrency in a larger program
* Disables parallelism in critical section to prevent data races

A barrier waits for all threads to catch up before allowing even a single thread to go past.

* Prevents any thread from crossing barrier
* Can be implemented using a mutex and condition variables (think about this)
* Typically NOT acquired and released by the programmer
* Is just placed wherever we need a synchronization point in the code
* Does not disable parallelism in section of code after barrier

### Parallel Architecture  
Parallel Programming is hard. There are a lot of Architectures  
There is no canonical parallel computer.  
There is a diversity of parallel architectures  
Hence, there is no canonical parallel programming language  
Architecture has an enormous impact on performance  
We wouldn’t write parallel code if we didn’t care about performance  
Many parallel architectures fail to succeed commercially. We can’t always tell what is going to be around in N years

Computation and data partitioning focus a single processor on a subset of data that can fit in nearby storage  
Can achieve performance gains with simpler processors  
Even if individual processor performance is reduced, throughput can be increased  
End of the GHz wars and move to multicore.  
Complements instruction-level parallelism (ILP) techniques  
Instruction pipelining  
Hyper-threading  
Out-of-order execution  
Speculative execution. 

### Classes of Parallel Architecture
Shared memory multiprocessor architectures

* Collection of autonomous processing units connected to a memory system.
* Global address space where each processing unit can access each memory location.

Distributed memory architectures

* Collection of autonomous systems with an interconnect.
* Each system has its own distinct address space, and systems must explicitly communicate to share data.
* Clusters of PCs connected by commodity interconnect is the most common example.

### Shared Memory Programming
A shared-memory program is a collection of threads.  
Threads are created at program start or possibly dynamically  
Each thread has private variables, e.g., local stack variables  
Also a set of shared variables, e.g., static variables, shared common blocks, or global heap.  
Threads communicate implicitly by writing and reading shared variables.  
Threads coordinate through locks and barriers implemented using shared variables.

### Distributed Memory Programming
A distributed-memory program consists of named processes.  
Process is a thread of control plus local address space -- NO shared data.  
Logically shared data is partitioned over “local” processes.  
Processes communicate by explicit send/receive pairs  
Coordination is implicit in every communication event  

### SPMD: Single Program, Multiple Data
There is one copy of the program, and all threads are executing it  
Essentially the same as MIMD (Multiple Instructions, Multiple Data)  
Matches the pthreads examples from last time  
Distinguish which processor and which data using implicit or explicit thread id