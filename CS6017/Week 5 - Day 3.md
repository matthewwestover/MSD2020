## Week 5 - Day 3
### Feature Selection
Imagine we have a data set with many columns (10s, 100s?) Which of those columns should we include in our model?  
For a few of the datasets we looked at, there were lots of features (independent variables, columns) in our data that we could potentially use to predict the dependent variable we're interested in (sale price, admittance to grad school, etc).  
The models we use for regression/classification should be as simple as possible while still being useful predictors, so we usually don't want to include all available features as inputs to our model.  
We saw how to "prune" features from our model when using linear/logistic regression by looking at the R^2 value and p-values of the individual coefficients. For some models, we have good numerical techniques. For least squares regression and logistic regression, we can look at the impact that adding or removing a feature from our model has on the R^2 value. We can also look at the P-Values for the individual terms.  
Decision trees sort of do feature selection automatically when they choose which feature to split on (this is a useful property of decision trees)  
Other models, however don't provide us with such easily interpretable measure of "feature usefulness." For KNN or SVMs we could train models with/without those features and compare their accuracy on test data, but that requires extra model fitting.

Feature Selection is the process of analyzing which features are most likely to be predictive before fitting a model. We'll look at a single technique in more detail.

### The Chi-Squared test
A feature probably should not be included in the model if it is uncorrelated or unrelated to the variable we're trying to predict. The Chi-Squared test is a hypothesis test that computes the probability that two variables are independent.  
Like the other hypothesis tests we've discussed, we compute a test statistic which is assumed to come from a known probability distribution if the variables are independent. In this case rather than a gaussian, our statistic is assumed to come from a "chi-squared" distribution. In this case, our null hypothesis is that the variables are independent.  
For categorical data, the test statistic is computed by comparing the distribution of labels for the data points with a given feature value to the distribution of labels for the whole data set.  
For example, with the Titanic dataset, we would compare mean value of gender for survivors, and for people who didn't survive. Since the gender ratio is very different for survivors and those who died, the statistic we compute would be very high, and its corresponding p-value will be very low (it would be unlikely to compute this statistic if the null hypothesis were true).  
We can reject our null hypothesis and conclude that sex and survival chance are NOT independent.  
SKlearn has a feature_selection module including a chi-squared method which computes p-values for each feature using the chi-squared test.  
The Chi-Squared test is a hypothesis test we can perform to help us determine if a given feature is important  
The main idea is that we want to see if the distribution of values of a given feature is dependent on the class  
We split the data based on its label (for the titanic example, split the data into 2 sets: people who survived, and people who didn't), then look at the compare the distribution of values for a given feature between those groups. 
If the feature is not a good predictor, the 2 distributions will be similar If it IS a good predictor, then the distributions will be very different  
Our null hypothesis is that the distribution of values of the feature is the same for each class (ie, survivors have the same ratio of men/women as perishers)  
We compute a test statistics which is expected to come from the chi-squared distribution (hence the name), from which we can compute a p-value  
If the p-value is very small, then we reject the null hypothesis and conclude that the feature has DIFFERENT distributions for the different classes, and so this feature is likely to be useful for prediction  
Based on the p-value we can decided whether or not we should include this feature in our model