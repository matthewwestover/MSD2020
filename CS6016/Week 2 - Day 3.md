## Week 2 - Day 3
### Additional ER To Schema
Many-to-Many: Primary keys of relating entities as foreign keys. The combination of the two is the primary key for that table  
1-to-Many (better option): Department can only have 1 manager Merge the relationship into the Entity.  
These merged tables become columns (manager and budget) The foreign key is the primary key of the not combined table.  
1-to-1: Treat as 1-to-M. If participation required on one side, use that side Add NOT NULL  
Merge relationship into one of the other tables  
Then comes the question of what has to be enforced as not null  
If participation is required, set NOT NULL. Else, NULL is allowed  
Referential integrity only enforced when inserting non-null data ( ie Not all departments have a manager)

In general:  
Fewer joins = better performance  
Fewer tables = fewer joins  
1-to-M and M-to-1 don’t need their own tables. 
Supporting Relationship: A supporting relationship is 1-to-M: Same procedure, except supporting key is combined with partial key  
If participation required on both sides, Regardless of cardinality, difficult to capture with schema design. Instead, enforce with SQL commands  
NOT NULL determined by participation constraints, Total participation on both sides – don’t capture with schema

