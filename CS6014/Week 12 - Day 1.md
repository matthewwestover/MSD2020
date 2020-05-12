## Week 12 - Day 1
### Meltdown and Spectre
These attacks use the intersection of lots of stuff we’ve covered:

* page tables/virtual memory
* caches (side channel attacks)
* the kernel’s interrupt handlers
* CPU architecture (which we’ve mostly skipped) concurrency!

#### Out of Order Execution
CPUs have lots of resources (registers, functional units, etc)   
Waiting is bad - “waste of time”  
Lots of CPU design work went into making a seemingly serial instruction stream execute (partly) in parallel!  
A big one is speculative execution  
Meltdown exploits speculative execution around memory accesses  
Spectre exploits speculation around branch prediction

#### Speculative Execution
The CPU starts executions even before it’s sure they need to be executed, as long as they have all the inputs they need  
If it turns out they should not have been executed, none of their results are written to registers/memory, etc. 
Basically it’s as if they were never executed   
**Except** they modify hidden CPU state. They might read memory into cache

Example branch prediction: CPU doesnt want to wait to see a true/false result so it starts executing the code it thinks is correct. If it is wrong it drops it work and starts the correct branch. Can have a high 90s% accuracy prediction rate. 

#### Cache Timing Side Channel Attack
Basically Meltdown/Spectre trick the CPU speculatively executing memory read instructions  
Based on which memory bits were brought into cache, these attacks can infer the value of memory the process wasn’t supposed to read! (Was this a fast or slow access)

#### Kernel Memory in User Processes
To make system calls efficient, all of the kernel’s memory was mapped into the user’s address space  
The memory manager has a bit that marks which pages can be read in user-mode or not  
Under normal operation, if a user processes tries to access kernel memory in its process, an exception occurs  
The OS’s map most or all pages of physical memory into their address space, so the pages of ALL processes would be compromised if a user process were able to read this kernel memory  
Meltdown circumvents this access control by exploiting speculative execution!

#### Meltdown
Cause a segfault but allow us to see what value was going to be read but not
256 page long “probe array”   

```
byte stolen = *kernelMemory //will segfault, but not until after we try other stuff 
probeIndex = stolen*sizeOfPage
probeArray[probeIndex] = whatever
```

The page gets loaded into the cache, the stolen byte is loaded into the page, which still lives in the cache after the segfault  
We can get the value by looping through the page array. The one that loads the fastest is the one in the cache, which is the one we stole from the kernel. 

Basically we dont want to wait to see if a memory segfaults, and the speculative execution leaves a trace as it updates the cache. This allows meltdown to work. 

##### Mitigation
“Kernel Page Table Isolation”  
Basically, DON’T map the kernel into user process address spaces  
Some bits of the kernel still have to be there (the exception table and a few other small bits), but most of the kernel is removed  
This causes a performance hit since we’ll have to load up the kernel’s page tables to perform system calls. 
Modern CPUs have extra bits in TLB entries to store something like a PID. This means that we don’t have to flush the TLB on context switches. This significantly reduces the performance hit of KPTI

#### Spectre
Different way of abusing speculative execution to leak information  
Relies on speculative execution around branches  
A bit more complicated than meltdown, and harder to fix. 
Seems limited to leaking info in a single processes’s address space. 
Still bad (javascript code leaking memory from the containing Chrome process, for example). 
Many variants, still being investigated + discovered