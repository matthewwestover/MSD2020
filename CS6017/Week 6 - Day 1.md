## Week 6 - Day 1
### Unsupervised Learning - K Means Clustering
#### Supervised Learning
All of the techniques we've discussed so far have been attempting to find some function, y = f(x) that's consistent with the set of x, y data points that we have  
If y was continuous, we called it regression If y was discrete, we called it classification  
These methods are called "supervised learning" methods because we have access to "the right answer" (y) for our training data  
There are many problems where this is not the case  
We have lots of LABELS for the data using real results

#### Unsupervised learning
In "unsupervised learning" we don't have a ground truth "right answer"  
The first unsupervised problem we'll look at is "clustering" which is similar to, but distinct from classification  
Knowing the difference between the 2 is something I expect everyone to master!!
In clustering, we have a set of data points that we want to divide into groups with "similar" points  
Unlike classification, THERE ARE NO LABELS FOR OUR TRAINING DATA! - We dont have real results to say “yes”  
If I am in group “a” it just means in I’m this cluster. Its arbitrary.

#### Clustering
So instead of our data being x, y pairs (x may be multi-dimensional), our data is just a big table of X.  
Thinking in terms of data frames, no columns are "special"  
The main idea in clustering is that points that are close together should be grouped together  
We'll look at a couple different techniques for constructing clusters starting with "K-Means clustering”

#### K-Means Clustering
We pick the number of clusters we want to create(k). Note: choosing a good value of K is hard!  
We choose K points representing the cluster centers (these are usually NOT points from in the data set)  
Each data point is assigned to a cluster by finding the closest out of the K cluster centers  
The locations of the cluster centers are chosen to minimize the "intra-cluster distance." In other words, we want the clusters to be "as compact as possible".  
In general, choosing optimal positions for the cluster centers is intractably expensive to compute, but we can find pretty good solutions pretty efficiently

#### Lloyd's relaxation
This is an algorithm for positioning the K cluster centers that is not guaranteed to be optimal, but generally works well in practice
repeat until "converged":  
for each cluster:  
set it's center to be the average position of all the points in the cluster. 
"recluster" : assign each point to the cluster whose center its closest to
So the cluster centers move slightly in the first step, and the second step changes cluster membership.  
The initial cluster centers are usually assigned with some amount of randomness

What does "converged mean?"  
Pick some number of steps to do (I'll do 100 iterations and use whatever centers I have after that)  
The cluster centers aren't moving very much (sum up the distance they move in an iteration and stop if that sum is "small")  
None (or only a few) of the points switched clusters this iteration How do we pick starting positions?  
Let the user do it (OK for 2D, but not for more than that)  
Randomly either independently or using "blue noise sampling” which

#### Picking K
The **"Elbow Method"**  
We're trying to minimize the "intra-cluster distance," so why not just plot that as a function of k and pick the minimum?  
Unfortunately, adding more clusters always decreases the total intra-cluster distance. In the extreme, every point is in its own cluster and the total ICD is zero...  
Using the "elbow method" we look for an "elbow" in this graph where we hit the point of diminishing returns when adding new clusters. It will still be going down, but at a much lower rate  
No matter how many dimensions our data has, this graph is always 2D (k vs total ICD) which is nice, but there's not always an obvious good elbow point to choose

#### Silhouette Analysis
The "silhouette" is a way of measuring for a point how well it "fits" in its cluster a(p) is the average distance from p to the points in its cluster  
b(p) is the average distance from p to the points in the next-closest cluster `silhouette(p) = s(p) = (b(p) - a(p))/max(b(p), a(p))`  
if a(p) is much less than b(p), this will be approximately 1, and p's cluster is a good fit for it  
if s(p) is close to 0, p is about in the middle of 2 clusters. If s(p) is negative, p is probably mis-clustered  
We can compute the silhouette for each point and can analyze the distribution of values  
We'll look at examples of silhouette graphs and see what they look like for k == too small, k == too big, and k == just right. 
Generally, we want to have very few points with small/negative silhouette values

#### "Distance"
We've kind of assumed that by "distance" we mean euclidean distance (pythagorean theorem distance)  
We could choose to measure distance different ways and will get different results  
This is a lot like the "kernel trick" idea we saw when we discussed SVMs. We tweak how we measure "fundamental" things like distance and can get dramatically different results

#### Evaluation
Did we do a good job?  
We can measure total intra-cluster distance or look at silhouette values, but could we do some sort of "ground truth" evaluation?  
One approach is to perform clustering on labeled data, and see if our clusters match the labels  
This is a bit tricky because the cluster "names" are arbitrary and will change if we perform clustering a second time. There's no connection between the cluster ID and the label  
We can look at how many points in each cluster had the same label ("homogeneity"), or how many points of a given label ended up in the same cluster("completeness")  
Note: this is only used as a way of trying to do evaluation with sort of synthetic data! Clustering(no labels) and classification(we have training labels) are different tasks with different methods!!!