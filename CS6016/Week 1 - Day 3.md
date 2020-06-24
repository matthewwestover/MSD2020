## Week 1 - Day 3
### Relational Models
Tables can be semi thought of as “pointers” to each other for relational information.   
Ex. Students table and Enrolled table  
If a student is deleted, what happens to the enrolled table?  
Enrolled now has a “pointer” as a student ID that doesn’t exist. 

#### Referential Integrity
References between values should always be meaningful  
This is an invariant – should hold true at all times  
C++ lets us violate this and Java enforces it by taking away control (with pointers on data)  
Option 1:  Whenever we remove a record from Students, run another command to update all Enrolled with that sID  
Invariant briefly broken  
What if more tables reference Students?  
Option 2: Let the DBMS update Enrolled automatically  
SQL provides some support

#### Foreign Key
Attribute in one table that uniquely identifies a row in another table.  
Think of it as a “pointer” from a child table back to a parent table.  
Not necessarily unique itself  
Multiple foreign keys all potentially pointing to different parent tables are allowed.  
Defined in the CHILD table, not the parent table  
Does not need to be a unique key

```
CREATE TABLE Enrolled(
sID int,
cID char(20),
grade char(2),
PRIMARY KEY (sID, cID),
FOREIGN KEY (sID) REFERENCES Students(sID))
```

If the referenced key is modified, what should we do in the referencing table? 

1. Delete corresponding record(s) - If the referencing record has no meaning on its own
2. Nullify foreign key - If we want to keep the data, but “unlink” it. We can still analyze, e.g. average historic GPA. Generally bad design
3. Disallow changes to referenced table - Try to delete Joe and SQL reports error. If we need to take some action first.

SQL will reject adding data to a child table with a foreign key that isn’t present in the parent