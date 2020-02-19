## Week 4 - Day 3
### Scheduling Part 2
Scheduling services need to be I/O aware.  
If a process depends on I/O (monitor, disk read, key read) - split other CPU based things into those slices  
Treat Job A as separate CPU bursts and let Job B run while A is doing its I/O operations

### MLFQ
**Multi-Level Feedback Queue**  
General purpose scheduling  
Must support two job types with distinct goals  
**Interactive** programs care about **response** time  
**Batch** programs care about **turnaround** time  
This is done with multiple queues, for each priority level. 
Pick a higher priority queue to run, if more than one job is in a queue, use the **Round Robin** (RR) on that queue to run. 

**Rule 1**: if priority(A) > priority(B), A runs  
**Rule 2**: if priority(A) == priority(B), run A & B run in RR  

Setting priority can be difficult. It should not be set by the user, as it can break things. This makes it the OS’s responsibility   
It can use **History** - past behavior of a process to predict future behavior  
Processes alternate between I/O and CPU - might merit lowering priority  
Guess how CPU burst will behave based on pasted CPU bursts of this process. 
Things can move up and down in the priority level queues

**Rule 3**: Processes start at the top priority (highest queue) Use RR and provide slices of time to each. 
**Rule 4**: if a job uses its whole slice, demote process, else leave it at the same level

* Process might nearly be done, so it doesn’t need to be on the highest priority
* OR, it might be needing MANY slices, and keeping it on the highest level is bad as it will hog the hardware
* Periodically things get  boosted up in priority so they don’t sit at low level for too long of time
* Interactive processes never uses its whole time slice, so its never demoted, but its CPU processing time stays low

This has problems, long running jobs will never receive CPU time if it is at the bottom of the queue all the time (starvation). 
Also smart users could game the scheduler. By doing something like an I/O burst to prevent a time slice from finishing, that process will never get demoted.

To fix **starvation** all jobs periodically get boosted back up to the highest queue. This is keeping track of processing aging  
To fix **gaming** account for total run time in a queue, downgrade when its threshold is reached. 

```
Rule 1: if priority(A) > priority(B), A runs
Rule 2: if priority(A) == priority(B), run A & B run in RR
Rule 3: Processes start at the top priority (highest queue) Use RR and provide slices of time to each
Rule 4: if a job uses its whole slice, demote process, else leave it at the same level
Rule 5: After some time period, move all jobs to the topmost queue, and repeat
```

#### Lottery Scheduling
Give processes lottery tickets  
Whoever wins runs  
More tickets = higher priority/share  
This is a super simple alternative to MLFQ  

```
int counter = 0;
int winner = getrandom(0, totaltickets);
node_t *current = head; while (current) {
    counter += current->tickets; 
    if (counter > winner) 
        break; 
    current = current->next;
}
// current gets to run
```

#### Scheduling Summary
Understand goals (metrics) and workload, then base a scheduler around that.  
General purpose schedulers need to support processes with different goals (interactive vs batch)  
Past behavior is a good predictor of future behavior  
Random algorithms can be simple to implement and avoid corner cases  

### Interprocess Communication (IPC)
This enables processes to communicate with each other to share information  
This is important because virtual memory is unique and a process can only see their chunk  

* Pipes (half duplex)
* FIFOs (named pipes)
* Stream pipes (full duplex)
* Named stream pipes
* Message queues
* Semaphores
* Shared Memory
* Sockets
* Streams

#### Pipes
Oldest and perhaps simplest form one UNIX IPC  
Half duplex - data flows in a single direction  
Only usable between processes with a common ancestor - parent-child, child-child

In C

```
#include <unistd.h> 
int pipe(int fildes[2]);
fildes[0] is open for reading 
fildes[1] is open for writing
```

The output of fildes[1] is the input for fildes[0]

Create a pipe, fork a process  
Parent and child can now write to the same pipe, parent and child can now read from the same pipe

To send messages from parent to child:  
1. Have parent close the read end of its pipe: close (filedes[0]);  
2. Have child close the write end of its pipe: close(filedes[1]);  
3. Use write() in the parent to write() to the pipe. Write to filedes[1].  
4. Use read() in the parent to read from the pipe. Read from filedes[0].

#### Pipes and Command Line
Chain output of one command into the input of another   
Syntax: pipe command1 | command2  
In Unix, all processes in a pipe are started simultaneously.  
Pipes implement buffering under the hood if read/write speeds of the two processes are different. (Not unlimited in size for buffer, but different read/write speeds can be handled)