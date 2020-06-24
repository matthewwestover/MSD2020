## Week 2 - Day 1
### Entity Relationship Models
Keys define the data not the other way around  
Keys apply to ALL possible instances of the data, not what is currently in the database

Database Design Steps

* Requirements Analysis
    * What does user need? What must it do?
* Conceptual Design
    * High level formal description
* Schema Refinement
    * Consistency and “normalization”
* Physical Design
    * Indexes, disk layout
* Security Design
    * Who accesses it, and how?

Conceptual Design - Accomplished using “Entity-Relationship” (ER) model  
We generally move from a problem to a solution: We don’t code in assembly(Problem specification) → Code in C++ → compiler coverts assembly  
For databases this is: Problem specification → ER Model → Schema  
You **CAN** skip the middle step (coding in C++ or the ER Model for Databases) but this is **BAD**

What are the entities, and their relationships?
To do this don’t think about tables

#### Entities
A real-world object, distinguishable from other entities  - `{u0123456, “Danny”}`  
An entity is described by a set of attributes  - `(uID string, name string)`

#### Entity Set
A collection of entities of the same type, e.g.  All students, All buildings, All people  
All entities in the set have the same types of attributes - An entity set has a key

In diagrams  
Square = Entity set (Employees, Department)  
Oval = Attribute (SSN, DepartmentID)  
Underline = key attribute

#### Relationship Set
Once you have multiple sets you need to define the relationship between them  
Relationship between 2 or more entities: “Danny works in School of Computing” = entity - relationship - entity  
Relationship Set:  
Set of relationships between entities of same type e.g. works in relates Employees to Departments  
Relationships can have attributes as well: Danny works in SoC since 2010. 
Starting date does not belong to Danny or SoC - It belongs to the relationship

In diagrams  
Diamond = Relationship   
Does not need a key, but does have oval attributes

#### Cardinality
Mapping between sets.  
Can a data set belong to multiple things at once?  
1 to 1 means 1 employee per department  
But what if they belong to multiple, or if a department has multiple people?  
Many to Many means multiple connections in any direction  
1 to many means one side only has one connection.   
Annotate opposite edge of relationship with cardinality  
“An employee can work for one department, but a department can have many employees” = 1-to-Many  
Employee connection to works in diamond is a 1, the department to works in connection is M  
“A student can take multiple classes, and a class can have multiple students” = Many-to-Many  
Both connections to the diamond is M  

Bold lines indicates all entities in the set must participate - Can also use a double line.  
This is an at LEAST once instead of having 0 or more
