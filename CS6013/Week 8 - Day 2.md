## Week 8 - Day 2
### Virtual Memory
Support processes when not enough physical memory  
Single processes with very large address space, multiple processes with combined address spaces

User code should be independent of amount of physical memory - always should return correctly even if not with great performance  
Users should assume “hey it will run”

Css provides illusion of more physical memory

### Multilevel Paging
Allows for massive address spaces  
Can far exceed size of physical memory  
Need to keep pages somewhere else, copy into physical memory as needed   
Pages cannot all fit in physical memory at once. Page to page to page, they get stored in disk with a memory hierarchy

### Memory Hierarchy
Registers: really small but very fast - 1 cycle  
Caches: larger but slower - handful of cycles  
Main memory: even larger but slower - hundreds of cycles (RAM)  
Disk storage: largest and slowest of memory - SSD faster than HDD but still slower than other forms of memory. Orders of magnitude slower

### Swapping
Idea: OS keeps unreferenced pages on disk  
Process can run when necessary pages are loaded into main memory  
Swap things in main memory to disk and from disk to main memory  
OS and hardware cooperate to provide illusion of large disk space as fast as main memory  
Same behavior as if all of address space in main memory  
Hopefully have similar performance  
OS must have mechanism to identify location of each page in address space→in memory or on disk  
OS must have policy for determining which pages live in memory and which on disk

### Swapping Mechanism
Each page in virtual address space maps either: 

* Physical main memory: small, fast, expensive
* Disk: large, slow, cheap
* Nothing (error): free

Use a “present” bit in page tables

* Permissions (r/w), valid, present
* Page is in memory: present bit set in PTE
* Page is on disk: present bit is cleared out

If a page is not present on main memory, this causes a page fault  
This traps the OS  
OS finds the actual disk location for the page in the page table  
Instead of aborting the process, it fetches the page, changes the PTE, and restarts that instruction

Ex:  
PPN is 23 and present bit is 0  
OS knows that the memory is located in disk on PPN 23. The OS traps, looks at disk at PPN 23, and moves it to main memory  
This location in RAM has its OWN PPN. This item gets updated to show this. PPN becomes 16, and the present bit is set to 1  
Present bit is usually only seen in kernel mode. To see in user mode make a syscall and request that specific bit back

Demand Paging: only swap from disk to memory when a page fault occurs. Pay cost of page fault for every newly accessed page.  
Anticipatory/prefetching: load page before referenced. OS predicts future accesses and brings them to memory early. 

Main Memory can get full. Some pages need to be evicted after use  
Evicted victims have to be picked to send back  
How to handled eviction changes based on the “Dirty” bit of the page  
If dirty bit is set, that information in memory has changed, it needs to get copied to disk.  
If dirty bit isn’t set, that information is same as disk, can just be dropped and pulled from disk at a later time if needed. 

### Picking a Evict Victim
OPT (Theoretical Optimal): replace page not used for longest time in the future. Guaranteed to minimize number of page faults. OS has to predict the future which isn’t possible to do completely  
**FIFO**: replace page that has been in memory the longest. This is fair, all pages get equal residency. It is easy with a queue. Some pages may need to be always needed  
**LRU**: least recently used. Replace page not used for longest time in the past. With locality, this can predict OPT. Must track time of each page access.

Add more memory, performance changes  
**LRU**, OPT: guaranteed to have fewer (or same number of) page faults. Smaller memory sizes are guaranteed to contain a subset of larger memory sizes. Stack property: smaller cache always subset of bigger  
**FIFO**: add memory, usually fewer page faults. Belady’s anomaly: May actually have more page faults! 

LRU is what is used in practice

### LRU Issues 
LRU does not consider frequency of accesses  
Page accessed once in the past equal to one accessed N times?  
Solution: Track frequency of accesses to page  
Pure LFU (Least-frequently-used) replacement  
Problem: LFU can never forget pages from the far past 

### Clock Algorithm
Hardware:   
Keep accessed bit for each page frame  
When a page is referenced, set accessed bit. 
OS:  
Page replacement: look for page with accessed bit cleared (not referenced for a while)  
Keep pointer to last examined page frame  
Traverse pages in a circular buffer - clear access bits as it traverses  
When a page is found with an accessed bit already turned off, that page can be replaced.  

### Boosting Clock efficiency 
Replace multiple pages at once - find multiple victims each time  
Add a software counter (“chance”) intuition - better ability to differentiate across pages. Increment the software counter if the accessed bit is 0. Replace when chance limit is reached.  
Replace “clean” before “dirty” pages - dirty is more costly to copy back to disk

### Average Memory Access Time
```AMAT = (PHit * THit) + (PMiss * TMiss)```  
What is average access latency?  

* L1 cache: 2 cycles
* L2 cache: 10 cycles
* Main memory: 150 cycles
* Disk: 10 ms → 30,000,000 cycles on 3.0 GHz processor

Assume access have following characteristics:

* 98% handled by L1 cache
* 1% handled by L2 cache
* 0.99% handled by DRAM
* 0.01% cause page fault
* Average access latency: about 3000 cycles / access

Moral: Need LOW fault rates to sustain performance!
Disk memory access is fucking **slow**

### Thrashing
Worst case is always having page faults  
Working set: collection of memory currently being used by a process.  
If all working sets do not fit in memory → thrashing 

* One “hot” page replaces another
* Percentage of accesses that generate page faults skyrockets

Typical solution: “swap out” entire processes

* Scheduler needs to get involved
* Need to be fair
* Invoked when page fault rate exceeds some bound

When swap devices are full, Linux invokes the “OOM killer”