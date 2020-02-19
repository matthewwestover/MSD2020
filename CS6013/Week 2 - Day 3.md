## Week 2 - Day 3
### Typical OS Structure
* User Level - typically all most people ever see
    * Applications
    * Libraries: many common “OS” functions
    * Example: malloc() vs sbrk()
* Kernel Level
    * Top Level: machine independent
    * Bottom level: machine dependent
    * Runs in a protected mode
    * Needs a way to switch between user and kernel
* Hardware Level
    * Device maker exports known interface
    * Device driver initiates operations by writing to device registers
    * Device uses interrupts to signal completion
    * DMA offload work but has restrictions

Traps, interrupts, etc trigger the switch between user and kernel mode

### Processes
#### Isolating processes
There are a lot of running processes on a computer  
Each has its own code and data  
One application can have multiple processes running at once  
Each need to interact with devices, memory, CPU  
Multiplexing is how we distribute resources between these processes. It needs to be safe and efficient  

**Process** - execution context of a running program  
It is an instance of a program, it is not the same as the program   
Many copies of the same program can be running at the same time  
OS manages a variety of activities such as: user programs, batch jobs, and scripts, system programs  
Each of these activities are encapsulated in a process  
Everything that happens is a process. But the OS itself is not a process  

Process state consists of: 

* Memory state: code, data, heap, stack
* Processor state: IP, registers, etc.
* Kernel state: 
    * Process state: ready, running, etc
    * Resources: open files/sockets, etc
    * Scheduling: priority, CPU time, etc

Address space consists of:

* Code
* Static data
* Dynamic data(heap and stack)

Special pointers in OS:

* IP: current instruction being executed
* brk: top of heap - explicitly moved
* sp: top of the stack, implicit moved

**Process Block Control** (PBC) tracks all of the process state
There is one PBC per process running, allocated in Kernel memory
PDC specifically tracks: 

* Process state (running, waiting, ...)
* PID (process identifier, often a 16-bit integer)
* Machine state: IP, SP, registers (if not currently running)
* Memory management info
* Open file table (open socket table)
* Queue pointers (waiting queue, I/O, sibling list, parent, ...) • Scheduling info (e.g., priority, time used so far, ...)

On creation: allocate PCB, initialize, put on ready queue (queue of runnable processes)  
On exit: clean up all process state (close files, release memory, page tables, etc)

Process State Machine

* Each process has a state:
    * new: OS is setting up process
    * ready: runnable, but not running
    * running: executing instructions on CPU
    * blocked: stalled for some event (e.g., IO)
    * terminated: process is dead or dying
* Process moves from state to state as a result of its actions or external actions
    * Program: sleep(), IO request, ...
    * OS action: scheduling
    * External: interrupts, IO completion
* Key to efficiency and high utilization
    * Blocked processes yield CPU 
    * They are just data structures

These switch on the fly. To the user it looks like they are running simultaneously.  
Modern CPUs have multiple cores that can handle multiple running at a time
Each core only has one process running at a time, they switch rapidly