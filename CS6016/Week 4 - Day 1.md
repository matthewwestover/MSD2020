## Week 4 - Day 1
### Relational Algebra
#### “Division"
Consider a list of Cars and Parts. A third table that has a list of what cars use what parts.   
Getting all cars that us part p1 `πcn(σpn=p1(A))`  
Get all cars that use p2 and p4, the table might be huge.  
Create a Table B that has these two parts.  
Use relational algebra / operator: A / B gives the cars that use those two parts

As / doesn’t really exist we have to construct it using the available operators  
`πx(A) — (πx((πx(A) X B) — A))`

1. Project x out of A. Removes duplicates, gives c1,c2,c3,c4. Call this A1.
2. Let C = A1 x B. Gets all possible combinations of c1,c2,c3,c4 and p2,p4. Has spurious combinations of cars and parts. Basically all rows get combined between the two tables. This does not reflect what is actually in table A
3. Get D = C - A. This will leave only the “fake” combinations of cars and parts. Here, c2 and c3 do not use part p4. Should not be in output of division.
4. Project x out of D. That should give the cars c2 and c3 that do not have those parts. We want cars that do have the parts. Call this D1.
5. Calculate A1 – D1 (all cars minus cars that don’t use the parts we want).

```
A1 = πx(A)
C = (πx(A) X B)
D = (πx(A) X B) — A)
D1 = (πx((πx(A) X B) — A))
```

#### Natural Join
`R1 ⋈ R2 -> R3`  
Just like SQL natural join  
Select all rows from R1 X R2 where their common attributes have equal values  
Output schema has just 1 copy of common attributes

#### Theta Join
`R1 ⋈condition R2 -> R3`  
Like SQL JOIN ... WHERE  
Output schema same as R1 X R2  
Shortcut for `σcondition(R1 X R2)`  
We can use logical operators like && and = as part of the condition.

#### Rename
Often useful to declare ‘variables’ by renaming a relation - Not really an operator  
`⍴(newname, expression)`