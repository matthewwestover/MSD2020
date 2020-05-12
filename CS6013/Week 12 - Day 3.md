## Week 12 - Day 3
### OpenMP
OpenMP is a library that helps with thread creation and management. 

Attempt to evaluate value of pi

Serial Code

```
static long num_steps = 100000; 
double step;
int main() {
    int i;
    double x, pi, sum = 0.0;
    step = 1.0 / (double)num_steps; 
    for (i=0; i<num_steps; i++) {
        x = (i+.5)*step;
        sum += 4.0 / (1.0+x*x); 
    }
    pi = step * sum; 
}
```

Attempt 1

```
#include <omp.h>
static long num_steps = 100000; double step;
#define NUM_THREADS 2

void main () {
    inti, nthreads; doublepi, sum[NUM_THREADS]; 
    step= 1..0/(double) num_steps; 
    omp_set_num_threads(NUM_THREADS);
#pragma omp parallel
{
    int i , id,nthrds;
    double x;
    id = omp_get_thread_num(); 
    nthrds = omp_get_num_threads();
    if (id == 0) 
        nthreads = nthrds;
    for (i=id, sum[id]=0.0;i< num_steps; i=i+nthrds) { 
        x=(i+0.5)*step;
        sum[id]+= 4.0/(1.0+x*x);
    }
}
for(i=0, pi=0.0;i<nthreads;i++)pi += sum[i] * step;
```

Algorithm strategy:
The SPMD (Single Program Multiple Data) design pattern  
Run the same program on P processing elements where P can be arbitrarily large.  
Use the rank ... an ID ranging from 0 to (P-1) ... to select between a set of tasks and to manage any shared data structures.

This pattern is very general and has been used to support most (if not all) the algorithm strategy patterns.  
MPI programs almost always use this pattern ... it is probably the most commonly used pattern in the history of parallel programming.

Calculating pi run time ~1.83 seconds

Poor run time due to:  
Threads on different processors can modify variables on the same cache line.  
Invalidates the cache line due to the modern CPU design.  
Cache lines will constantly be written to memory and loaded (“sloshing”).  
Degrades performance.  
Solution: Pad arrays so elements you use are on distinct cache lines.  

Attempt 2:

```
#include <omp.h>
static longnum_steps= 100000;
double step; 
#define PAD 8 // assume 64 byte L1 cache line size
#defineNUM_THREADS 2 
voidmain () {
    int i, nthreads;
    double pi, sum[NUM_THREADS][PAD];
    step= 1..0/(double) num_steps; 
    omp_set_num_threads(NUM_THREADS); 
    #pragmaomp parallel {
        inti, id,nthrds; 
        double x;
        id= omp_get_thread_num(); 
        nthrds= omp_get_num_threads();
        if(id == 0) 
            nthreads= nthrds;
        for (i=id, sum[id]=0.0;i< num_steps; i=i+nthrds) {
            x=(i+0.5)*step;
            sum[id][0] += 4.0/(1.0+x*x);
        } 
    }
    for(i=0, pi=0.0;i<nthreads;i++)
        pi += sum[i][0] * step;
}
```

Calculating pi run time ~1.83 seconds  
Padding arrays requires deep knowledge of the cache architecture.  
Move to a machine with different sized cache lines and your software performance falls apart.  
There has got to be a better way to deal with false sharing.

Synchronization:  
Bringing one or more threads to a well defined and known point in their execution.  
The two most common forms of synchronization are:  
Barrier: each thread wait at the barrier until all threads arrive.  

```
#pragma omp parallel
{
    int id=omp_get_thread_num(); A[id] = big_calc1(id);
    #pragma omp barrier
    B[id] = big_calc2(id, A); 
}
```

Mutual exclusion: Define a block of code that only one thread at a time can execute.
Critical Mutex:

```
float res;
#pragma omp parallel
{ float B; int i, id, nthrds;
id = omp_get_thread_num();
nthrds = omp_get_num_threads(); for(i=id;i<niters;i+=nthrds){
B = big_job(i);
#pragma omp critical
res += consume (B);
} }
```

Atomic Mutex:

```
#pragma omp parallel
{
double tmp, B;
B = DOIT();
tmp = big_ugly(B);
#pragma omp atomic
X += tmp; 
}
```

Attempt 3: “critical"

```
#include <omp.h>
static long num_steps = 100000; 
double step;
#define NUM_THREADS 2
voidmain ()
{ 
    double pi; 
    step= 1..0 / (double) num_steps;
    omp_set_num_threads(NUM_THREADS); 
    #pragma omp parallel
    {
        inti, id,nthrds; 
        doublex, sum; 
        id= omp_get_thread_num();
        nthrds= omp_get_num_threads();
        if(id == 0) nthreads=nthrds;
            id = omp_get_thread_num(); 
        nthrds= omp_get_num_threads();
        for (i=id, sum=0.0;i< num_steps; i=i+nthreads) { 
            x= (i+0.5)*step;
            sum+= 4.0/(1.0+x*x);
        }
    #pragma omp critical
        pi+= sum*step;
    } 
}
```

Calculating pi run time ~1.83 seconds

Attempt 4: “Atomic"

```
#include <omp.h>
static long num_steps = 100000; 
double step;
#define NUM_THREADS 2

voidmain ()
{ 
    double pi; 
    step= 1..0/(double) num_steps;
    omp_set_num_threads(NUM_THREADS); 
    #pragma omp parallel
    {
        inti, id,nthrds; 
        doublex, sum; 
        id= omp_get_thread_num();
        nthrds= omp_get_num_threads();
        if(id == 0) nthreads=nthrds;
            id = omp_get_thread_num(); 
        nthrds= omp_get_num_threads();
        for (i=id, sum=0.0;i< num_steps; i=i+nthreads){
            x= (i+0.5)*step;
            sum+= 4.0/(1.0+x*x);
        }
        sum= sum*step;
        #pragma atomic
            pi += sum ;
}
```