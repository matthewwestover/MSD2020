## Week 9 - Day 2
### Locks
Transactions are *not* intended to be used as locks although they can solve some of the same problems

SQL provides various locking mechanisms  
Table locks – lock an entire table (like a mutex)  
Select locks – lock only the row(s) selected, Tied to a transaction

Table Lock

```
LOCK TABLE t <READ | WRITE>;
UNLOCK TABLES;
```

Inside the lock is not automatically a transaction  
Very rare in practice  
Unrelated rows locked

Lock Table - Read  
Any session can read  
Other sessions stall on writes  
Locking session fails on writes

Lock Table - Write  
Locking session can read and write  
Other sessions stall when reading/writing  
Other sessions stall when trying to lock 

Solves the Library Problem

```
LOCK TABLE CheckedOut WRITE; 
SELECT ... as copies; 
if(copies > 1)
    INSERT INTO CheckedOut …;
UNLOCK TABLES;
```
But it would be much slower - Any other query requiring CheckedOut would stall

### Select Lock

```
START TRANSACTION;
SELECT ... FOR UPDATE;
COMMIT/ROLLBACK;
```

Useful in the context of a transaction scope of lock is remainder of transaction

`SELECT * FROM T WHERE Col=2 FOR UPDATE;`

Only selected rows locked  
Stalls until other writing transactions finish

Other sessions: Can do non-locking reads, Stall on locking reads, Stall on writes  
Until locking transaction is rolled back or committed

Select Lock vs Serializable  
Achieve same outcome  
Advantages: Don’t have to change isolation level, Fine control over which SELECTs are locked  
Disadvantages: Not directly supported by LINQ

### Indexes
How does a DBMS efficiently structure data?  
More like the index at the back of a book - “Information about the information”  
Biggest difference between DBMS and “regular” program:  Giant dataset, Access order harder to predict

Data Storage in DBMS  
DRAM? Too small, Volatile (power outage = data loss)  
DRAM acts as a cache in DBMS  
Disk? Big, Non-volatile, Too slow? No choice, must try to limit delays

How to fix disk read latency?  
Read consecutive data  
Pay for seek and rotation delay only once, then read subsequent sectors  
Requires consciously organizing data  
OS does as much as it can for you  
Don’t randomly jump around through numerous files

### Read Amortization
If you can’t eliminate latency, amortize it  
Anticipate that nearby data will also be needed soon. 
OS reads 8 sectors (1 page) at a time into memory, Or more...  
Program must take advantage of this – page awareness

In summary: Database size >> memory size – have to use disk. 
Going to disk is very slow  
Reading data within same page – probably already in memory. 
Make your program (DBMS) aware of this – minimize # of disk accesses

Normal program: Keep data in cache – avoid main memory at all costs  
DBMS: Keep data in main memory – avoid disk at all costs

### Data
Records are physically stored in some order  
Good for some queries, bad for others  
Records span many pages (N)  
Searching by unordered column: N/2 pages on avg  

Store multiple copies of the data, sorted on different criteria? No – dealing with massive data as is. 
Sort records on desired criteria before search? Sort operation touches every page.  
Works great if you only search on one criteria. Works horribly if you search on anything else

### Index
Auxiliary data structure pointing to main records  
Hold only index value + pointer  
Ordered by index value.  
Think of them as a table with pointers to the main table  
Index table itself must reside on disk somewhere  
Algorithmically fewer accesses to find entry +1 to follow pointer to main table  
Without index: O(N)  
With index: O(log N) + 1

Index tables are also a lot smaller than main table - They hold only keys

Primary Index  
Every table has an index on its primary key, called the primary index

Secondary Index  
You can choose which other column(s) have an index  
You can choose the type of index  
`CREATE INDEX <idx name> ON <table>(<column list>);`