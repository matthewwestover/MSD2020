## Week 12 - Day 1
### Query Evaluation
SQL is good for human-level specification - Not so good for operational specification  
Like a compiler, DBMS must translate and optimize the high level language - Result is a plan

SQL -> Plan  
1. Parse and translate  
2. Optimize  
3. Execute the plan

Assume no indexes unless specified - Whether they exist or not greatly affects the optimal plan

Consider this SQL query on Chess:

```
SELECT Name FROM Players p
  WHERE Elo < 2000
Two possible translations:
pName(sElo<2000(Players)) 
sElo<2000(pName(Players))
```

Projecting last reduces number of rows to project


DBMS looks at many reasonable plans • Picks the one likely to be fastest  
“Likely fastest” determined by heuristic or relational algebra rules.  
For instance, can “permute” certain relational algebra operators to minimize number of rows in output.  
Can fold together operators… Two projections – only the outermost projection matters

```
SELECT Result FROM Games G JOIN Events E ON G.eID = E.eID
WHERE Date = “2005-03-05”
```

This has two options in SQL. Join everything then select, or select date then join  
G = 1000 rows  
E = 100 rows (5 with specified date)

Joining first = Total: 101,005  
Select first = Total: 5,105  

Indexes  
What if there is an index on Date?  
Plan selection takes them into account

Join  
How does a JOIN actually work?  
Nested-Loop Join  
Merge Join.  
Hash Join  

### Nested-Loop Join
R NATURAL JOIN S  

```
foreach r in R foreach s in S
    if(keys match)
      add <r,s> to output
```      

Algorithmic cost: |R| * |S|  
Actual cost: depends on disk, memory, etc  
I/O cost, assuming small memory  
Memory holds 1 page from R and 1 from S  
Page load count: Only 1 page of R and 1 page of S fits in RAM  
Total page loads = Pr + |R|*Ps  
If one relation is small enough to fit in memory, it should be the inner loop  
We always call the outer relation R and the inner relation S  
DBMS chooses which of the real relations becomes R or S  
R = outer relation, Pr = total pages in R  
S = inner relation, Ps = total pages in S  
Worst-case page loads: (|R| * Ps) + Pr  
Best-case page loads: Ps + Pr. 


A smart DBMS will automatically reorder the search to keep pages cached for as long as possible  
i.e. work on smaller parts of the join at a time, much like array tiling

### With Indexes

```
foreach r in R
if(s = indexLookup(r in S))
    add <r,s> to output
```

Outer loop requires linear scan no matter what  
Index does not help if we have to scan every record anyway  
If R has index: (|R| * Ps) + Pr  
If S has index: (|R| * log(Ps)) + Pr  
Always make S the one with the index

Multi Join  
`SELECT ... FROM A JOIN B JOIN C WHERE …`  
 Multiple options of where they stack. 

Explain  
We can add EXPLAIN in front of a query to show the chosen plan  
Gives info about each table used (R, S, …)  
Lots of info per table:  
id: which SELECT it’s used in  
key: which key is used (if any)  
rows: estimate of rows scanned  

MySql automatically creates an index on any foreign key because they are regularly used in JOIN  
If you really want to get rid of that index, you have to get rid of the FK

The best thing you can do is pick the right index(es), let the plan generator do the rest  
However, you can “block” the plan generator  
The order of clauses in your statement does not matter much  
But using fundamentally different clauses can matter a lot!  
self-join WHERE filterA OR filterB  
Vs  
filterA UNION filterB. 