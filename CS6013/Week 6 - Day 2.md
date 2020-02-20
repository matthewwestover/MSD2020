## Week 6 - Day 2
### Address Translation
Static relocation - rewrite code and addresses before running - historic not used in practice  
Dynamic relocation - multiple methods  
Base & Bound - add a base register to all accesses, trap on virtual addresses beyond the bound. If it is beyond bound it is an access violation   
Segmentation - instead of blocking entire process in a single continuous set, take the different segments of the process (code, heap, stack) and treat those as individuals in a base & bound set up. Literally discontinuous in physical memory.

External Fragmentation - free memory that can’t be usefully allocated  
Basically free memory that is too small or scattered to be used by the CPU. It is a memory hole  
There is internal fragmentation - the allocated memory doesn’t match the amount the process needs.  
External Frag is visible to the allocator (OS)  
Internal Frag is visible to the requester  

#### Paging
This works like segmentation but works to fix the external fragmentation issue. 
Instead of thinking of a process as a code block, heap, stack - the whole process is divided into “pages”  
These are in chunks of 2^n (powers of 2) - usually hardware and os specified  
So virtual process memory is completely contagious, physically it split in different locations. Heap can be 3 pages, code block 2 pages. Can be the same size as a segment would be, could be smaller.  
The physical page in memory is a page frame.  
These pages can be arbitrarily intermingled in physical memory. Something doesn’t live in any specific location to point to in physical memory. They are scattered

##### Translation of Page Addresses
High order bits of address designate the page number  
Lower order bits address designate offset within page 
Ex:

```
20 bits: virtual page num (VPN) + 12 bits (page offset) in virtual memory
Translates from page table
Physical Page/Frame Number (PPN - translated from VPN) + page offset in physical memory
```
This limits the number of pages (2^20 total pages) based on physical memory this sets how big a page can be

The bit total in the VPN do not need to match the bit total in the PPN  
Ex (VPN can be 2 bits and PPN can be 4 bits) the Table does the translating so it doesn’t matter if there is a size different  
If VPN is smaller than PPN there is free space  
If VPN is bigger than PPN that means that virtual addresses may have to share physical space.  
This lets data get shared. Two different VPNs point to the same data in Physical  
If they aren’t sharing data that spot physical memory has to be scheduled for who gets access when

Page Table needs to keep track of many more “bases” as the starting point in physical memory than Segmentation had  
Where are page tables stored?  
Assuming 32-bit virtual addresses, 4 kb size pages, and 4 byte page table entries = 4 MB in physical memory just to keep track of the table for mapping

Hardware finds page table base with register (cr3 on x86 cpu)  
Each process has its own page table in memory  
When context-switching the cr3 register has to move its pointer to where in the pcb it needs for the new running process

There is other info in page tables  
Protection bits - read write execute to the page  
Present bit - trap if access a VPN that isn’t present  
Accessed bit - has this page been used  
Dirty bit - gets set when a page modified  
All page table entries are just bits  storied in memory. They are interpreted by the hardware/OS

#### Accesses with Pages
Mov rax, rdi[rsi] - this is all virtual memory addresses. Take data that is rsi away from rdi (virtual address) and place it in rax(physical register)  
Rdi[rsi] needs to get translated into physical memory.  
The tops bits at rdi+rsi are the VPN. If the page table is an array look up pagetable[VPN]  
This gives the physical address of the page in memory  
The PT[VPN] has all the bits for the page table entry (protection, present, accessed, dirty bits, etc).  
Check that this process can access this address from the table - get the PTE  
The final physical address is the PPN of the PTE + the lower bits of the Virtual address (rdi+rsi)  
This is SLOW - Two memory access for every memory access

CPU caches the recently accessed PTEs.  
This is done in the Translation Lookaside Buffer - TLB. 
On access if the PTE is in the TLB skip the page table - this is super fast  
If the PTE isn’t in the TLB - get from physical memory and add it to the TLB cache