## Week 4 - Day 2
### RA to SQL
Project (π) -> Select

```
SELECT col1, col2, … if RA has a projection
SELECT * if RA has no projection
```

Select (σ) -> Where  
`WHERE <condition>`

Renaming  

```
select p.CardNum // Can use ‘p’ before its declared as p
from Patrons p 
where p.Name = ‘Joe’;
```

Nest Queries

```
select Serial from
(select ISBN from Titles where Title = 'The Lorax') as lorax //store all books titled ’The Lorax’’s serial #s
natural join Inventory;
```

Cross Product -> Join  

```
R1 JOIN R2 // R1 X R2
R1 NATURAL JOIN R2 // R1 ⋈ R2
```

Union (U) -> Union

```
SELECT Addr FROM CorporateLocs UNION // if you want to allow duplicates use UNION ALL
SELECT Addr FROM RetailLocs
```

Intersect -> Intersect  
Not supported in MySQL  

```
SELECT * FROM
(SELECT Addr FROM CorporateLocs) AS corp NATURAL JOIN
(SELECT Addr FROM RetailLocs) AS retail;
```

Same schema = natural join is the same

In  
Filter by nested query  
If nested query appears after IN, it is not renamed  
Any x that are part of nested query are returned  

```
SELECT x FROM y WHERE x IN
(SELECT …);

SELECT Addr FROM CorporateLocs
WHERE
Addr IN (SELECT Addr FROM RetailLocs);
```

Set Difference (—) -> Not In

```
SELECT Addr FROM CorporateLocs
WHERE
Addr NOT IN (SELECT Addr FROM RetailLocs);
```

### Exercise

1.All Patrons who have not checked out a book

```
SELECT p.Name from Patrons p
WHERE p.CardNum NOT IN
(SELECT CardNum from CheckedOut)
```

2.All Patrons who have checked out ‘The Lorax’AND ‘Harry Potter’

```
SELECT Name FROM Patrons WHERE Patrons.CardNum IN 
(SELECT CardNum FROM CheckedOut NATURAL JOIN Inventory NATURAL JOIN Titles WHERE Titles.Title=’The Lorax’)
NATURAL JOIN
(SELECT CardNum FROM CheckedOut NATURAL JOIN Inventory NATURAL JOIN Titles WHERE Titles.Title=’Harry Potter’)
```

### Weak Entity Chain
Keys get progressively more complex down the chain  
This only becomes a concern when translating to schema - ER diagram does not care  
Key starts at A, then A,B, then A,B,C as you go down the list  
Primary Keys should be simple.  
Achieve same constraint with UNIQUE, assign a “simple” primary key that references the combined levels.  
{b, a}. Third level becomes {bID, c} instead of {a, b, c}