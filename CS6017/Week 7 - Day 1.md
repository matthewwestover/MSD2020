## Week 7 - Day 1
### Midterm Review

#### Basic Statistics
* Common descriptive stats and their meanings: 
    * Standard Deviation
    * Mean
    * Variance
    * Median
    * Quartiles
    * Correlation 
* Working with probability distributions: 
    * sampling
    * pmf - prob mass(Bernoulli, discrete outcomes - like heads/tails)
    * pdf - prob density(continuous analog, relative probability. Look at area under curve to get likelihood)
    * cdf - cumulative density (getting less than or equal to)
* Common probability distributions (discrete and continuous)
    * Bernoulli - discrete, coin flip - specify prob for heads
    * Binomial - add up of Bernoulli, eg 10 flips, how many heads
    * Poisson - homeruns example
    * Normal Distribution - continuous 
    * Students T distributions - fat tails

#### Hypothesis testing
* What are we trying to show when we perform a hypothesis test?
    * Show our understanding of probability is correct - is it likely to get this response for this issue
    * Never confirm, only get “yes its reasonable” or reject
    * Significance level - threshold of the P-value for if we can reject the null hypothesis
        * Ex if less than 1% chance to see this p-value we reject
* What is a P-value and what is its significance?
    * The probability of obtaining results 
* What is the "null hypothesis?"
    * An assumption of something normal. If our sample returns as “unlikely” we can reject it

#### Visualization
* What are the basic types of plots we work with?
* What is a mark?
    * Representation of a data point.
    * Dots, lines, etc.
* What is a channel? Which channels are the most effective at conveying information to humans?
    * Colors/shapes/sizes that differentiate the marks. Contrast similar data on the same plot
    * Good: Position is best for humans, size can be okay (length if lined up), color works for categorical

#### Linear regression
* How does linear regression measure the quality of a "line"?
    * Distance of points to the line - least squares. 
    * We get the line by the coefficients (betas) for the line. (Intercept and slope, etc.)
* What are the important statistics that can help us understand the quality of our regression?
    * Residual is the value of distance between dots and line
    * R-squared is the normalized average of that distance between 0-1
        * If 0 beta likely has no relation to data changing
* What kinds of models can we fit with ordinary least squares?

#### Getting data
* What's the difference between web scraping/using an API?

#### Classification
* What is logistic regression? How is it related to/different from linear regression?
    * Classification between categorical data. (Is it yes or no, heads/tails)
    * Min max are 0 and 1 which translates to probability between the two (close to 0 has low prob, or close to 1 has high prob)
* What is KNN classification? - how many near by neighbors. 
* What are the common spatial partitioning data structures used to implement KNN efficiently?
    * Grid and Trees 
* What are the limitations of logistic regression?
* What are the available knobs to tune with KNN, and what effect to they have on the trained classifier?
    * How big K is. the bigger K is the more underfit it is. 
    * Ask everyone you are just seeing majority of the whole data set. 
    * K is small that is overfit, outliers get incorrectly classified
* What is a decision tree? What are their pros/cons?
    * Some feature we split on 
    * Repeat until we get to leaf node - no more splits. See what probability is on leaf nodes of being one or the other
    * Good for understanding at seeing what attributes are important (biggest split - ex. titanic was male/female for survival)
    * Bad 
* What is bootstrapping?
    * Generate a bunch of new data sets using subsets of the real data, so they would be similar but not the same. 
* What is bagging?
* What are random forests?
* What is cross validation and why is it useful/important?
* What are SVMs and what knobs do we have?
    * Adjust thickness of the “margin” line
    * Thin line = overfitting. 

#### Clustering
* What's the difference between clustering/classification?
    * Classification has actual classes to be placed into - supervised learning (there is a right answer)
    * Clustering is that data points happen to be close to each other
* How does K-means clustering/Lloyd's algorithm work?
    * Number of clusters we try to find. Get the center of each cluster and any new data would be clustered based on how close it is to that center point
    * Lloyd’s relaxation algorithm would pick random centers - calculate difference in points from other groups, then shift center and calculate again. Repeat until centers stop moving.
* What is hierarchical clustering?
    * It uses height of splits to indicate good splits into leaf nodes. 
* How does the "bottom up" (agglomerative approach) construct a hierarchical clustering?
* What is a dendrogram and how do you read one?
* What types of distance (dissimilarity) measures might we use for clustering?
* What is a linkage criterion? What are some examples of popular linkage criteria?

#### NLP
* What is the "bag of words" model and how can we use it for tasks like "sentiment analysis?"
