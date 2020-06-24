## Week 5 - Day 2
### Advanced SQL
SQL has pseudo regex:

`... WHERE Title LIKE ‘J_%m’ // 'J’ followed by at least one arbitrary character followed by ‘m’`  
‘_’ means any one character  
‘%’ means 0 or more arbitrary characters

Limiting Results  
‘Limit' the number of rows returned  
`SELECT ... LIMIT 5;`  
Useful in combination with ORDER BY  
Find ... with smallest CardNum  
`SELECT ... LIMIT 1 ORDER BY CardNum;`  
Find ... with biggest CardNum  
`SELECT ... LIMIT 1 ORDER BY CardNum DESC;`

Aggregate Functions  
An aggregate function returns a single number from a multiset  
Usually used on a single column

```
COUNT(…)
MAX(…)
MIN(…)
SUM(…)
AVG(...)
```
Remember: aggregate functions don’t return a row - They aggregate a group into one value  
Find the name of the oldest student in each class

```
Select sName, joined.cID from
(select * from Students natural join Enrolled) as joined
join
(select cID, min(DOB) as dob from Students natural join Enrolled group by cID) as eldest
where joined.cID = eldest.cID and joined.DOB = eldest.dob
```

In summary: if we want the rest of the row associated with min/max, we need additional joins/filters. 
Be careful of duplicates (tie for oldest student)

Count  
Returns count instead of rows. 
How many copies of ‘Harry Potter’?

```
SELECT COUNT(*)
FROM Titles NATURAL JOIN Inventory WHERE Title='Harry Potter';
```

Group By  
Gives one representative row for each group in the specified column  
GROUP BY usually used in conjunction with an aggregate function  

Find number of students in each class.  
`Select count(*), cID from Enrolled group by cID;`  
Find number of classes for each student  
`Select count(*), sID from Enrolled group by sID;`

Having  
Filter on groups (instead of rows)

```
select Major, count(*) from Students 
group by Major
HAVING count(*) = 2;

//Better Version
select Major, count(*) as c
from Students group by Major 
HAVING c = 2;
```

Where vs Having  
Where is used before GROUP BY to filter rows  
Having is used after GROUP BY to filter groups, or on the result of an aggregate function

Find students with 2 or more classes, earning an A in every class

```
select sID, count(*) as c 
from Enrolled e
where e.Grade=‘A’
group by sID
having c >= 2;
```

Date  
DATE is a compound type, containing: Year, Month, Day  
But we can still do comparisons on them:  
`SELECT ... WHERE DOB > ‘1990-01-01’;`  
Better to be explicit:  
`SELECT ... WHERE DOB > DATE(‘1990-01-01’);`  
We can extract one component from a date:

```
select YEAR(DOB) from Students;
select MONTH(DOB) from Students;
select DAY(DOB) from Students;
```

This does NOT account for leap years, math gets wonky

TIMESTAMPDIFF  
Gives number of years, months, days, or seconds between two dates and it accounts for leap years  
`SELECT TIMESTAMPDIFF(unit, date1, date2) ...`

CURDATE()  
We don’t want to have to rewrite this query every day:  
`SELECT TIMESTAMPDIFF(unit, date1, DATE(“2019-02-11”)) … `  
CURDATE gives us the server’s date  
`SELECT TIMESTAMPDIFF(unit, date1, CURDATE()) ...`

Case  
Conditional select is possible  

```
select
case
when Grade = ‘A’ then ‘Superior’ 
when Grade = ‘B’ then ‘Good’ 
when Grade = ‘C’ then ‘Adequate’ 
else ‘Poor’
end as Remarks
from Submissions;
```

Returns a column containing “Superior”, “Good”, or “Adequate” values  
Usually want to rename it using ‘as’ - end as Remarks

Delete With Conditional  

```
DELETE CheckedOut FROM
CheckedOut join Patrons WHERE
```

Without specifying CheckedOut it would delete from the temp table created from CheckedOut join Patrons which doesn’t accomplish anything

Write a query to checkout “The Lorax” for “Joe”  
There may be multiple copies of “The Lorax”  
Some or all of them might already be checked out  

```
Insert into CheckedOut values(
(select CardNum from Patrons where Name=“Joe”),
(select Serial from Inventory natural join Titles where Title = “The Lorax” and Serial not in (select Serial from CheckedOut)
limit 1) );
```

Who Does What?  
MySQL server can do many things: 

* Sort
* Select
* Compute
* Average
* Filter

Programmers can do many things:

* Sort
* Select
* Compute
* Average
* Filter

MySQL It’s probably faster/better at it - It has an optimizing plan generator