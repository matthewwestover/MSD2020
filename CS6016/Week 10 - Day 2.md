## Week 10 - Day 2
### Indexing
Index and SQL Queries  
Having the right index can speed up queries, especially joins.  
Imagine SQL joins are implemented via nested loops over relations.  
If an index exists on a relation, it is possible to replace one loop (linear scan) with a tree search.  
This is key to understanding the interaction of indexes and joins.  
Indexes will also be used within joins to trim tables before joins.

### B+ Structures
All nodes must be >= 50% full - Unless it’s the only node at that depth  
Adding/Splitting nodes: k items per node, During add, we only split if the node is full  
Delete: Redistribute, maintain order between siblings  
Potentially update parent  
Right side pointer means >= all children  
If Redistributing breaks the >=50% full rule: merge into one node  
If parent empty, delete parent  
Continue recursively if special cases apply to parent, e.g. merging two leaves might cause merging two parent

Special construction maintains balance at all times  
Guarantees no more than ~50% space overhead

### Hash Index
Basic principal: use hash table instead of tree  
Buckets can be page-sized  
bucket = hash(key) % numBuckets.  
Then search through bucket: Multiple keys hash to same bucket  
If bucket gets full: Linear probing, Quadradic probing, Separate chaining

### Hash as Secondary Index
Buckets contain primary key of records  
Additional lookup to find record (from primary index)

Pros: Very fast insert/delete/lookup, Good option if your database is a map (key-value store)  
Cons: Does not support range comparisons, Performance degrades if too full

Normally we rehash to keep load factor < 50%  
Needed to guarantee O(1)  
What about in a DBMS? Rehash requires touching every single byte! This could take days (literally)  
Because of this, hash indexes have limited utility: Useful if number of items is well-bounded

### Tree Index
Pros: Speeds up range queries, Speeds up equality queries, Speeds up ORDER BY, Doesn’t need to be rebuilt  
Cons: Not quite as fast as hash (if no rehashing)

### Spacial Data
Suppose we have a database of building locations  
For simplicity, assume <x, y> coordinates  
Searches can be like: Find all fast food within d kilometers, Find the k nearest hospitals, Find all locations within some boundary

MySQL types: POINT, LINESTRING, POLYGON  

Standard search trees will not work, How would we decide that one point is “less than” another?  
Use a 2D Grid  
Divide space into regular grid  
Find all points within d radius, Expand outward until radius covered, Reject false-positives  
KNN could work similarly, Expand from center until K found

How does a grid handle empty space? Poorly!

The better solution uses a tree  
Warning: we will look at a “dumbed down” version  
Not optimized for page access  
Ignore insert/delete  
Only handles point data (not polygons)  
Naïve construction strategy

### Bounding Box
If X is d units away from the box that bounds a set of points, then all of those points are >= d units away  
Recursively subdivide region into rectangles  
Search for KNN of x  
Form a tree of the bounding boxes  
Since they form a hierarchy, can reject entire branches of tree by testing against one node of tree.  
Keep doing so until you hit a leaf node, which is a point itself.  
This is a nearest-neighbor.  
Do so K times for KNN.

### R-Tree
The “real” spatial index for a DMBS is an R-Tree  
Wider branching factor (page-based nodes)  
Always balanced  
Splitting/merging/redistributing to handle insertions and deletions

### Spacial Data in SQL
Make a table for storing the name and x/y address of fast food chains

```
create table FastFood( 
    Name varchar(100), 
    Location point
);
```


Location will hold 2D coordinate (400e, 255s)

Points are stored in a binary form, and not readable by default   
Use AsText(...) to convert to readable form   
`select AsText(Location) from FastFood;`.  
Use point(x, y) to create a point from two numbers  
`insert into FastFood values ('Arbys', point(420, -400));`  
ST_DISTANCE(p1, p2) returns distance between the two  
Find distance from origin to all points:  
`select ST_DISTANCE (point(0, 0), Location) from FastFood;`  
Creating a Spacial Index (R-Tree if available):  
`CREATE SPATIAL INDEX i ON T (col);`