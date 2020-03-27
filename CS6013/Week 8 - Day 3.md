## Week 8 - Day 3
### Virtual Memory Wrap-up
Multi-level paging = defines a page as just a chunk of memory. Can contain more pointers to other pages, or the page of memory itself.  
This means we need a way to swap pages from disk and back to disk  
We need to have a policy in place about what pages are removed.  
This uses an access bit + dirty bit to dictate if a removed page gets removed or removed and updated in disk  
Thrashing is page faulting and swapping all the work, not doing as much processing as possible. 

### Thrashing 
Working set: collection fo memory currently being used by a process  
If all working sets do not fit in memory -> thrashing  
One “hot” page replace another. Percentages of accesses that generate pages faults skyrocket

The typical solution is to “swap out” entire processes  
Scheduler gets involved, and needs to be fair. This is triggered when a page fault rate exceeds a bound limit  
When all the swap devices are full, Linux invokes the “OOM Killer” - Out of Memory

UNIX typically uses “Global Replacement” 
All pages are competing against each other for all process. There is a single shared pool  
**Advantage**: very flexible -> can globally “optimize” memory usage  
**Disadvantages**: thrashing more likely, can often do the wrong thing (replace the pages of a process about to be scheduled)

Windows used to “Per-process replacement”  
Each process has a private pool of pages, competes with itself  
Not fighting other processes for physical memory  
Alleviates interprocess problems, but never process equal  
Need to know working set size for each process  
Windows has calls to set process’s min/max working set sizes  
Memory might not be used as efficiently as processes don’t know about each other. 

### Copy on Write - COW
Say OS needs to copy a page from process 1 to process 2  
Process 2 wants read-only access, don’t copy. Just provide access, mark as COW  
If write access is needed:  
Trap into OS - OS notices COW marking  
Allocates a new page in address space of Process 2  
Copies data from the Cow page to the new page  
Process 2 now has its own copy

Copying pages is slow, could be going to physical memory or disk, extra memory used. So avoid copy unless you need it

Can use COW to great effect with fork()  
Entire parent address space can be marked COW  
Parent and child now share the address space, until the child needs to modify  
Intuition: most of the address space rarely changes across parent child

### Demand Zeroing
Page frames cannot be reused directly - may change sensitive data  
OS zeros pages before re-mapping them  
Can be lazy - only zero a page frame when process accesses the memory. 

### Mmap()
System call to manipulate address space  
Map a file for demand paging  
Can treat file as a big byte array  
Other processes can map to same file to share state  
Map anonymous pages to add heap space - can map regions larger than memory  
Modern ```malloc()``` uses this -> C library wrapper around mmap() which is a syscall.   
Map pages that can be shared with children   
On fork(), mappings copies without COW protection

```void* mmap(void *addr, size_t len, int prot, int flags, int fd, off_t offset);```

addr is the pointer you’d like to get back. Usually we don’t care, so pass null meaning “OS can pick”  
len is the number of bytes you want to allocate (beware: it will rounded up to a multiple of the page size!)  
prot describes protections on the chunk of memory, a combination of PROT_READ, PROT_WRITE, and PROT_EXECUTE  
flags describe properties of the memory (backed by a file, shared with your child on fork(), etc)  
fd is the file descriptor for the file that we’re using to back this memory  
If we want to map the middle of the file we can specify a nonzero offset

Example mmap flags:  
MAP_ANON allows for “anonymous backings”, no making file needed. - malloc replacement  
MAP_SHARED means that this si preserved after fork. Child and parent process use the same page in memory - uses COW framework

Munmap(); systemically for clean up in a mmap() functions  
Together are the equiv of malloc() and free(). Slower but can be dropped in used. 

malloc() is not a syscall! Free() is not a syscall! 
Use mmap() and munmap()  
Get memory using either the sbrk() syscall which grows  
the heap section (less common now) or by getting chunks of memory from mmap (more common)  
Once the allocator has a big chunk of memory, it carves it up and gives out chunks when users call malloc()