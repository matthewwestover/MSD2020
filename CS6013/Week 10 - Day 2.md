## Week 10 - Day 2
### Condition Variables
There are more complicated problems then just preventing multiple threads hitting the same critical section. Locks handle that well but isn’t enough
Wait until the account balance is positive?  
How do we make a producer thread wait for a free buffer, and a consumer thread wait for a full buffer? (Output buffer and input buffer)  
How can we implement a “lock” where many reader threads can execute inside the critical section at once, but only one writer?

We do this with condition variables  
**Option 1**: spin until it happens - this is wasteful  
**Option 2**: sleep in a queue until condition is met  
Relies on a parent/child and waking up the parent  
Queue of threads waiting on “event” inside of a critical section  
A condition variable is always paired with a lock

* wait(lock)
    * Atomically release lock and go to sleep
    * When thread wakes up, it atomically reacquires the lock
    * Put onto a CV-specific wait list
* notify_one()
    * Wake up thread waiting on event → no-op if nobody is waiting
* notify_all()
    * Wake up all threads waiting on event → no-op if nobody is waiting

### Join V1
```
Parent
void join() {
    c.wait();
}
Child
void thr_exit() { 
    c.notify_one()
}
```
This is an issue  thread scheduler doesn’t know one is a parent and one is a child.  
If notify_one() is called before wait(), the parent will sleep forever

### Join V2
```
Parent
void join() { 
    if (done == 0)
         c.wait(); 
}
Child
void thr_exit() { 
    done = 1; 
c.notify_one();
}
```
Same issue, there can be an interrupt between the if (done == 0) and the wait()

Solution: hold a lock and release atomically with wait (same as setpark(); guard = 0; park() from before).

Waiter

```
lock l(m);
if (!cond)
c.wait(l); 
```

Waker

```
lock l(m)
// Work that establishes cond.
 c.notify_one();
```
 
Lock ensures

* Checking condition & modifying it remain mutually exclusive
* Checking condition & sleep on it remain atomic
wait() must unlock mutex atomically w/ going to sleep
* If mutex not released, waking thread cannot make progress
* If release is not atomic, then race condition

### Semantics
Mesa semantics:

* Thread that notifies keeps the lock.
* Waiting thread waits for the lock.

Hoare semantics:

* Thread that notifies gives up lock and waiting thread gets the lock.
* Waiting thread releases lock when it exits or waits again.
* Lots of responsibility for the wait method

Mesa semantics are easier to implement. And the standard

### Join V3 - Correct
```
Parent
void join() {
    lock l(m);
    while (done == 0)
        c.wait(l);
}
Child
void thr_exit() { 
    lock l(m);
    done = 1;
    c.notify_one();
}
```

### CV Hygiene
* Shared state determines if condition is true or not
* Check the state before waiting on cv
    * In a while loop
* Use a mutex to protect
    * shared state on which condition is based, as well as, 
    * operations on the cv
* Acquire the mutex before calling notify_one()

### Bound Buffer/Producer Problem - Common issue
Very common. UNIX pipes uses this. Streaming etc all have a buffer writer and buffer reader  
Producer needs to not write when full, reader needs to not pull when empty

```
Producer
Lock l(m);
for (int i=0; i<loops; i++) {
while (numfull == MAX)
    c.wait(l);
do_fill(i); 
c.notify_one(); 
l.unlock();
}

Consumer
lock l(m); 
while (1) {
while (numfull == 0) 
    c.wait(l);
int tmp = do_get(); 
c.notify_one() 
l.unlock(); 
printf("%d\n", tmp);
}
```
Multiple consumers can use notify_one() which might just wake up another consumer, not the producer  
This means the producer never wakes up  
The fix is for the consumer to do c.notify_all()