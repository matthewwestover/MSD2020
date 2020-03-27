## Week 8 - Day 1
### Virtualization
There are multiple virtualization methods.  
Base and Bound had internal fragmentation due to wasted “empty” space for heap and stack  
Segmentation has external fragmentation due to variable size of the segments - weird sized holes of memory  
Paging has less external fragmentation but has more complex memory accesses

### Paging Disadvantages
Extra memory reference to look up location in the memory table  
It is inefficient  
Page memory table has to be stored in memory itself  
MMU only stores base address of the page table  
Uses TLBs to avoid extra access (caching of memory locations)  
Storages for page table may be substantial  
Simple page table: requirers PTE for all pages in address space, even if the page isn’t allocated  
Dynamic stack and heap within the address space is difficult to manage

Page tables are more complicated than just a large array  
Any data structure is possible with software managed TLB  
Hardware looks for VPN in TLB on every memory access.  
TLB doesn’t contain the VPN the OS is triggered to find the VPN-PPN translation. Stores this back in the TLB

### Inverted Page Table
Hash table indexed by VPN + shortened PID (ASID)  
Only one table for whole system rather than per process  
Only contains entries equal to allocated page frames (depends on active VPNs)  
Complex data structure - only done under software controlled TLB  
If address not in the TLB, OS handles the trap.  
Looks up the has(VPN+ASID) to find the PPN  
Populates the TLB for VPN, ASID, PPN combo  

For a hardware defined TLB need a well defined simple approach.  
TLB is modernly more often hardware controlled

### Segmented Paging
Not commonly used anymore, but historically important  
Divide address space into segments (code, heap, stack)  
Divide each segment into fixed-sized pages  

Logical address divided into three portions  
Seg# - 4 bits  
VPN - 8 bits  
Page offset - 12 bits  
Implementation - each segment now has a page table. Each tracks based and bounds for that segment

### Advantages of Segments  
Supports sparse addresses spaces  
Decreases size of the page table  
If segment is not used, no need for a page table  

### Advantages of Pages
No external fragmentation  
Segments can grow without reshuffling  
Can run a process when some pages are swapped to disk

### Advantages of both
Increased flexibility of sharing  
Share either single page or entire segment

This still leaves external fragmentation due to variable sized page tables - each segment has a page table. Segments are variable so while pages are fixed sized, the tables for each segment can change

### Multi-level Page Tables
Page the page table.  
page the pages of the page tables  
Goal: let page tables be allocated non-contiguously   
Paging the page tables - creates multiple levels of page tables, outlived is a page “directory”  
Turn a page table into a page itself  
Only allocate page tables for pages in use - prevents unneeded pages in memory.   Easier to share memory resources between processes this way  
Used in x86

32 bit virtual address:  
PD Index - 10 bits  
PT index - 10 bits  
Page offset - 12 bits  

How should this address be structured  
Take a 30 bit address  
The page offset is 12 bits 

**Each page table needs to fit within a page** (4096 bits for example)  
PTE size * number PTE = size  
PTE size can be 4 bytes  
Page size = 2^12 bytes = 4 KB  
2^2 bytes * number PTE = 2^12 bytes  
Number of PTE per page = 2^10  
Number of bits for selecting the inner page = 10 bites  
Outer page size is then 8 bits

The outer page directory is still too big.  
Use a 64 bit virtual address  
Split page directories into pieces as well  
PDP -> PD -> PT -> Offset  
How large the virtual address with 4KB per page, for byte PTE  
4 KB/ 4 bytes -> 1024 entries per level  
1 level: 4 MBs  
2 levels: 4 GBs  
3 levels = 4 TBs  

This only works if you only load the parts that being used. Pages not in use get stored on disk. 

### Summary
Linear page tables require too much contiguous memory - wasted space and irregular sizes  
Many options for efficiently organized page tables  
If the OS traps on a TLB miss, OS can use and data structure - inverted page table  
If the hardware handles the TLB miss - page tables must follow the hardware format  
X86 uses multi-level pages  
Each page table fits within a page   
Page directory indexes of a process page. 

Virtual address needs a physical address to execute

1. Check the TLB (the cache) to see if it is there. If it is then can quickly execute with physical address
2. Not in TLB - look at the outer page table to see if that address maps to something loaded in physical memory or not
3. Once that address is in physical memory it checks the inner table and sees if THAT is in physical memory or not
4. Once that address is in physical memory - does it one last time for the final set of data/instructions the process needs to execute

This requires loading from disk which is very slow.  
Leads to “thrashing” - a process spending more time loading memory from disk than actually processing