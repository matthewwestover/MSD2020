## Week 14 - Day 3
### Filesystems
Persistence: Data persists beyond jobs, crash

* Disk provides basic non-volatile storage
* OS can enhance persistence via redundancy

Speed: Fast access to data

* Random access handled efficiently
* OS can enhance performance via file caching

Size: Can store lots of data • Sharing/protection:

* Users can control who/what has access to their data

Ease of use: Basic file abstraction (names, offsets, byte streams, …)

* Directories simplify naming and lookup

File: Container of persistent data  
Unix: flat, variable-length byte sequence  
Directory system: Hierarchical naming relationships  
Directories are special “files” that index other files  
OS exports operations to manage directories indirectly  
Common file access patterns:

* Sequential: data processed in order, byte/record at a time
    * Example: Compiler reading a source file
* Random access: address blocks of data based on file offset
    * Example: Demand paging reads, database searches

#### Inode Number
Each file has exactly one inode number  
Inodes are unique (at a given time) within file system  
Different file system may use the same number, numbers may be recycled after deletes  
See inodes via “ls –i”; see them increment

#### Paths
String names are friendlier than number names  
File system still interacts with inode numbers  
Store path-to-inode map in predetermined “root” file (typically inode 2)  
We call it a directory

Each process has a working directory  
Stored in its PCB, effectively hangs onto a dir inode  
First character determines if traversal starts from working dir or root. 
Absolute paths start with ‘/’, else relative

#### File Descriptor
Idea:

* Do expensive traversal once (open file)
* store inode in descriptor object (kept in memory)
* Do reads/writes via descriptor, which tracks offset

Each process:

* File-descriptor table contains pointers to open file descriptors
* Integers used for file I/O are indexes into this table stdin: 0, stdout: 1, stderr: 2

#### Deleting Files
There is no system call for deleting files!  
Inode (and associated file) is garbage collected  
when there are no references (from paths or fds)  
Paths are deleted when: unlink() is called  
FDs are deleted when: close() or process quits

