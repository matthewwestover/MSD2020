## Week 3 - Day 2
### SQL Quiz
Query for getting cardnum, phonenum, and name for “Dan”?  
`select * from Patrons natural join Phones where Patrons.name=“Dan”;`

Get Titles of first 3 books in inventory (by serial)  

```
select Title from Inventory natural join Titles where Serial<1004;
select Title from Inventory natural join Titles order by Serial limit 3;
```

Get Titles of first 3 books in inventory (by Title)  
`select Title from Inventory natural join Titles order by Title limit 3;`

### Additional SQL
Results from a join is a table that can be joined  
`(Patrons join CheckedOut) join Inventory`

Query to get titles and authors of books checked out by Joe?  
`select Title, Author from Patrons natural join CheckedOut natural join Inventory natural join Titles where Patrons.Name=“Joe"`

#### Inserting Values to a table  

```
INSERT INTO <table name> VALUES (value1, value2, ... ); // does this in order of columns
insert into Titles values (“978-0441172719”, “Dune”, “Herbert”);

insert into <table name> (col1, col5) values( value1, value5 ); // inserts in col1 and col5
```

#### Modifying values
`update <table name> set col1 = val1, col2 = val2 where ...`

#### Add a Column
`ALTER TABLE <table> ADD <column>`