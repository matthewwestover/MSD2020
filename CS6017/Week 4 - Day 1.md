## Week 4 - Day 1
### Data Structures for KNN
Multi-dimensional nearest neighbors exist.  
We want to search and gather nearest easily and fast.  
KNN needs to calculate each point to gather the K nodes closest.  
We can also do a range query - gather everything within a radius of a point  
Range is typically easier as you know how far you can search. 

#### Binary Search Tree
Each step down we can throw away half of the data. The data is sorted as it is added  
Hash Tables - chaining is better than probing. Linked lists in each “bucket”  allow for quick searching / removal of unneeded data. Jump to the right bucket really quickly, but the bucket is smaller than the whole set of data. 

#### Bucketing with no Tree - Grid
Want to quickly find what bucket a point belongs to  
Divide into a “grid” of locations, and everything in a section is now a bucket.   
Bucket # = location - min X/bucket size  
All you have to store is the lower value of the grid and the bucket size. From that point you can figure out what bucket.   
You do this for each dimension, x,y,z as needed by the data.  
Knowing how to split the data to have good buckets can be hard.  
Simple “buckets” are an array. You have to turn bucket coords into a normal location within an array.  
Break space into fixed size "buckets"  
Within each bucket, we just store an array of points inside that bucket We can quickly figure out which bucket a point is in with:  
`bucketIndex = (position - bucketCorner)/bucketSize` for each dimension  
If we split n-ways along each dimension, we have n^dimension buckets (this is bad for high dimension!)  
To do a range query, we find out which buckets x - radius and x + radius are in, then loop through all the buckets "between" them  
In a single bucket, we just check the distance from x of each point and keep it if it's less than the radius To do a KNN query, we do a range query and increase the radius if we don't find enough points  
Need a way to covert "row 1, column 2" into an index in a 1D array  
Could use a hash function for this. If a lot of buckets are empty, this works very well!  
Could also "flatten" the array  
In 2D you could say index = row*numColumns + column. Can come up with a similar formula for any number of dimensions

#### Tree with no buckets
Finding the top of the tree. The mean doesn’t necessarily translate to an actual point. It also can be unbalanced, as a bunch of smalls and one big put it off balanced.  
Use the MEDIAN point as the root. This will guarantee to be a point, and split the data in half  
However this only works on one axis. A value can be large y but clustered close to the same X value.   
No obvious way to combine multiple trees to catch the different axises  
To build a SINGLE tree, alternate between the different dimensions  
Get median from X values, then next level get median of the Y values. Then the X, then the Y  
Every point is a node  
Height of the tree is log(n)  
Generalization of a BST to many "keys"  
Each node stores the median of it's subtree according to one of the dimensions (x, y, z, etc)  
Each time we go down one level, we move to the next dimension (if I split by x, my children split by y)  
Each node stores a point and the dimension it splits by

#### Combining both Ideas
Make a grid, then divide each grid into another grid  
This is a **Quad Tree**  
Compromise between KD Tree and Grid  
A node is either a "bucket" (array of points)  
or an internal node with 4 children (each of my children covers 25% of my area)  
We pick a "threshold" number of points to decide whether to split a node or make it a bucket  
Each node stores the region it covers (an "axis aligned bounding box") and either a list of points, or pointers to its 4 children