## Week 1 - Day 2
### Relational Models and Keys
**Schema**: A set of attributes - Specifies the structure/rules of a table - The column headings for a table  
**Attribute**: A name and a type (column heading)  
**Instance**: The values in a table - A set of tuples  
**Tuple**: One row  
**Relation**: a.k.a “table” - A schema + instance - A set of tuples - By definition, each tuple is unique  
There can be the same Data stored in individual cells in a tuple, but the tuple as a whole is unique  
**Superkey**: A set of fields is a superkey if no two rows have the same values in those fields - can be multiple columns of a table together  
**Keys**: uniquely identify each tuple - Critical for the DBMS’ underlying operations - ex: an ID number - It is a superkey - no proper subset of its fields is a superkey  
**Subset**: Does not include the entire set. [A,B,C] is not a subset - [A], [B], [C], [A,B], [A,C], [A,B] is

In a database data does not come first and keys define what data is valid  
Design comes first. You dictate what columns are “keys”  
To design a database you take a problem description and come up with a schema  

Example: a table with a student ID (sID), course ID (cID), and grade.  
If sID is the key, students cannot take more than one course  
If sID and cID are the key, they cannot take the same course more than once.  
If all three are the key, then any duplicate attempts that give the same grade are discarded.

DBA specifies one key as the primary key  
If a schema has more than one key, they are called “candidate keys”  
We can specify additional uniqueness constraints (candidate keys)
Ex:  
`PK(sID, cID)`  
`UQ(sID, Grade)`  
"A student cannot take a course twice, and cannot receive the same grade twice”

1. “A student cannot take the same course twice, and no two students can receive the same grade in the same course”  
`PK(sID, cID) UQ(cID, Grade)`
2. “A student can retake a course for a different grade and both will show up in the record”  
`PK(sID, cID, Grade)`
3. “A student can only take one course and receive a single grade for that course, and courses have maximum enrollment of one”  
`PK(sID) UQ(cID)`

Keys relate two separate tables together. This allows you to find related tuples between tables.

**Foreign Key**: Attribute in one table that uniquely identifies a row in another table 