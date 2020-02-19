## Week 3 - Day 1
### Processes

#### Process Control Block (PCB)
One is created per process  
Includes: 

* Process state(running, waiting)
* PID (16 bit int id)
* Machine state
* Memory management
* Open file table
* Queue pointer
* Scheduling info

On create:

* Allocates memory for the PCB
* Initialize
* Put on ready queue (queue of runnable processes)  

On exit - Clean up all process state (close files, release memory, page tables)  

#### Process State Queues (PSQ)
The OS tracks the PCBs with a queue  
Ready processes are on the ready queue  
Each I/O has a wait queue (an interrupt causes them to do stuff)  
OS invariant: A process is either running, or on the ready queue, on a single wait queue  
Processes linked to parents and siblings.  
There are ready, disk wait, sleep, running, queues  

A program has to be capable to shift around these different queues. We cannot control the queues  
We can ask the OS to prioritize our stuff, but it doesn’t mean it will  
Order of execution of processes is nondeterministic. You can’t write a program that depends on execution order

OS use to allow users to control user program queues  
It was very inefficient only can run as many processes as there are cores on the CPU  
The OS has better handling of efficiency on the CPU  
For hardcore data science super computers you still need an OS to handle things.   The OS is typically linux and highly stripped down. User submits a process and the super computer handles it. 

#### Process Management
OS manages process
It creates, deletes, suspends, and resumes processes
Schedules processes to manage CPU allocation
Manages inter-process communication and sync
Allocates resources to processes and takes them away

Practical process management - things users can look/touch

```
ps -Af //all running processes
Kill -9 547 // ends process with PID 547
Top //displays dynamic info
```

Create a program that has commands

```
getpid(): returns current process’s PID (process id)
fork(): create a new process
wait(): wait for exit of a child process
exec(): load a new program into the current process
sleep(): puts current process to sleep for specified time

// These are accessible in C/C++ with #include <unistd.h>
```

#### Creating a new process
**Fork();**  
Lower level function, creates a process that is near clone of a parent address space and running state  
Basically clones the process control block to a new process id  
Fork() returns 0 in the child, and the child PID parent (returns twice on call)  
Many kernel resources are shared (open files and sockets)  
Modern OSes limit fork space. Use to be an exploit to “fork bomb” with forking in a loop to take up all memory  

**Wait();** lets a process wait for the exit of the forked child. Spawns a new program use some form of exec();  
**exec();** does not return anything but replaces process’s program  
**exit();** doesn’t terminate the process, but lets the OS know it is ready to be terminated. Does not return anything