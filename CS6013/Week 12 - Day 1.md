## Week 12 - Day 1
### Intro to Parallel Algorithms
Worry about speed up instead of safety.  
Speed up = sequential execution time / parallel execution time  
The smaller parallel execution time, the greater the speed up  
Amdahl’s law says eventually there is a limit of speed up. Sequential execution  HAS to take time and that is a hard limit parallelism can’t break past  

To develop parallel algorithms we need to think about several things  
Ignore architectural details for now (next time)  
Assume we are starting with a sequential algorithm and trying to modify it to execute in parallel  
Not always the best strategy, as sometimes the best parallel algorithms are NOTHING like their sequential counterparts  
But useful since you are accustomed to sequential and now concurrent algorithms

We are very good at thinking about sequencing and less good at parallel thinking

Designing Strategy  
Data or Domain Decomposition -Divide up the data  
Task Decomposition - Divide up the work  
Synchronization/Communication - Keeping data values consistent  
Limit Overhead - Extra work for parallel algorithm vs. sequential  

Data/Task decomposition are both VERY different. Many libraries provide both options

A **race condition** exists when the result of an execution depends on the timing of two or more events.  
A **data dependence** is an ordering on a pair of memory operations that must be preserved to maintain correctness.  
**Synchronization** is used to sequence control among threads or to sequence accesses to data in parallel code.

Pthreads - Standard interface for ~60 functions that manipulate threads from C programs.  
Creating and reaping threads

```
pthread_create()
pthread_join()
```

Terminating threads

```
pthread_cancel()
pthread_exit()
exit() [terminates all threads] 
RET [terminates current thread]
```

Synchronizing access to shared variables

```
pthread_mutex_init
pthread_mutex_[un]lock
```

Barrier synchronization

```
pthread_barrier_init
pthread_barrier_wait
```

Barriers are an important concept for parallelism. 

Example - Summing list of Ints

```
int _computeseqsum(int *_iplist, int size) {
    int i;
    int sum = 0;
    for (i = 0; i < size; i++) {
        sum += _iplist[i]; 
    }
    printf("sum = %d\n", sum);
    return sum; 
}
```

Parallel Approach:  
Each thread computes a sum of a subset of the integers, typically roughly size/t
size is the number of integers, t is the number of threads  
These local sums are “reduced” into a global sum  
Abstract computation:  
Pack up inputs and outputs into a structure  
Make a parallel function call to create and invoke threads  
Join the threads together when done  
Use safe and efficient technique to derive the global sum  

With PThreads

```
// Pack arguments into custom structure
typedef struct parstruct { int *ip; int size, t, my_id;} ps; 
ps tmp[thread_count];
for (int i=0; i<thread_count; i++) {
    tmp[i].ip = _iplist;
    tmp[i].t = thread_count; 
    tmp[i].size = size; 
    tmp[i].my_id = i;
    // create each thread and invoke function in parallel
    pthread_create(&thread_handles[i], NULL, _computeparsum, (void *)&tmp[i]); 
}

// join threads and return to single-threaded execution
for (int i=0; i< thread_count; i++) { 
    pthread_join(thread_handles[i],NULL);
}
```

With each thread getting its own copy of the entire array, you have to create block sizes, start and end points for each thread as it does its work

```
void *_computeparsum(void *pt_args) {
    // unpack arguments (not shown)
    ...
    int block_length_per_thread = size/t; // assume evenly divisible 
    int start = my_id * block_length_per_thread;
    int end = start + block_length_per_thread;
    for (i=start; i<end; i++) {
        sum += _iplist[i]; 
    }
}
```

#### Issues
Dependence on sum across iterations/threads – But reordering ok since operations on sum are associative  
Load/increment/store must be done atomically to preserve sequential meaning

To correct this - add a mutex lock

```
void *_computeparsum(void *pt_args) {
    // unpack arguments (not shown)
    ...
    int block_length_per_thread = size/t; // assume evenly divisible 
    int start = my_id * block_length_per_thread;
    int end = start+block_length_per_thread;
    for (i=start; i<end; i++) {
        (void) pthread_mutex_lock(amutex);
        sum += _iplist[i];
        (void) pthread_mutex_unlock(amutex);
    }
}
```

This means only one thread can update the global sum at any time. This completely removes the parallelism we did. So to get the correct result we removed the speed. 

To fully utilize both speed and correct results, we store a local variable for each thread, and then lock the update to the global at the end of the internal local summation 

```
    int my_sum = 0;
    int block_length_per_thread = size/t;
    int start = my_id * block_length_per_thread; int end = start+block_length_per_thread;
    for (i=start; i<end; i++) {
        my_sum += _iplist[i]; 
    }
(void) pthread_mutex_lock(amutex);
sum += my_sum;
(void) pthread_mutex_unlock(amutex);
```

Basically each thread calculates a local sum, the threads finish this task at different times, and its update to the global one is still serialized. 

The lock can be removed by having a “master” thread that holds an array. The other threads input their local sums to the array and the master thread can sum from that array. 

This can be an issue as the master thread can’t run until all these other threads finish.  
To make the master thread wait we use a barrier   
Barriers can use spin locks and condition variables to execute.  
Everythread has the barrier, and when it hits, it stops. Once ALL threads hit the barrier, anything after it is executed.  
This allows the master thread to know the other summations are done and can finish the calculation

```
int lsum = 0;
int block_length_per_thread = size/t;
int start = my_id * block_length_per_thread; 
int end = start+block_length_per_thread;
for (i=start; i<end; i++) {
    lsum += _iplist[i]; 
}
my_sum[id] = lsum; // my_sum is a global variable
int rc = pthread_barrier_wait(abarr); // barrier
if (my_id == 0) { // master thread sum = my_sum[0];
    for (i=1; i<t; i++) 
        sum += my_sum[i];
}
```