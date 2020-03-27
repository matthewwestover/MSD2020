## Week 9 - Day 1
### Threading
Process can be seen as a program running through memory tracing a path 
It has an address space - virtual memory restricted to the process  
It only operates on this data - set as the “state”  
Processes are isolated in this manner by the OS  
Threads only live within a process’s address space  
Possible to have multiple threads in the same thread  
While a process thinks it has access to all memory - threads only have access to memory inside the process  
This means that each thread shares the code/data/files within a process  
Modern take is threads are the more fundamental unit of execution

### Thread Control Block - TCB
Just like the PCB we have a TCB for each thread  
There are as many TCBs as there are threads in the process  
Each TCB only holds information on the state of its thread  
These usually sit inside the PCB itself  
This has to hold the registers for each thread so it can be moved around and executed  
Typically seen as a block of Pointer to next TCB, Program Counter, Registers

Context switching between thread TCBs is much faster than process switching  
Avoid multiple syscalls in this process as well. There are no syscalls for thread switching  
Because threads share process address block, threads can still override each other because they don’t have that same restrictions within it.  
More responsibility on the programmer to ensure threads remain detached.  
Trade off between speed, multitasking and not having to worry about sections of code overriding each other  
Effectively each thread is represented as a stack within the PCB

### Thread Use Cases
Word processor or video game: one thread waiting on input, the other renders things  
Web server: a thread to listen for client requisitions, another for service requests  
Threads allow data parallelism  
Operate on pieces of data in parallel if operations are independent  
Classic example: ```c[I] = a[I]+b[I], I=0…N```  
Threads simultaneously handle the different ranges of indices

### Example
Create multiple threads saying **“Hello from thread: #”**  
Output in thread can look like 

```
Hello from thread: 0

Hello from thread: 
Hello from thread: 3
Hello from thread: 2
1
Hello from thread: 4
```

We can’t predict the thread controller for when they swap between them in side the process. 