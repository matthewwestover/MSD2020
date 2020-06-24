## Week 5 - Day 1
### Advanced SQL Queries
Find all people with the same phone number - people with DIFFERENT card numbers with the SAME phone number  
Phones X Phones  
Combines all phone numbers with each other  
This will get you closer as the table will now rows with duplicate phone numbers  
We need to pull out the rows where the two card number columns are different

To do this slightly better rename first  

```
⍴(P1, Phones)
⍴(P2, Phones)
σP1.Phone = P2.Phone(P1 x P2)
```

This pulls out the rows with the same phone number, but still has rows where CardNum = CardNum

`σP1.Phone = P2.Phone(P1 x P2) && P1.CardNum != P2.CardNum(P1xP2)`

#### SQL Self-Join

```
select p1.CardNum, p2.CardNum 
from Phones p1 join Phones p2 // Have to rename or else SQL has issues. 
where p1.Phone = p2.Phone 
and p1.CardNum != p2.CardNum;
```

Join  
Default join is called an inner join  
`x JOIN y WHERE … // Gives rows where condition is true`  
Equivalent to x INNER JOIN y or  x, y

Outer Join  
Two types of outer join: LEFT and RIGHT  
ON clause required!  
`x LEFT JOIN y ON condition`  
Gives all rows where condition is true  
And gives all rows from x

`x RIGHT JOIN y ON condition`  
Gives all rows where condition is true  
And gives all rows from y

`Patrons p LEFT JOIN CheckedOut c ON p.CardNum = c.CardNum;`  
Shows all rows from Patrons, and several of the rows have NULLS based on not having things checked out. Some Patrons can have multiple rows if they have multiple books checked out  
CardNum columns get listed twice in this case, With nulls from the CheckedOut column

`Patrons p RIGHT JOIN CheckedOut c ON p.CardNum = c.CardNum;`  
Removes the NULL rows, and you don’t see the patrons that haven’t checked out a book. 

To clean up and remove dup rows like the CardNum use a shortcut: NATURAL  
`Patrons NATURAL LEFT JOIN CheckedOut;`

Find Names of Patrons who have not checked out a book

```
SELECT Name
FROM Patrons NATURAL LEFT JOIN CheckedOut
WHERE CheckedOut.Serial is null;

SELECT p.Name
From Patrons p LEFT JOIN CheckedOut c ON
P.CardNum = C.CardNum 
WHERE C.Serial is null;
```

NULL is a special value in SQL  
It does not have the reflexive property  
i.e. NULL != NULL  

```
WHERE CardNum != NULL // wrong
WHERE CardNum IS NOT NULL // right
```

Nested Query as a Condition  
There are multiple versions of this  
X is a value, A is a multi-set

```
SELECT x FROM y WHERE x IN (SELECT …);
x IN A // True if X is in A
EXISTS A // True if A is not empty
x OP y ANY A // True if there exists a y in A such that x op y is True.
x OP y ALL A // True if for ALL y in A, x op y is True.
```

Find all students not enrolled in Databases


```
select s.Name from Students s 
where s.sID not in
(select e.sID from Enrolled e 
natural join Courses c
where c.Name='Databases’ );
```

Find all students younger than everyone taking Database

```
select s.sName from Students s 
where s.DOB > all
(select DOB from
Students natural join Enrolled 
natural join Courses c
where c.Name='Databases’);


// Can be thought of as a nested for-loop
select s.sName from Students s
where s.DOB > all
(select s2.DOB from Students s2 
natural join Enrolled 
natural join Courses c 
where c.cName='Databases’);
```

Exists  
Filter by complex nested query  

```
select x from y where EXISTS
(select ...);
```

If any rows exist in nested query, x is selected

```
select x from y where NOT EXISTS
(select ...);
```

If nested query empty (returns nothing), x is selected

Division  
Find Students taking all classes  
Can combine as nested division to accomplish this

```
select s.sName
from Students s // For-each Student
where not exists(select c.cID
                from Courses c // For-each Course
                where not exists(select e.cID
                                from Enrolled e // For-each Enrolled
                                where e.cID = c.cID 
                                and e.sID = s.sID));
```