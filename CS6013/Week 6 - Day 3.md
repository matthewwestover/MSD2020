## Week 6 - Day 3
### Midterm Review
Questions are conceptual  
Draw diagrams for code snipets  
Draw diagrams for how the flow of information works + hardware  

#### CPU Architecture - Lecture 2
Parts of the CPU:  
Control Unit - mastermind, shuffles things around from memory  
ALU - Arith Logic Unit  
Registers - Stores data for calculations  
Instruction Pointer - pointer to an action to execute (can be different than what is running in instruction register  
Instruction Register - current instruction being executed - taken from the pointer  
State machine of the Control Unit: 1. Fetch, 2. Execute, 3. Increment  

Modified: Von Neumann Architecture: CPU, RAM, and peripherals talking through a hardware bus - uses interrupts  
Often many busses

Illustrate how instructions in memory are executed in a quick sketch

#### ISA and Assembly - Lecture 3
Instruction Set Architecture  
RISC vs CISC  
RISC - Reduced Instruction Set Computer  
CISC - Complex Instruction Set Computer - x86 for example  
Modern machine have RISC hardware but CISC ISA  
Instruction set has been decoupled from the hardware. Allows hardware to be optimized while letting programmers use more complex instructions  
Decoder converts between RISC and CISC instructions  
Decoder is typically a dedicated chip on the CPU that does the conversion  
Convert for basic assembly to code or code to simple assembly (for loop, etc) - godbolt

#### ABI - Lecture 4
Application Binary Interface  
Tells us the conventions for functions calls  
Parameters are stored in registers specified by the ABI (rsi, rdi, etc)  
Int return values are placed in rax and rdx, floats in xmm0;  
Preserved registers - ABI guarantees these registers will not be modified by the instruction  
Temporary registers - Data can get clobbered/changed by the function

#### System Calls - Lecture 5
Require running kernel code with the cpu in kernel mode (Ring 0) - give access to cr3 - page table register pointer  
Also other protected registers. Keeps processes from messing with each other   
We use the syscall instruction to “trap” - a CPU exception that jumps to the kernel’s exception handler table  
When returning, we switch the CPU back to user mode (Ring 3)  
Regular functions uses a call instruction  
Syscalls are numbered (unique numbers), and the number must be stored in rax register  
Regular functions calls are made in user mode, syscalls result into traps into kernel mode  
There is a table of interrupt numbers, can be triggered with a int hex code command to execute syscall code based on the interrupt table 

#### Processes - Lecture 6 and 7
Draw PCB - code block, stack, heap  
First process is a systemd (consisting of init) created by the OS on boot  
Each process has a PCB that keeps track of info for us:  
Stack (grows down), heap (grows up), code (typically fixed), and free space for stack and heap to grow  
Pseudo code to make a PCB:  
Struct to hold all the registers the process is running for context switching

Create new processes with fork(); - clones the process into a new process  
Context switching is where the CPU shifts between executing programs. Registers are stored for the current running process (current state), load the state of the new process (from its own PCB) into physical memory and execute. Repeat for every switch. 

We can overwrite a process to run a new program with exec(); replaces the code for the running process with another set of code. This literally changes the program. 

Create a new process - fork() to clone the running process, exec() - replaces the code of the new process  
Typically how UNIX does this. Takes the init background process, forks, execs and that is the new process running  
When we compile a program and run its executable it creates a process. UNIX literally forks the running process and execs in the code we compiled into the new running process  

Each process has its own virtual address space.

#### Process Lifecycle - Lecture 6 - 9
Created, running, blocked, runnable, zombie (finished running but still isn’t cleaned up and PCB removed)  
Context switches  
Orphans are created from killing a parent function, can be used to create a daemon (processes that run in the background) - fork twice and kill the child.  The grandchild process becomes a daemon

#### Scheduling - Lectures 10 and 11
Schedulers pick which processes run next  
Preemptive and non-preemptive scheduling  
Non-preemptive: FIFO (each jobs runs for the same amount of time), SJF (all jobs arrive at roughly same time)  
Preemptive: STCF (predicated on Turnaround Time rather than Response Time), Round Robin (RR)  
There are advantages/disadvantages to all of these.  
Multilevel Feedback Queues (MLFQ) - uses a round robin with multiple levels of priority 

- keeps track of total time in queue rather than the time slice - gaming can send an interrupt to stay in a high queue, this gets around that hack of controlling the CPU
- as a process on a higher level is aged it gets lowered in priority
- if a process has been “starving” it gets boosted to a higher priority 

#### IPC - Lectures 11 and 12
Interprocess communication (IPC)  
Pipes both in C and command  
FIFOs vs pipes  
Connect via code to reader/writer pair between processes

#### Virtual Memory - Lectures 13 and 14
All processes deal with virtual addresses. 
Hardware/OS team up to automatically translate these to physical addresses as a program runs. 
Dynamically translating relocating addresses uses base and bound, or segmentation  
Base and bound has a starting location that translates to a physical address. If a process exceeds its bound it is a memory violation. 

Segmentation does base and bound but in smaller chunks instead of the entire PCB

#### Paging  - Lecture 15
Memory split into fixed sized “pages”  
Virtual address = virtual page number (VPN) + offset  
Physical address = physical page number (PPN) + offset  
PT[VPN] = PPN. Add offset to get physical address  
Offset doesn’t change during translation  

#### TLB - Lecture 15
The TLB is a cache of recently used translations  
Walking page tables requires extra memory accesses, so is slow.  
The TLB makes paging practical  
Given the speed of cache access vs memory access, you should be able to say how much time memory accesses (in some given assembly instructions) take with and without TLB.  