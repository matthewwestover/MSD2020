## Week 2 - Day 2
### Entity Relationship Models
Items can have a hierarchical type:

```
class Employee { int SSN; string name; }
class HourlyEmployee extends Employee
{  float wage;  }
class SalaryEmployee extends Employee
{  float salary; Benefits b;  }
```

HourlyEmployee “IS-A” Employee  
IS-A is denoted with a triangle  
The top of the triangle is “base class” in this case employee  
The bottom parts of the triangle are the subclasses

Entity or Attribute?  
It’s usually obvious: - A student is an entity -A student ID is not an entity  
Is it complex data that needs its own keys? If yes - entity  
Does the data type make sense in another relationship? If yes, it cannot be an attribute  
Can there be more than one? - likely an entity  
It is possible to make entities attributes  
Ex: addresses tied to an employee  
As an attribute, just make address start/end date  
But as an entity it can be unique addresses then attach it to the employ as an entity via a relationship  
But this can be tricky. Can an employee move back to a prior address? Not immediately in this set up  
While in set notation this is many to many, 

#### Weak Entity
A weak entity can’t be identified by its own attributes  
It is identified by a combination of: its own attribute(s) and a foreign key (something else’s key attribute)  
i.e., its existence only makes sense in the context of another entity  
Consider the difference between a course and a class  
Course: Subject, Number, Name, Description, CS 6016 Database Systems, “in this class we study …”,  
Class: Semester, <reference to course>, <reference to teacher> Spring 19, <reference to CS6016>, <reference to teacher>  
Classes have no meaning without a Course. It gains info from the Course as a specific instance  
Double rectangle for the entity set - Classes   
Double diamond for the supporting relationship - Listed as a instance of  
Supporting relationship must be 1-to-M, and weak -entity must participate

`|Course| - 1 - ||Instance of|| -—M—- ||Class||`

A weak entity can’t be identified by its own attributes  
It is identified by a combination of: its own attribute(s) and a foreign key  
A weak entity set has a partial key  
Shown by a Dashed underline  
Must be combined with a foreign key: {cID, Semester} is the key for Classes

### SQL Tables
```
create table <name> (
<column1Name> <type> <properties>, 
<column2Name> <type> <properties>, ...
<table properties>
);
```

Properties are optional

#### Data Types
Numeric:  
Integers – int, \<tiny, small, medium, big>int, <unsigned>  
Reals – float, double, decimal  
Dates – very important in DBs - Date, datetime, time stamp, time, year 
 
Strings: char(N) - exactly N characters & varchar(N) - up to N characters  
Blobs – Binary Large Objects •Enums  
CHAR(N) - exactly N characters: ISBN, Phone number( We could use BIGINT UNSIGNED if we left out the dash ‘-’)  
VARCHAR(N) - up to N characters: Title, Author, Name  
How to pick (N) for VARCHAR(N)?  
Requires N bytes for storage plus 1 or 2 bytes for size  
1 size byte if N <= 255  
2 size bytes if N > 255  

#### SQL Properties
Column properties: `NOT NULL, DEFAULT ‘hello’, AUTO_INCREMENT`  
Table properties: `PRIMARY KEY (column1, …), UNIQUE (column1, …)`

##### Not Null
Improves index optimizations ... it’s not just for table design  
Specifying NOT NULL is not required on key values, But SQL will automatically set it

#### Creating Table Examples
```
create table Titles (
ISBN char(14) not null,
Title varchar(255) not null, 
Author varchar(255) not null, 
primary key(ISBN)
);

create table Inventory (
Serial int unsigned not null auto_increment, 
ISBN char(14) not null,
primary key(Serial)
);
```

#### SQL Foreign Keys
```
FOREIGN KEY (<column>) REFERENCES 
<table>(<table’s key>)
ON DELETE <action>
ON UPDATE <action>
```

\<action> can be:

* RESTRICT (Default): disallow the change
* CASCADE: also delete/update in child table
* SET NULL: nullify key in child table
* SET DEFAULT: set to column’s default value

##### Example
```
CREATE TABLE Phones ( ...
FOREIGN KEY (CardNum) 
REFERENCES Patrons (CardNum) 
ON DELETE CASCADE
)
```

#### Reducing ER Models to Schema
We’ve been using a “naïve”, but correct relational model , Sometimes resulting in unnecessary tables  
If we start with a good ER model, the translation to a good RM is mechanical

Basic algorithm:

1. Every entity set becomes a schema - every set becomes a table
2. Relationship sets might become schema depending on cardinality, otherwise they are merged

Entity Sets  
Every entity set translates directly to a schema  
Attributes become columns (The Ovals)  
Pick one of the key attribute sets as the primary key. 