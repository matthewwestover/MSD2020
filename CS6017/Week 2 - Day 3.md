## Week 2 - Day 3
### Multilinear Regression
We saw that we can use least squares to fit functions more complex than a\*x + b... what are the rules?  
We use OLS to fit any function as long as the unknowns are “linear coefficients”  
`F(x, y, z, ...) = beta_0 + beta_1\*(whatever, as long as there’s no betas in here) + beta_2\*(anything else except for betas) + beta_3*(no betas here) + ...`  
The parenthesized expressions can depend on x, y, z, combinations of x, y, z, functions of x, y, z. Whatever you want, as long as there are no betas in there

#### Examples

```
f(x) = beta_0 + beta_1*x # OK
f(x,y) = beta_0 + beta_1*x + beta_2*y# OK
f(x, y) = beta_0 + beta_1*x + beta_2*y + beta_3*x*y # OK
f(x, y) = beta_0 + beta_1*x + beta_2*beta_1*y # NOPE
f(x) = beta_0 + beta_1*sqrt(x) # SURE
f(x) = beta_0 + beta_1*sqrt(beta_2* x) # NOPE
```

1 beta per term, in front: **OK**  
Multiple betas, or beta inside stuff: **Not OK**

### Nonlinear Least Squares
If you need to combine betas, or put betas inside functions like sqrt() or sine(), you need to use a “nonlinear least squares solver”  
They are generally slower and less user friendly than linear/ordinary least squares solvers, but they do exist if you need one  
We’ll see a special case of this when we talk about classification

### Hypothesis tests
For basic linear regression, our null hypothesis was “the beta we computed comes from a distribution with mean 0”  
The p value told us the probability of getting our beta if H0 was true  
A small p value meant that our coefficient was likely NOT 0  
For multilinear regression, we can compute an “F Score” corresponding to the null hypothesis that ALL the betas are 0. If our F score is very big, its corresponding p-value is very small, and at least one of our betas is really nonzero  
We can also compute p-values for each coefficient by fixing the rest of the coefficients and assuming that just this one is zero.  
We’ll gloss over the details, but the practical concern is that a large P-value means a coefficient/ term probably isn’t useful for your model and shouldn’t be included

### Overfitting
We can fit all sorts of models using least squares  
If you remember calculus, you know that any function can be written as  
`f(x) = beta_0 + beta_1*x + beta_2*x^2 + beta_3*x^3`  
This means that we can actually fit **ANY** function with least squares if we include enough terms!  
This is not the best idea. Because we CAN doesn’t mean we should  
The more terms we add (especially polynomials like x^5), the more “wiggly” our fit will be  
This means it will be closer to our data, which seems good...  
But our model won’t generalize well to unseen data, making it useless for prediction  
The goal of regression is not to come up with a function that goes through every data point! It’s to fit a model that can describe our dataset or help us predict y = f(x) for some new x!  
As we add more terms, we are more likely to “overfit” the noise in our data, and create useless models

### Cross Validation
Looking at p-values our terms is one way to understand if we’re overfitting data  
“Cross Validation” is another approach  
In the simplest form, we split our data into 2 groups: training, and testing  
We fit a model to the training dataset  
We compute the error of that model on the testing data set  
WE DO NOT USE TESTING DATA FOR MODEL FITTING!  

**Underfitting**: accuracy for both the training and testing datasets is low. This means our model is not complicated enough to describe the data. We should add more terms  
**Overfitting**: training set accuracy is very high, but testing set accuracy is low. We learned all the wiggles in our training dataset. Our predictions on the testing data are way off because our model learned the noise, not a model of the actual data  
**Just right**: training and testing accuracy are about the same. Our model describes the underlying data well and the same amount of “noise” is measured in both the training and testing data sets

The bias of the method is the error caused by the simplifying assumptions built into the method.  
The variance of the method is how much the model will change based on the sampled data.  
The irreducible error is error in the data itself, so no model can capture this error.

