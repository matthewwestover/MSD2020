## Week 11 - Day 2
### Deadlock and Concurrency Bugs
Therac-25 - computer controlled radiation therapy machine  
Two modes of operation - low power and high power  
At least 6 incidents, including 3 deaths.  
Patients given massive doses of radiation.  
Most errors were software engineering errors  
One specifically was a race condition which cause high power to function when switched in the past.  
Critical code can be written and concurrency can literally be life or death

Study from 2008 Lu et al.  
Four major projects searched >500k bugs for concurrency issues. ~105 were chosen at random  
Projects were Mozilla, Apache, MySQL, OpenOffice  
Atomicity, Order, and Deadlock were most common issues 

### Atomicity - MySQL
SQL is slow, if you want speed you have to have parallelism and multithreading  
Multiple threads would update and check 

```
thread->proc_info

Thread 1:
if (thd->proc_info) { 
...
fputs(thd->proc_info, ...); 
...
}
Thread 2:
thd->proc_info = NULL;
```
Simple common  solution is mutex and locks

```
Thread 1:
std::mutex m;
m.lock();
if (thd->proc_info) { 
...
fputs(thd->proc_info, ...); 
...
}
m.unlock()

Thread 2:
std::mutex m;
m.lock();
thd->proc_info = NULL;
m.unlock();
```

### Ordering - Mozilla
Some threads have to occur before another can execute. 

```
Thread 1:
void init() { 
...
mThread = PR_CreateThread(mMain, ...);
...
}

void mMain(...) { 
...
mState = mThread->State; 
...
}
```
Here thread 1 sets and initializes the variable mThread, which is needed by thread 2 to execute the mMain()  
To solve this use a condition variable. MtInit lets thread 2 know if it has been run yet

```
Thread 1:
void init() { ...
mThread = PR_CreateThread(mMain, ...);
std::mutex m; 
m.lock(); 
mtInit = 1; 
notify_one(); 
m.unlock();
...
}

Thread 2: 
void mMain(...) { 
...
std::mutex m;
std::condition_variable cv; 
{
std::unique_lock<std::mutex> l (m); // Only lives within this curly brace
while (mtInit == 0)
    cv.wait(l); // continually checks while thread is running that thread 1 has done its thing
}
mState = mThread->State;
 ...
}
```

### Deadlock
No progress can be made because two or more threads are waiting for the other to take some action, and thus neither ever does  
Cooler name: deadly embrace  

Imagine a 4 way stop. Vehicles A and B arrive, and whoever gets there first goes first from the stop.  
Stops can be seen as locks, passing through the intersection can be seen as the critical section  
4 cars appear at the same time in the intersection. The two options are they all go at once, or don’t go at all. Either way, they don’t move very far or clear the intersection

```
Thread 1:
Lock(&A)
Lock(&B)

Thread 2:
Lock(&B)
Lock(&A)
```
These might result in deadlock  
Draw a chart. Thread 1 holds lock A, Thread 2 holds lock B  
Thread 1 wants lock B, thread 2 wants lock A  
This is a circle dependancy. They want the lock the other holds  
Cyclic is a deadlock, acyclic is not deadlock  

One simple fix is to have both call locks in the same order. This prevents them from holding each others lock.  
To reorder locks it is often very more complicated, involves moving code beyond just the locks around

Deadlock’s necessary conditions:  
**Mutual exclusion**: Resources cannot be shared  
**Hold and wait**: A thread is both holding a resource and waiting on another resource to become free  
**No preemption**: Once a thread gets a resource, it cannot be taken away  
**Circular wait**: There is a cycle in the graph of who has what and who wants what

Remove any one of these, deadlock is eliminated. 

Find lock-free algorithms that allows for mutual exclusion

#### Atomic Primatives
```
void add(int *val, int amt) { 
    mutex_lock(&m);
    *val += amt; 
    mutex_unlock(&m);
}

void add(int *val, int amt) { 
    int old;
    do {
        old = *val;
    } while(CompareAndSwap(val, old, old+amt) != old); //Compare and swap is atomic instead of a lock
}
```
Get rid of hold-and-wait  
Get all locks at once. Put them in a “meta”lock that holds them. Unlock the meta when the other locks are needed 

```
lock(&meta); 
lock(&L1); 
lock(&L2);
... unlock(&meta);
// Critical section code 
unlock(...);
```
You need to know all the locks ahead of time for this.  
Another approach is that if a thread doesn’t get what it wants, release what it holds and return to the top of its code to try again

```
lock(A);
if (trylock(B) == -1) { 
unlock(A);
goto top; 
}
```

This can still result in threads just locking and unlocking. You have to adjust the thread rescheduling to account for that. 

#### Supporting Preemption - Very common to get rid of deadlock

Release held resources when new request arrives  
Threads must be coded to expect this behavior  
If application expected mutually exclusive access to resource for duration, then this won’t work  
But, it if just needed ‘atomicity’, could ‘undo’ on preemption. 
Database transactions use this approach

Hardware transactional memory (Eg. Intel TSX)  
Expose special memory API that guarantees atomic loads/stores.  
Can code to this API with knowledge that common concurrency issues won’t occur (“optimistic concurrency”)

#### Eliminating Circular Waiting
Strategy:  
Decide which locks should be acquired before others  
If A before B, never acquire A if B is already held!  
Document this, and write code accordingly  
Works well if system has distinct layers  

When in doubt about correctness, better to limit concurrency (i.e., add unnecessary lock)  
Concurrency is hard, encapsulation makes it harder!  
Have a strategy to avoid deadlock and stick to it  
Choosing a lock order is probably most practical 