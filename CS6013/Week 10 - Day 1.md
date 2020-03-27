## Week 10 - Day 1
### Spinlocks and Blocking Locks
Critical section: code that accesses a shared resource and must not be concurrently executed by more than one thread  
One approach is mutual exclusion. Thread A is in the critical section, Threads B and C cannot be

### Locks
Goal: Provide mutual exclusion (mutex)  
Three common operations:

* Allocate and Initialize
    * std::mutex my_lock
* Acquire
    * Acquire exclusion access to lock;
    * Wait if lock is not available (some other thread in critical section)
    * Spin or block (relinquish CPU) while waiting
    * my_lock.lock();
* Release
    * Release exclusive access to lock; let another thread enter critical section 
    * my_lock.unlock();
* We did this with hardware atomic operations

Atomic operation: No other instructions can be interleaved  
Special “Atomic” Hardware instructions:

* Test & Set
* Compare & Swap
* Fetch & Add

Basically when a thread is in the critical section, the other threads “spin” waiting to run it.  
Fetch and Add is a “ticket” system

Fetch and Add doesn’t guarantee efficiency, however it DOES guarantee that all threads will eventually run.  
You can add ```Yield();```  
This means time can be given up so time isn’t wasted doing “nothing”

Tickets improve fairness, yield improve performance  
With many threads, you can hit a bunch of threads that don’t matter, before getting to the one that matters for the critical section

Spinlock performance is CPU wasteful  
Without yield: O(threads * time_slice)  
With yield: O(threads * context_switch)  
With tons of contect_switches, this creates thread contention  
So even with yield, spinning is slow with high thread contention  
Next improvement: Block and put thread on waiting queue instead of spinning

### Blocking Locks
```
int locked = 0; 
int guard = 0; 
queue_t q;
Acquire() {
    while(tas(guard,1));
    if (locked) {
        queue_add(&q, gettid()); 
        guard = 0;
        park();
    } else { 
        locked = 1; 
        guard = 0;
    } 
}
Release() { 
    while(tas(guard,1)); 
    if (queue_empty(&q))
        locked = 0;
    else
        unpark(queue_remove(&q)); 
    guard = 0;
}
```

Unlike spin locks, this has both locked and guard variables.  
```while(tas(guard,1));```

This is a test and set command. It returns the value of guard, and sets it to the input.  
This means that thread 1 passes through, hits this to acquire the lock, guard gets set to 1 and the else section of the statement is hit. If thread 2 tries to acquire prior to thread 1 finishing the else statement, it spins until it is done. Once locked is set and guard returns to 0 by thread 1, this releases thread2, which hits the locked portion of the if/else statement

Acquire() and release() occur around the critical section  
Park() and unpark() have to deal with managing the queue of threads needing to access the critical section  
Park() puts a thread to sleep  
unpark() wakes a thread up

Release() doesn’t unlock automatically. If there is something in the queue it passes the lock to the next thread. 

There is a potential race condition within this code  
Assume Thread 2 holds the lock. Thread 1 about to park.  
Could switch to Thread 2 right before Thread 1 parks.  
If Thread 2 finishes its work, and releases the lock, control returns to Thread 1.  
Thread 1 will now call park() forever. Nothing happens to trigger the unpark.  
Something cannot unpark if it holds the lock