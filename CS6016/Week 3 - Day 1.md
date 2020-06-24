## Week 3 - Day 1
### More ER to Schema
#### Weak Entities
A weak entity can’t be identified by its own attributes  
It is identified by a combination of: its own attribute(s) and a foreign key (something else’s key attribute)  
i.e., A class is a weak entities of a course.   
To be identified class needs to see some attributes from the courses

Why not just add an ID field to an entity to make it a “strong” entity?  
This removes the connection to the strong entity and its attributes the weak one needs to describe it  
A weak entity are subsets of strong entity 

Intuitive meaning  
Weak Entity – a class is an instance of one specific course  
Participation Constraint – a department must have a manager, but the manager can change  
i.e., a Class can not change which Course it is tied to

Formal meaning  
Weak Entity – its uniqueness depends on a foreign attribute (plus its own)  
Participation Constraint – does not say anything about uniqueness

A weak entity can be supported by another weak entity - As long as the chain is anchored by a strong entity
Keys get progressively more complex down the chain - try to avoid super long chains
This only becomes a concern when translating to schema - ER diagram does not care

#### IS-A to Schema
Method 1:  schema per entity and pull down primary key from the top. The primary key gets labeled as the PK and the FK to the top level

Method 2: no schema for base type - pull down all attributes
This means the “primary” top class isn’t created but in the diagram it is shown the two IS-As are related. 

Is the base type “abstract”? Meaning, can’t specifically create one?  
If yes, use method 2

Can an entity be two different derived types?  
If yes, consider method 1

### History of DBMS
1970s – commercial database systems becoming popular. Many different languages - impractical  
SQL – one of the first languages to implement the relational model (from Edgar F. Codd) – “Structured Query Language”  
ANSI standardized SQL in 1986 - Revised many times, most recently in 2016  
2010s -Tons of new entrants, mostly non-relational: Firebase, MongoDB, Oracle NoSQL, Redis

SQL is the language (like C++)  
DBMS is the implementation (like gnu/g++)  
Many SQL implementations: MySQL, PostgreSQL, SQL Server  
Varying behavior, despite the standard.

SQL vs Sequel  
Originally called “Structured English QUEry Language” - Forced to change name – no longer an acronym  
If you interview at Microsoft, recruiters may not understand you if you say SQL

SQL is a combination of languages:  
DDL: Data Definition Language: create/modify tables and settings  
DML: Data Manipulation Language: create/modify/delete tuples, search for tuples

### Data Definition Language (DDL)
```
create table <name> (
    <column1Name> <type> <properties>, 
    <column2Name> <type> <properties>, 
    ...
    <table properties>
);

drop table <name>;

alter table <name> 
    add ...
    drop column ...
CREATE TABLE table_name (
col_name type [DEFAULT default_expr] 
[col_constraint [, ...]] | 
table_constraint [, …]
);
```

Column constraints: 

```
col_constraint =
NOT NULL | NULL | UNIQUE | PRIMARY KEY | CHECK (expression) | AUTO_INCREMENT | REFERENCES reftable [ ( refcolumn ) ] [ ON DELETE action ] [ ON UPDATE action ]
```

Several are ignored in MySQL (Check, References, On Delete, On Update). They are instead viewed as table level

Table constraints:

```
table_constraint =
[CONSTRAINT constraint_name]
UNIQUE(col_name [, ...]) |
PRIMARY KEY(col_name [, ...]) |
FOREIGN KEY(column_name [, ...]) REFERENCES reftable(refcolumn [, ... ])]
[ON DELETE action] [ON UPDATE action]
```

### Data Manipulation Language (DML) 
```
select ...
insert into ...
delete from ...
```

#### Select
```
SELECT [DISTINCT] target-list FROM relation-list
[WHERE qualification]
[ORDER BY column] [DESC]
[LIMIT     number]


select <columns> from <table> // gets the entire table. 
```

Multiple tables?  
Remember that we are dealing with relations: schema + instance  
select commands input a relation and result in a relation  
We need a new relation that combines these two
`<table> join <table> //creates a temporary table`

SQL has lots of ways of doing things

```
select * from Phones join Patrons;
select * from Phones, Patrons;
select * from Phones inner join Patrons;
```

#### Where
Apply a conditional filter to any select

```
SELECT ... FROM ... WHERE <condition>
WHERE CardNum > 2 WHERE Title = ‘Dune’

// Works on tables
SELECT * FROM Patrons JOIN Phones WHERE Patrons.CardNum=Phones.CardNum;
```

\<condition> can be complex (combined with AND, OR, etc)  
Conditions comprised of comparison(s): `<, >, <=, >=, =, !=`  
`HERE Name=‘Joe’ AND Age >= 18`  
`SELECT * FROM Patrons JOIN Phones WHERE Patrons.CardNum = Phones.CardNum AND Patrons.Name = ‘Joe’;`

#### Join on
Adds a filter to the join  
`<table> join <table> on <condition>`

\<condition> - Usually want to compare a column from each table:  
Phones.CardNum = Patrons.CardNum  
Can produce a smaller quicker table with the necessary results we need

`Patrons.CardNum = Phones.CardNum`  
vs.  
`Patrons JOIN Phones WHERE
Patrons.CardNum = Phones.CardNum`

A subtle difference:  
“Where” filters AFTER joining.  
“On” filters BEFORE joining.  
Doesn’t matter for inner joins, but will matter later.

#### Natural join 
Combines the duplicate columns into one. How we would typically think a combination of two tables would be if there was an overlap of values (foreign keys) 



