## Week 10 - Day 1
### Indexes
Every table has an index on its primary key, called the primary index  
There can also be secondary indexes used to speed up common queries  
You can choose which other column(s) have an index

```
CREATE INDEX <idx name>
ON <table>(<column list>);
```

### Exercise
Common queries:  
Find book by Author - Index by author  
Find book by Title and Author - Index by (Author, Title)  
(Title, Author) does not work. Indexing sorts by first column then searches second. This would sort by title first, this makes searching for author much slower

(Author, Title) allows for both common queries to be fast

### Multicolumn Indexes
`CREATE INDEX (Dept, Num)`  
Index key is both values appended  
`select Name from Classes where Dept = “CS”`  
Works great – just look at first two characters of the key  
`select Name from Classes where Num = 1030`  
Bad Index! – Not sorted primarily by num

A multi-column index on (A, B) is like sorting with a tie-breaker  
Primarily sorted by A  
Each group of A secondarily sorted by B

Get book by Title OR Author  
Two single-column indexes: Index(Author) and Index(Title)

```
index(A, B) != index(B, A)
index(A, B) != index(A); index(B);

INDEX(A, B)
```

Good for searching by both columns with AND Good for searching by A  
Bad for searching by B

```
INDEX(A) 
INDEX(B)
```

Best option for searching by both columns with OR
Bad for searching by both columns with AND Good for searching by A, Good for searching by B

### Indexes in SQL
Create an index  

```
CREATE INDEX <idx name>
ON <table>(<column list>);
```

Delete an index  
 `DROP INDEX <idx name> ON <table>;`

View indexes  
`SHOW INDEX FROM <table>`

### Storing Data in DBMS
DRAM?  
Too small, Volatile (power outage = data loss), DRAM acts as a cache in DBMS  
Disk?  
Big, Non-volatile, Too slow? – No choice; must try to limit delays

**Disk access**  
Mechanical motion is extremely slow.  
Computers are used to “speed of light”   
Avg. seek time: 4ms.  
Avg. rotation time: ~4ms @ 7200 rpm  

**Read Amortization**  
Anticipate that nearby data will also be needed soon  
OS reads 8 sectors (1 “page”) at a time into memory (Or more)  
In summary:  
Database size >> memory size – have to use disk  
Going to disk is very slow. 
Reading data within same page – probably already in memory  
Make your program (DBMS) aware of this – minimize # of disk accesses

### Index Goals
Fast insertion, Fast deletion, Fast search  
Range query  
Unique match (==)  
*Touch as few pages as possible during operations*

### Binary Search Tree
Good starting point  
Then look at optimizing for page accesses  
Indexes must reside on disk - Potentially occupying many pages

Where/how to allocate tree nodes?  
Option 1 - Add new nodes to end of current page, Get new page whenever full  
As tree gets large, pages accessed ≈ height + 1

Option 2 - Quad Search Tree  
Still 3 keys per page - One node fills entire page  
Each traversal step requires more computation, But all within one page

Option 3 - Balanced Tree (B Tree)  
Generalization of a balanced search tree  
Branching factor undefined, but usually wide  
Perfect balance is maintained without the need for tree rotations

### Balanced + Tree (B+ Tree)  
Many DBMS use them  
Minor differences with B Trees  
Each node is an array 
Internal nodes have pointers between keys  
Keys repeated in corresponding child, Internal nodes hold only keys, Leaves hold <key: value>  
Internal nodes sorted for fast search  
Leaf nodes not (necessarily) sorted. 
Every leaf at same depth  
Always perfectly balanced. 
Leaves form a semi-ordered linked list. 

If b branches per node and N total nodes  
Pages visited per lookup: (logb N)  
Performance measured in I/O operations, not in compute  
B+ tree pages visited: (logb N)  
Binary search: (log2 N)  
BST: (???) -Depends on insertion order

Typical example:  
100,000,000 total records  
b = 100 (100 keys per 4096B page)  
N = 1,000,000 nodes  
B+ tree: 3 pages  
Binary search: ~20 pages  
BST: ~10 – 20 pages

Every table has a primary index (“clustered index”)  
Leaf nodes contain actual table rows (page-sized chunks)

How should we pick which one is the primary/clustered index?  
It’s always the primary key  
The decision was already made based on relational model
