## Week 3 - Day 2
### Process Termination
When a process dies, OS reclaims recourses:

* Record exit status from PCB
* Close files, sockets
* Free memory
* Free nearly all kernel structures

Process terminates with **exit();**  
Process terminates another process with **kill();**

#### Orphans and Zombies
Parent wait() on child returns status  
Must keep around with PCB with status after child exits  
Zombies are exited processes with uncollected status  
If a parent closes before a child, that child is orphaned:

* Init daemon adopts orphans
* Collects and discards status of reparented children
* Could be handed off to a “us-reaper” instead of init
* A useful ways to create and start daemons

#### Daemons
Daemon computer program that runs as a background process  
First process (init) is a daemon that keeps running while computer is on  
Can start daemons by orphaning processes

* Fork() twice (create a child and grandchild)
* Terminate the child
* Grandchild process is adopted by init

Many ways to start daemons besides orphaning grandchildren  
Single init daemon has been mostly replaced by systems, which contains 69 programs

#### Low cost process isolations
**Virtualiztion**

* We want to share the CPU among many jobs
* To make things simpler on the programmer side, we want each job to think it can use the full CPU (virtualization)
* Timeshare the CPU among processes

**Isolation**

* Processes must be isolated from each other
* Kernel must be isolated from processes
* Hardware must be isolated from processes

**Performance**

* Processes should run at full hardware speed
* Cannon have kernel interpose between process & CPU

#### Limited Direct Execution (LDE)
Run code on CPU with constraints  
X86 has privilege/protection rings  
Ring 0 - kernel mode  
Ring 3 - user mode  
Rings 1 and 2 are rarely used  
Switching from user to kernel mode can cost 1000-1500 cycles even for getpid()  
Hypervisor (ring -1) allows gets operating system to run ring 0 operations. This is hardware assisted virtualization  
CPU is in one ring or another. Cannot be in both ring 0 and ring 3 at the same time. This is a hardware limited thing.

Challenges with the LDE
Must restrict access to sensitive state
Process could access kernel, other process, devices, memory
Process could reconfigure address space
Must prevent denial of service; must share resources
Process could enter an infinite loop
Process could schedule massive I/O
Again must restrict access to state that controls interrupts
Key to this is privileged vs unprivileged mode (ring 0 vs ring 3, kernel vs user)
Specific operations for process (user) code
Privileged code can only execute on well defined boundaries in the kernel

#### Challenges with the LDE
Must restrict access to sensitive state  
Process could access kernel, other process, devices, memory  
Process could reconfigure address space  
Must prevent denial of service; must share resources  
Process could enter an infinite loop  
Process could schedule massive I/O  
Again must restrict access to state that controls interrupts  
Key to this is privileged vs unprivileged mode (ring 0 vs ring 3, kernel vs user). 
Specific operations for process (user) code  
Privileged code can only execute on well defined boundaries in the kernel

##### Problem 1 - Sensitive State:
Hide device control registers from applications.  
This uses virtual addresses, all address translate with a table. (Virtual locations mapped to actual locations in ram, etc)  
Restrict accès to certain CPU state  
Privileged instructions/regs only available in ring 0  
%CR3 controls virtual address translation.  
HLT, LIDT, MOV $CRn, CLI, STI, %CR3, %IDTR all restricted  
Lower privilege level to ring 3 before running user code  
Raise privilege level to ring 0 when kernel is in control  

Computers have a hardware that does the look up from the virtual addresses to the physical addresses in memory