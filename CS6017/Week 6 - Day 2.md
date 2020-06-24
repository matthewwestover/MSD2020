## Week 6 - Day 2
### Hierarchical Clustering
K means works well if the clusters are convex and isotropic (cluster are circular-ish blobs)  
We saw that it doesn't work well for other shapes (smiley face, stretched blobs)  
K-Means also requires us to pick how many clusters we want (K) up front. This is hard to do a good job of in general  
Hierarchical clustering can overcome many of these issues

Hierarchical clustering doesn't produces a set of clusters (... so how is it clustering?)  
We create a tree whose internal nodes represent a cluster created by merging its children  
We can get a set of clusters by picking taking a "slice" of the tree at a given height Depending on the height we pick, we will get different clusterings.  
If we slice toward the leaves, we will get more, smaller clusters, and if we slice near the top we will get fewer, larger clusters. 
Looking at the tree can help us choose an appropriate number of clusters

#### Construction
We could construct it top down (like we did for KD/Decision trees) or bottom up  
We'll focus on bottom-up construction which is called "agglomerative clustering"  
The leaves of our tree are clusters which each contain one data point  
We insert internal nodes which merge their children until we get a single root node

The algorithm is pretty simple  
Make a cluster(node) for each data point while there is more than 1 cluster (node) left:  
Find the 2 "most similar clusters" and insert an internal node with those nodes as children  
The tricky part is how to decided which 2 clusters should be combined?  
For the leaf nodes, we could pick the 2 points which are closest, but how to we generalize that to clusters with many points in them?

#### Choosing what to merge
For points, we can use distance between them to decide which to merge  
For clusters we need to define a "linkage criteria" which lets us compare pairs of clusters Some common choices of "linkage criteria" are:

* "Maximum" or "Complete" : the farthest distance between a point in cluster A and a point in cluster B
* "Minimum" or "Single" : the smallest distance between points in A and B
* "average" : average of the distances from particles in A/B
* "centroid" : The centroid is the average position of a single cluster. So the "centroid" linkage metric between clusters is the distance between their centroids
* "wards criterion" : look at the intra-cluster distances after merging. Basically, try all the possible merges and pick the one which seems best after the fact

#### "Dissimilarity Metric" or "Affinity"
This is jargon that just means "how to distance between points" Usually we use euclidean (pythagorean theorem) distance  
But we could use "manhattan distance" (Travel down the block to turn instead of how direct line)    
Or pairwise correlation which basically means we're treating points like directions rather than positions, and points are "close" if they point in the same direction, regardless of whether they are actually close to each other  
We saw similar ideas for KNN, SVMs, and K-Means clustering. If we change what we mean by "distance" we get different results.

#### Dendrograms
A dendrogram is a visualization of the "cluster tree" we create The leaves are on the bottom and correspond to individual points  
The y- axis measures the linkage criterion Leaf nodes are drawn as a horizontal line with vertical lines to their children. The higher the horizontal line, the more dissimilar the child clusters are  
We can use the dendrogram to decide on an appropriate number of clusters by choosing the height at which to cut (we'll see examples of this)  
**The x axis is arbitrary. The ordering just prevents us from having a bunch of crossing lines. Don't use the x-axis ordering to draw any conclusions!**
