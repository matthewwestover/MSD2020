## Week 9 - Day 3
### Thread Concurrency
Concurrency leads to non-deterministic results  
Different results even with same inputs  
We call this a race condition  
Whether a bug manifests depends on CPU schedule!  
Critical section: code that accesses a shared resource and must not be concurrently executed by more than one thread  
One approach: mutual exclusion for critical sections • if thread A is in critical section C, thread B can’t

Data race: when there are two memory accesses in a program where both:

* Target the same location
* Are performed concurrently by two threads
* Are not reads
* Are not synchronization operations

Reads are not seen as a data race. If one or both attempt to WRITE based on the same data it is a data race

###Reinforcing Terms
* Race condition: Threads run, result depends on timing
* Data race: unsynchronized non-read-only accesses to shared data
* Synchronization (or Concurrency Control):
    * Using atomic operations to eliminate race conditions
* Critical section: code that must run atomically
* Mutual exclusion: Ensure at most one process at a time
* Lock: Sync mechanism that enforces atomicity via mutual excl.
    * Lock “protects” data: lock() before accessing, unlock() when done

### Locks
Three common operations:

* Allocate and Initialize
    * ```std::mutex my_lock```
* Acquire
    * Acquire exclusion access to lock;
    * Wait if lock is not available (some other thread in critical section)
    * Spin or block (relinquish CPU) while waiting
    * ```my_lock.lock();```
* Release
    * Release exclusive access to lock; let another thread enter critical section
    * ```my_lock.unlock();```

### Lock Requirements and Goals
Correctness

* Mutual exclusion (mutex)
    * Only one thread in critical section at a time
* Progress(deadlock-free)
    * If several simultaneous requests, must allow one to proceed
* Bounded Waiting(starvation-free)
    * Must eventually allow each waiting thread to enter

Fairness

* Each thread waits for same amount of time

Performance

* CPU is not used unnecessarily (e.g., spinning)

### Attempt 1

```
void acquire(lockT *l) { 
    disableInterrupts();
}
void release(lockT *l) {
      enableInterrupts();
}
```

Prevents critical section access. Completely locks the computers processing power until the accessing thread finishes the section

### Attempt 2

```
boolean flag = false; // shared variable 
void acquire() {
    while (flag) /* wait */ ;
    flag = true; 
}
void release() { flag = false; }
```

One thread can pass through with no issue.  
A second thread can the critical section and hit the infinite while loop. Once thread1 finishes, flag is false and thread2 can then run  
However this no guarantee the acquire will happen in one instruction. It can get interrupted as threads switch, so there is no way to verify the thread didn’t get interrupted. 

1. Testing lock and setting lock are not atomic, so not a real mutex
2. Endless waiting (aka spinning) checking the flag

Atomic operation: No other instructions can be interleaved  
Examples of atomic operations

* Code between interrupts on uniprocessors
    * Disable timer interrupts, don’t do any I/O
* Loads and stores of words
    * Loadr1,B
    * Store r1, A
* Special “Atomic” Hardware instructions
* Test&Set - swapping
* Compare&Swap - if value == one arg, swap with second arg
* Fetch&Add - get a value, update the value

These are hardware based methods that can be used to generate a lock in a single command that cant be interrupted by the different threads

```
Locked = 0
Acquire() {
    While (tas(locked,1) // Test and Set
    While (cas(locked, 0. 1) // Compare and Swap
}
Release() { locked = 0}
```