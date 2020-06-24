## Week 2 - Day 2
### Linear Regression
#### Senario
Have a data set with 2 attributes (age and number of teeth, 40yd dash times and receiving yards, etc) and many data points. Basically, a data frame with 2 columns and many rows.  
We want to understand the relationship between the attributes, assuming the simplest useful model  
Specifically, we assume there is a linear relationship between the attributes: y = ax + b  
How do we choose a and b to get the “best” linear fit?  
**Linear regression** is the process of computing the "best" values of a and b in the equation above.  
Once we know a and b, we can predict the y values for values of x that we didn't see in the data, or we can summarize the data in a much more compact way.

#### Math
We want to come up with a and b so that for all the (xi, yi) pairs, a*xi + b is as close to yi as possible   
Need to define a way to measure how well we did, and pick a and b that are the best with respect to that measure

##### Take 1
`error_i = y_i - (a*x_i + b )` ... pretty obvious. difference between actual value and predicted value.  
This error is called the “residual” - the difference between our fake line the actual data point  
total error = sum(all the error_i)  
pick a, b to minimize total error  
We can pick a, b to set total error to 0! (this should worry you... how can the total error be 0 if the points aren’t on a line?)  
Pick a = 0, b = mean(y_i’s). When you sum up all the errors, the +s and -s cancel, so the total error is 0.  

##### Take 2
OK, throw in absolute value. `error_i = |y_i - (a*x_i + b)|`, sum them up to get total error  
This is reasonable, but turns out to be very hard to optimize. Imagine graphing error as a function of a. Absolute value adds really nasty “kinks” to the graph where terms hit zero.  
It’s really hard to optimize a function with sharp kinks, so this approach is not preferred.  
This measure of error does have some nice properties and is worth the challenge of optimizing in some applications

##### Least Squares - This is the way
Using absolute value gets rid of +/- cancellation but is hard to work with.  Another way to get rid of - signs is by squaring things  
`error_i = (y_i - (a*x_i + b))^2`  
This is nice and smooth making it easy to work with  
Since we’re minimizing the squared residual, we call this **least squares**  
There is a formula for a and b in terms of the x_i and y_i that’s efficient, so this approach is great  
We’ll see that we can use this approach for more than just fitting lines…  
We want to find the a and b with the "least squares error." If you remember calculus, we actually find this by taking the derivatives with respect to a and b, setting them to 0, and solving for a and b.

###### Final Formulas
```
a = sum( (x_i - mean(x))*(y_i - mean(y)) )/sum( (x_i - mean(x))^2 ) 
b = mean(y) - a*mean(x)
```

One measure is the sum of errors: sum( (y_i - (a*x_i + b)) ^2 ) Called the “residual sum of squares” (RSS).  
OK, but dependent on the scale of Y. RSS would get smaller if you converted temperature measurements from Farenheit to Celsius

##### Normalized Error
Take the RSS and divide by something that describes the scale of Y:
Total sum of squares `(TSS) = sum( (y_i - mean(all ys)) ^2)`. This is the “total variation in y”
Residual Sum of Squares (RSS) = `sum_i y_i - a*x_i - b`

#### Relative Error (R-squared Measure)
We'd like to normalize the error based on the magnitude of the Y's we're dealing with.  
We measure the variation in our data by. 
Total Sum of Squares `(TSS) = sum_i (y_i - mean(y_i))^2` which is the variance of y.
The R-squared measure is `(TSS - RSS)/TSS = 1 - RSS/TSS`. 
This is always between 0 and 1 and describes how much of the variation in y is due to the linear relationship (as opposed to deviations from our line). **It is 1 if all data points are exactly on the line**.  
Typically people look at the R^2 value to decide whether a linear fit is a good model for the data or not.   
The cutoff for “good” depends on the application