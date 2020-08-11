## Week 9 - Day 1
### ACID
Race Conditions can still occur with databases  
Read-Modify-Write is very common  
Two threads read old value and one of the “modify” steps is lost  
Critical section - any portion of an algorithm in which a race condition exists  
For guaranteed correctness, no more than one thread must be in a critical section at a time

Various ways to handle critical sections:

* Re-write algorithm to eliminate them
* Protect them with a “lock” (mutual exclusion)
* Transactional operations
* Other forms of synchronization

“Joe” and “Dan” try to check out the same book at the same time = Race condition  
One will succeed, the other will violate PK constraint, DBMS  automatically protects against this race

Transactions  
By default, operations in MySQL are a transaction  
Has two possible outcomes: 0% completion or100% completion  
Will not interfere with other transactions

Atomicity - Operations completely succeed, or completely fail  
Consistency - Operations never result in violated constraints  
Isolation -Transactions don’t interfere with each other, even if concurrent!  
Durability - Result of completed operations will not be lost

A good DBMS provides “ACID” ... like MySQL and MariaDB

### Examples
Transfer money from account X to account Y

```
update Acts balance=balance-100 where actNum = X;

update Acts balance=balance+100 where actNum = Y;
```

Both of these are independent operations. Both either fail or succeed

What if the server crashes between these, and account Y never gets that’s $100, the customer is out that money. This is bad obviously   
For this to work we need to see both of the transactions as a single operation that can succeed or fail. 

We can manually increase the scope of a transaction

```
START TRANSACTION; 
<SQL commands> 
COMMIT;
```

This creates a single transaction with many SQL commands. All have to execute or else it breaks, changes are not made permanent in the database

We can actually rollback commands as well

```
START TRANSACTION; 
<SQL commands> 
ROLLBACK;
```

Functionally this is more like the commands not being committed in the first place  
Manually rolling back is not usually useful  
The system automatically rolls back when:

* The transaction causes an error
* Connection interrupted
* Server restarts

### Multiple Sessions
During transaction, changes are only visible within the transaction session (by default)  
After the commit of the commands the transaction is available globally for all sessions

### Multi-step Operations
Suppose we need to leave at least one copy for the library

```
SELECT COUNT(*) FROM … // One operation
if(count > 1) ...
INSERT INTO CheckedOut … // One operation
START TRANSACTION
SELECT COUNT(*) FROM ...
if(count > 1) ...
INSERT INTO CheckedOut ...
COMMIT
```

However **Transaction != Mutex**  
Mutex = lock, unlock - 1 thread/session  
Transaction = start, commit - multiple threads/session

Multiple clients can happen at once and override the count variable
Transaction -> Commit - Only guarantees that both operations fully fail or fully succeed  
A transaction does not provide mutual exclusion  
Its job is to ensure consistency (no partial operations)  
Only synchronizes when it needs to

DBMS typically handles this for you

Distributed web servers. Constraints have to be on the database. The web servers controlling this can lead to conflicts if they manage it, as multiple servers can happen at once. 

### Isolation
Operations must not interfere with each other Transactions must provide some form of synchronization to detect conflicting writes
We can specify increased isolation

```
SET [GLOBAL|SESSION] TRANSACTION
ISOLATION LEVEL [level]
```

select @@TX_ISOLATION will return current isolation level.

### Isolation Levels
SQL provides four isolation levels  
READ UNCOMMITTED - Less Isolated Faster  
READ COMMITTED  
REPEATABLE READ (MySQL default)  
SERIALIZABLE - More Isolated Slower

Stronger isolation requires more synchronization

Read Uncommitted  
Other sessions can read uncommitted data i.e., nonexistent data, The value seen by session 2 can be rolled back/fail/etc. 

Read Committed  
Writes not visible to other transactions until committed

Repeatable Read  
Each transaction has its own “snapshot” of the DB
Typically the default level for DBMS

Serializable  
Complete isolation between transactions  
A SELECT in one transaction can stall an INSERT in another And vice versa  
Session 2 will not SELECT until the INSERT is committed  
This can also happen the other way around. Session 2 can select and session 1’s insert happening just after will hang until Session 2 has commited  
SERIALIZABLE starts to look like a mutex - But still critical difference: reads can proceed in parallel  
Not surprisingly, everything slows down as more isolation is required