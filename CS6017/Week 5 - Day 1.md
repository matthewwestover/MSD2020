## Week 5 - Day 1
### Decision Tree
Decision trees are very similar to KD trees  
Each node stores a "decision" : a feature (column) and a threshold value (or multiple cutoffs). A decision tree need not be a binary tree (but they often are)  
To classify a point, you traverse the tree from top to bottom and at each node you descend to the appropriate child. Eventually you reach a leaf, which stores stores a class (or a probability distribution between classes)  
Assuming the decision tree is "small" it's very easy for a human to understand exactly why a given decision was made . The structure of a good tree also tells us about what features are important... important features are used in nodes near the top of the tree  
You could imagine your cable company's call center workers using a decision tree to help you troubleshoot your unreliable internet connection (here the "class" is the cause of your internet issues)To predict the class of a data point of interest with a decision, you hierarchically look at one attribute of the point of interest which will lead you down different branches of a tree. When you get to a leaf node, the decision tree tells you what the prediction is.   
For example, if you're using a decision tree to predict whether or not a patient has a given disease, each internal node might ask about a given symptom. If the patient has that symptom you would descend to one child, and if they don't, you'd descend to the other.  
Decision trees are quite simple to understand/explain/use for prediction. We'll mostly talk about using them for classification, but they can also be used for regression.

#### In Detail
Leaf nodes are "prediction nodes"  
They store the prediction for query points that end up in that leaf during traversal  
They generally store the percentage of training points in each class associated with this node  
Internal nodes are "decision nodes"  
They store a feature and threshold(s) to decide which child to descend to  
The leaf nodes represent predictions. These store a predicted class, and possibility the confidence of that prediction, computed as the percentage of training data elements with that class that are associated with that node.  
Internal nodes represent "choices" where we examine a feature of a data point.   For categotical features, we might have a child node for each value of that feature. For quantitative data, we usually have a splitting value with data points with a smaller value in the left subtree, and data points with a greater feature value in the right subtree (just like in a KD tree!).  
For efficiency of creating a decision tree, the trees tend to be binary, but could have any arity.

#### Construction
Often constructed recursively, top down with the "ID3" algorithm  
If all points are the same class (maybe with some tolerance): make a leaf node  If we're out of features we can split on: make a leaf node  
if we're working with 3 binary features, we can only have 8 leaf nodes  
we might also limit the number of internal nodes intentionally(more on this later)  
Otherwise, pick the "best possible split" among the available features, make an internal node, and recurse  
Decision Trees are usually constructed top-down, recursively with a relatively simple algorithm (the ID3 algorithm):

* If all data have the same class, we don't need to make any decision: Make a leaf node and label it with that class.
* If we don't have any more "features" to look at, make a leaf node labeled with the most popular class in the data. We'll usually store the percentage of the data with this class to provide more context for our predictions
* If neither of those cases happens, we should add a new internal node and split the data. Pick the "best possible split" out of all possible splittings for each available attribute. We'll explain what we mean by best in a bit. Recursively build the subtrees of this new node.

We might limit our construction to a given depth, stop splitting when nodes reach a given size.

#### Picking the Best Split
In general, computationally infeasible to build an optimal decision tree, so we use heuristics/local reasoning  
We look at the child nodes that would be created by each potential split, assuming that they would become leaf nodes, not subtrees  
We pick the split that produces the best child nodes according to some measure  
Popular choices:

* sum of squared residuals ( sum (prediction - true value)^2 ) like we used in OLS
* "GINI" a measure from economics that measures how similar all the points are. It is 0 if all the data is the same class, and large if there are points in lots of different classes
* "cross-entropy" a similar measure of "purity". small when the nodes are mostly in the same class

#### Overfitting
Especially if we have continuous features, we can often produce a decision tree that has 100% training set accuracy  
This seems good, but you all should know by now that it's actually bad  
To avoid overfitting, we often intentionally limit the size of the tree  
We can either limit the tree depth (uniformly prevent overfitting)  
Or set a minimum leaf size (allow tall sections as long as there are a lot of training samples in them)  
We can use cross validation to measure how much we are overfitting

#### K-Fold Cross Validation
If we split our data into training/testing we're potentially prevented from using a big chunk of our data for training (typically, but not always, more data means better models)  
But, if we make our testing set too small, we risk overfitting  
With K-fold cross validation, we split the data into k groups. We train k models, each one uses a different 1/k of the data for testing, and the rest for training  
To compute the error, we compute the maximum or average error of all k testing sets  

#### Measurement as Sampling
When we were talking about linear regression and hypothesis tests, we thought about our data as being a random sample from some probability distribution  
In other words, if we repeated our measurements, we'd get different data points from the same distribution  
If we repeated our experiment many times and performed linear regression each time, we'd expect the slopes and intercepts to be normally distributed around the "true" values  
We actually did this in one of our notebooks and saw that this was the case for synthetic data  
Understanding the distribution of parameters like this (slope and intercept, for example) is very powerful, so we'd like to be able to do this sort of analysis  
But imagine you're pollster. Each poll includes 1000 respondents. No one is going to pay you to do 100 polls of 1000 people each to satisfy this curiosity!

#### Bootstrapping
To approximate re-running an experiment we can sample data points with replacement  
So, say you have N measured data points  
`for i = 1 to N: newData.add(data[random(0,N)])`  
This gives you a "new" data set which is from the "same true distribution" as the data you actually measured. Usually about 2/3 of the original data set is in the new data set (1/3 of the new set is repeats)  
This process is easy to repeat and you can use it to get as many data sets as you want (ie, repeat this 10 times, fit a line to each and look at the mean/variance of the line slopes)  
Because these data sets aren't independent, we don't get nice guarantees (the central limit theorem assumes independence), but this is still a useful tool  
A similar technique can be used for auditing elections

####  Bagging
Bagging is an approach for reducing overfitting with decision trees Use bootstrapping to generate a bunch of data sets  
Fit a "tall" decision tree to each one (each tree will overfit its data) To make a prediction, let all the decision trees vote  
Each independent tree will be overfitted (high variance), but by letting them all vote to predict, we hope to smooth (average) out that variance  
This would work better if we actually had several independent data sets to train with. We need more trees to do a good job if we're using bootstrapping

#### Random Forests
Random forests are similar to bagging (which should probably also have forest in its name)  
Instead of using different datasets to create trees, we use the same data, but when we split a node, we restrict the set of features that can be considered
Each time we split, we take a subset of the available features (maybe sqrt(#features) of them), and split based on one of them  
This will result in a "forest" of many trees which can vote in order to make predictions  
Here, the variation in the trees is due the sampling of which feature we can split on, not on sampling data points