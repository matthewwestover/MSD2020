## Week 3 - Day 3
### Relational Algebra
A query language is not a programming language - Usually not even Turing-complete  
Any programming language can implement any computational problem - But we start with an abstract algorithm description

A programming language algorithm writing 

```
function merge(left, right)
var result := empty list
while left not empty and right not empty
if first(left) ≤ first(right) then
```

For Query languages, much like an algorithm, it is powerful to specify a query in abstract terms.  
`Title, Author (Titles X Inventory)`

Relational Algebra - Operational – describe plan of execution  
Relational Calculus - Declarative – describe what you want in the end  
These form the basis of practical languages like SQL - The two are logically equivalent (Codd’s Theorem). Our focus is on relational algebra.

Algebra that operates on relations  
Operators consume and produce relational instances. Only relations go in, relations come out

π - Projection  
σ - Selection  
x - Cross Product  
— - Set Difference  
U - Set Union

Operations are closed: input/output is always a relation - Thus, we can compose operations

#### Projection
Get certain columns from relation.  
`πcolumn list` (relation)  
`πTitle, Author` (Titles)  
`πTitle, Serial (Titles X Inventory)` - Gets tables of Titles and Inventory, outputs a table with them joined.  

SQL is relational algebra (loosely)

```
SELECT Author FROM Titles;
Herbert
Herbert

πAuthor (Titles)
Herbert
```

Relational Algebra output never has duplicate rows. SQL can show “duplicate” rows when the rest of the line has different data  
Why does SQL not eliminate duplicates by default? Because it’s computationally expensive

#### Selection
Filter rows on condition  
`δcondition` (relation)  
`σCardNum > 3` (Patrons)  
`πPhone(σCardNum > 3 (Patrons X Phones) )` - Phone number of any patron with a card number greater than 3

#### Cross Product
Binary operator (takes two operands) relation1 X relation2 → relation3
Does cross product need to eliminate duplicates? No, All R1 are unique and all R2 are unique  
Is cross product commutative? R1 X R2 == R2 X R1? Yes, SQL analog (JOIN) may give columns in a different order, but it doesn’t matter

#### Set Union
`relation1 U relation2 → relation3`  
Takes two sets and makes them one.  
It is like “add”
Relations must be union compatible (same schema) i.e. same column types in same order  
Duplicate values must be removed. 

Is union commutative? R1 U R2 == R2 U R1? Yes

#### Set Difference
Think of it like subtract. Remove tuples from relation1 that are also in relation2  
Relations must be union compatible  
Is it commutative? No. R1 gets things removed that are in R2 and the reverse. If they are different the outputs are different

#### Set Intersection
These are the 5 operators, can we create an intersections where only the same values remain?  
`A - (A - B)`  
(A-B) gives the chunk of not overlap in the original. Subtract it from A provides only the overlap area. 

#### Exercises
Locations that are retail only:  
`πAddr(RetailLoc) — πAddr(CorpLoc)`

Locations that are corporate only:  
`πAddr(CorpLoc) — πAddr(RetailLoc)`

Locations that have both retail and corporate:  
`πAddr(CorpLoc) — (πAddr(CorpLoc) — πAddr(RetailLoc)) or πAddr(RetailLoc) — (πAddr(RetailLoc) — πAddr(CorpLoc))`

Managers who are friends (manage at same address):  
`πManager(CorpLoc - (CorpLoc - RetailLoc))
πManager(σR.A = C.A(R x C))`
