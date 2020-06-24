## Week 3 - Day 2
### Classification with Logistic Regression
We've seen linear (and general least squares) regression  
Given continuous independent variables, we want to predict a continuous output variable What if the variable we want to predict is NOT continuous?  

* Will a student who has a 2.8 GPA and studied for 3 hours pass the test?
* Is the tumor cancerous or benign?
* Is the pixel background or foreground?

"Regression" with categorical values is called "Classification"
The input is labeled data (a data frame where one of the columns is the "class"), and we want to produce a model that can predict the class based on other measurements

We'll start by assuming 2-class classification (yes/no or true/false)  
We could just use regression to fit a continuous output value and apply a "cutoff" to predict the class. This doesn't work well...  
What does the continuous value that we fit "mean?" We'd like it to be "probability of yes" but linear regression will give us values from -Infinity to Infinity

We basically fit a linear model, and then smush it by running the output through the "logistic function" aka the sigmoid function  
You've probably seen the logistic function in the news recently (flatten the curve!)  
`logistic(x) = (e^x)/(1 + e^x)`

We fit a line or plane, and then compute  `probability(y(x) is true) = logistic(line(x))`  
The logistic(0) = 0.5, so the place where the line crosses the x axis we have 50% chance of being true/false.  
logistic(x > 0) approaches 1, so the higher our line is above 0, the closer we are to probability 1 of x being "true"  
logistic(x < 0) approaches 0, so the lower our line goes, the closer we are to probability 1 of x being "false"  
The parameters we fit are still the slope + intercept of a line, but the logistic function turns the output of the line into something that can be understood as a probability

The larger the magnitude of "a" (the steeper our line is) the faster logistic(line(x)) will transition from 0 to 1  
A large value of "a" means there is a very narrow "uncertainty region"  
A small value of "a" means there is a big range where there's a reasonably large probability of values being true or false. 
the combination of a and b control where the probability is 50/50

Because it's based on fitting a line( or plane or hyperplane in higher dimension), logistic regression produces a straight "decision boundary". 
In other words, the place where there's a 50/50 chance of being in either class is a straight line  
If the true decision boundary is curved, logistic regression will not work well!  
Logistic regression is designed for 2-class classification. You have to get creative to use it for multi-class classification