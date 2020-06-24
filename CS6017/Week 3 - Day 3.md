## Week 3 - Day 3
### Classification with K Nearest Neighbors
**Limitations of Logistic Regression**  
We saw that Logistic Regression was a tool for performing classification  
But, because it's the result of "taking a line, and smushing it into an S shape" the decision boundary must be a straight line  
It also requires some creativity in order to be used for multi-class classification

**KNN** is a very (conceptually) simple technique for classification (and can also be used for regression)  
You simply ask which training points are nearby the point you want to classify, and have them vote  
The "K" comes from grabbing the K points closest to x which we call x's "K nearest neighbors"  
K is the only* parameter for this method and results are usually not TOO sensitive to the choice of K  
ie, K = 1 and K = 100 may give very different predictions, but K = 10 vs K = 11 usually produce similar decision boundaries

#### Measuring Nearest
There is another "parameter" that can be tuned for KNN which is that you can choose how to measure the distance between points to decide which neighbors are nearest  
We usually use the normal "euclidean" distance (pythagorean theorem version), but you could use the "manhattan distance" (sum of absolute value difference in each dimension)  
For variables that have values 1 or 0, you might want to multiply that coordinate difference by some scale that describes how important that coordinate is. For example, if 1 = went to college, and 0 = didn't got to college, you could multiply (a_college - b_college) by a scale that would be > 1 if you think this is an important variable, and < 1 if it's not that important

#### Data Structures
Being able to perform KNN queries efficiently is important for classification as well as a bunch of other problems (physical simulation, computer graphics, GIS, etc)  
Many of them are ways of generalizing a binary search tree to a situation where you need to sort based on multiple keys simultaneously

