## Week 3 - Day 4
### Hash Tables

Fancy wrapper around an array  
Index the data goes into is based on a hash value of the data  
Most common way is to take the hash value % array size  
Mod (%) array size can be seen as included in the hash value calculation or separate if it is easier to piece together  
All hash table operations are O(1) with careful tricks  
Collision is the hardest part handle, along with clever hash value equations to get there  
Ints have an obvious hash value (itself)  
Things like Strings, or custom objects (Books, Circles, etc) need to have a good hash function  
A hash function should always return the same output for the same object every time you pass it in  
Two different inputs CAN have the same hash value  
Around half full arrays should be grown to prevent clustering  
All items should have their hashes update the indices in the new grown array to remove some collisions and clustering  

Linear probing typically uses a secondary list to mark each index as null, active, or deleted  
This is because collisions just push the data to the next empty array slot  
If an item is removed, you need to not just stop at empty in the data array when looking for an object. Use the secondary array to know when to stop  

All efficiency on Hash Tables depends on if the hash function is good or not. If it is rubbish with lots of collisions, or slow to run hash tables are useless  

### Quadratic Probing
Quadratic probing attempts to handle linear probing’s clustering of collisions  
If ```hash(item) = H``` and the cell at H in the array is occupied:  
Try to place at H+1^2, then H+2^2, then H+3^2 etc.  
Ie: next cell, then 4 cells away, then 9 cells away, then 16 away  
Wrap around to beginning of array as needed. (Mod % these with the array size)  
You aren’t checking every cell, you are checking increasingly large gaps.  
This spreads out the data and increases the odds of finding an empty cell

| Inserting  Index Loc |  0 | 1 |  2 | 3 | 4 | 5 | 6 | 7 |  8 |  9 |
|:--------------------:|:--:|:-:|:--:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|
|        89 / 9        |    |   |    |   |   |   |   |   |    | 89 |
|        18 / 8        |    |   |    |   |   |   |   |   | 18 | 89 |
|        49 / 9        | 49 |   |    |   |   |   |   |   | 18 | 89 |
|        58 / 8        | 49 |   | 58 |   |   |   |   |   | 18 | 89 |
|         9 / 9        | 49 |   | 58 | 9 |   |   |   |   | 18 | 89 |

Finding next is (H+I^2)%size  
H+ai^2+bI+c combines quadratic, linear, constant probing to attempt to even further break apart clustering  
Quadratic probing isn’t always going to guarantee an open spot, it is possible it only returns the same few index values  
Array[16] and hash(item) = 0  
This loops between 0,1,4,9 in different orders between 0 and 0+7^2%16  

To prevent this and prove quadratic probing will always return an empty array  
Table size must be a prime number  
Load factor (λ) must be < 0.5 (half full)  
This guarantees no cell is visited twice, and every cell is visited  
It does have a 2x space overhead as max full the hash table can be is 50%
to resize the table  
Double the current size, then pass into a find next prime number equation  
You cannot just copy items into the new table.  
Items need to be rehashed (ie % new table size) to place items in their new index location  
Avoid copying lazy deletes  

This does NOT eliminate the clustering problem  
But prevents primary clustering - where clustering is close  
This is secondary clustering where clustering is just more spread out  
To get around collisions we create buckets  

### Buckets
Turn each location in the array to a linked list. Then add items to the linked list in the array at the hash index for the item  
Collisions just go into the same “bucket”  
Linklists are easy to just add items  
Contains() you just go through the linked list. If an item is removed you just remove it from the linked list.  
Determine a good max size for the linked lists, and once the buckets get too big grow array and rehash into new linked lists. Not often this is needed.  
Don't combine probes with chaining, as collisions are just fine they go in the bucket  
This also removes the need for a lazy delete and storing data for if an index has been deleted or is active  
Load factor(λ) becomes average of the length of all the lists in the array  
Resize when that becomes “large”  
Analysis needs to be done on the set up to decide what “large” is (probability and requires knowing what the individual hash functions are)  

This is far easier to implement and generally best performing  

Hash tables are fast  
Good hash functions need to be fast, avoid collisions as much as possible  
There is no ordering to the hash table. Finding the “smallest” value means you have to compare every stored value  
In a BST you just go all the way down the left (O(log n))  
This O(N) in a hash table.  
Ordering based stuff should use a tree. Hash tables are great for anything not involving sorted/ordered data
