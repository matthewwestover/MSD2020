## Week 4 - Day 2
### Dispatch Loop
The OS is always running an infinite dispatch loop

```
While (1) {
    Run process A for some time-slice
    Stop process A and save its context
    Load context of process B
}
```

OS can let the process decide to ```yield()``` which allows the kernel to switch processes.  
If the process never yields then it seizes control of the system. Only way to stop is to reboot the whole system  
This is also why drivers/major updates force resets, it gives access to guaranteed registers.  

This isn’t the best set up, given how easy it can be to take control of the whole system

The other option is **Preemptive Multitasking**  
Guarantee the OS can obtain control periodically
Enter OS by enabling periodic alarm clock - hardware generates a timer interrupt
Example once every 10 ms per process  
User cannot mask this timer. The dispatch loop counts interrupts before switching  
Allow 20 slices of 10 ms for a 200 ms total time processing time  
Common allowed times is 10-200 ms

On the interrupt of a process, the context needs to save.  
Process is saved in the kstack, and loaded from a kstack  
EX two processes running:  
Process A is running ->  
the Hardware hit a trap/interrupt, causing the CPU to turn to kernel mode ->  
All context regs(A) is saved to kstack(A). The CPU goes to the trap handler
OS calls switch() saves kstack(A) to PCB(A), it restores regs(B) form PCB(B) and switches to kstack(B) ->  
Returns from the trap/interrupt. Finish restoring the regs(B) from kstack(B) ->  
Process B starts running for an allotted amount of time

| Step |                                                             Operating System                                                             |                                                        Hardware                                                       |     Program    |
|:----:|:----------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------------:|:--------------:|
|   1  |                                                                                                                                          |                                                                                                                       | Process A runs |
|   2  |                                                                                                                                          | Syscall or timer interrupt Hw switches to kstack Raises to kernel mode Save regs(A) to kstack(A) Jump to trap handler |                |
|   3  | Handle the trap Call switch() routine Save kstack(A) to PCB(A) Restore regs(B) from PCB(B) Switch to kstack(B) Return-from-trap (into B) |                                                                                                                       |                |
|   4  |                                                                                                                                          | Restore regs(B) from kstack(B) Move to user mode Jump to B’s IP                                                       |                |
|   5  |                                                                                                                                          |                                                                                                                       | Process B runs |

### Scheduling
With how to switch handled, the CPU still needs to know how to schedule the order of processes to execute, and how much of a time-slice to give to each process  
Transitioning is the “mechanism” and how that happens rarely changes (multithreading has moved us away from multiprocessing)  
How to schedule is the “policy” which does change  
**Workload**: set of job descriptions (arrival time, run time)  
**Job**: View as current CPU burst of a process - Process alternates between CPU and I/O -Moves between ready and blocked queues  
**Scheduler**: logic that decides which ready job to run   
**Metric**: measurement of scheduling quality  

#### Common Metrics
Minimize turnaround time

* Do not want to wait long for job to complete
* Completion_time - arrival_time

Minimize response time

* Schedule interactive jobs promptly so users see output quickly
* Initial_schedule_time - arrival_time

Minimize waiting time

* Do not want to spend much time in Ready queue

Maximize throughput

* Want many jobs to complete per unit of time

Maximize resource utilization

* Keep expensive devices busy

Minimize overhead

* Reduce number of context switches

Maximize fairness

* All jobs get same amount of CPU over some time interval

Workload assumptions:

* All jobs runs for the same amount of time
* All tribes arrive at the same time
* All jobs only use the CPU (No I/O)
* Run time of each job is known

Times below in seconds, which is significantly longer than a normal time for any process.

#### FIFO
**First In, First Out (or FCFS - First Come First Serve)**  
Jobs run in arrival_time order  
Our turnaround is completion time - arrival time  
**Time 0:** A Arrives, B Arrives, C Arrives  
Each takes 10 seconds to run  
**Time 0:** A run for 10 seconds  
**Time 10:** A finishes and B run for 10 seconds  
**Time 20:** B finishes C runs for 10 seconds  
**Time 30:** C finishes  

Average turnaround time (10 + 20 + 30) / 3 = 20 seconds to complete  
This time increases as more jobs arrive

But processes don’t have the same run_time  
Also if A is a bigger job, it can make B and C take a lot longer than it should be. If A takes 60 seconds that becomes ~70s to run even though B and C still take 10 seconds to run

#### SJF
**Shortest Job First**  
Assuming we know the runtime of the all three jobs, doing B and C before a 60 second A  
Average turnaround time from FIFO goes from 70 seconds to ~36.7s (80+10+20)/3  
This improves the turnaround time of short jobs more than it harms the runtime of long jobs

Processes don’t arrive at the same time however. If A is received first, and B and C don’t show up until after A starts, this doesn’t help improve the turnaround time ~63.3

Both FIFO and SJF are **non-preemptive**  
Only schedule new job when previous job voluntarily relinquishes CPU.  
We need a **preemptive** schedule. If A is running, B shows up but would finish faster, it should switch to that, complete it and return to A

#### STCF
**Shortest Time to Completion First**
A Starts for 60 seconds, 10 seconds in B and C arrive. A has run for 10 seconds, then stops. B and C both run to completion, then A resumes  
This takes time back down to 36.7s to complete
 
This is good for batch systems. When we care about when jobs starts however this isn’t so good. We need to examine the response\_time instead of just completion_time  
If A, B, C all take 10 seconds to run, and it is a **FIFO** policy, B has a turnaround time of 20s, but response time is 10s.  

#### Round Robin
If a process is ready, allow an allotted amount of run time, then switch. This alternates ready state processes  
As above, A, B, C run FIFO with 5 second run time. Average response time is 5 seconds. With round robin it can moves down to 1 second response time.  
However time to completion can become much longer (15 seconds for A,B,C)  
If the run-times are unknown, short jobs get a chance to run and finish faster, this is a fair trade

Just calculating we tend to care more about turn_around time. But as soon as we program for users with things like GUI interaction the response time becomes more important 