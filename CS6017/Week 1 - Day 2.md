## Week 1 - Day 2
### Introduction to Descriptive Statistics
**Variable types**

* Discrete variables: values are discrete (e.g., year born, T/F)
* Continuous variables: values are real numbers (e.g., length)

**Levels of measurement**

* Categorical: Unordered variables
* Ordinal: There is an ordering but no implication of equal distance between the different points of the scale.
* Interval: There are equal differences between successive points on the scale but the position of zero is arbitrary.
* Ratio: The relative magnitudes of scores and the differences between them matter. The position of zero is fixed.

**Categorical variables - Unordered variables**  
Typically discrete values  
Examples: 

* Survey responses: sex (M/F), true or false (T/F), yes or no (Y/N)
* Size: S/M/L/XL
* Color: red, blue, green, etc.

**Ordinal Variables**  
There is an ordering but no implication of equal distance between the different points of the scale.  
Examples:

* Educational level (high school, some college, degree, graduate…)
* On Likert scale of 1 to 5, how satisfied are you with your instructor?
* Social class (lower, middle, upper)

**Interval Variables**
There are equal differences between successive points on the scale but the position of zero is arbitrary.  
Examples:

* Measurement of temperature using the Celsius or Fahrenheit scales.
* Longitude

**Ratio Variables**
The relative magnitudes of scores and the differences between them matter. The position of zero is fixed.
Examples:

* Absolute measure of temperature (Kelvin scale)
* Age
* Weight
* Length

**Other Examples**:

1. Latitude - Ratio and Continuous 
2. Olympic 50 meter race times - Ratio and Continuous
3. Olympic floor gymnastics score - Ordinal/Ratio and Discrete
4. College major - Categorical and Discrete
5. Amazon rating for a product - Ordinal and Discrete for individual review, Continuous for overall

**Descriptive Statistics**  
Mean, Min, Max, Average explanation for the data set

In Python can be done via

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

%matplotlib inline
plt.rcParams['figure.figsize'] = (10, 6)
plt.style.use('ggplot’)

# compute the min and max
np.min(Alta_avg_month_snow),np.max(Alta_avg_month_snow)

# what month do these correspond to? 
imin = np.argmin(Alta_avg_month_snow)
imax = np.argmax(Alta_avg_month_snow)
months = ['Oct','Nov','Dec','Jan','Feb','March','Apr']
months[imin], months[imax]

# compute the mean
np.mean(Alta_avg_month_snow)

# compute the median
np.median(Alta_avg_month_snow)
```

Pandas is a library to read .csv files

```
# use pandas to import a table of data from a websitedata = pd.read_csv("http://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data", sep=",", names=("age", "type_employer", "fnlwgt", "education", "education_num", "marital", "occupation", "relationship", "race","sex","capital_gain", "capital_loss", "hr_per_week","country", "income”))

# export a list containing ages of people in 1994 Census
ages = data["age"].tolist()

# this allows easy usage
print(len(ages))
print(np.min(ages))
print(np.max(ages))
print(np.mean(ages))
print(np.median(ages))

#can still used as len(data[‘age’]) instead
```

Quantiles - Describe what percentage of the observations in a sample have smaller value

```
np.percentile(ages,25), np.percentile(ages,75)
(28.0, 48.0) 
// 25% of the people are 28 and younger, 75% are 48 and younger. 
// This means 50% of the data is within 28-48
```

Variance and standard deviation quantify the amount of variation or dispersion of a set of data values.

```
print(np.var(ages))
print(np.std(ages))
```

Small standard deviation - data is more clumped close to the mean  
Large standard deviation - data is more spread out from the mean  

Covariance and correlation measure of how much two variables change together.  
Confounding - a undisplayed causation that causes a correlation between two sets of data

Descriptive statistics quantitatively describe or summarize features of a dataset.  
Inferential statistics attempts to learn about the population from which the data was sampled.  
Often, we will model the population as a probability distribution.  
Inferential statistics is deducing properties of an underlying probability distribution from sampled data.

A Random variable is a variable whose value is one of the possible outcomes of a random event. For example, a random variable representing a die roll has possible values 1-6. A random variable has an associated "probability distribution" which describes the probability of the variable taking on a particular value. For the die, the probability distribution is basically: [1 : 1/6, 2: 1/6, 3: 1/6, ... ] meaning each outcome is equally likely.

Random variables can be either discrete or continuous. A die is an example of a discrete random variable. A continuous random variable could represent the angle of a spinner on a board game (assuming we care about the angle it lands on rather than whether it lands on "right foot green" or "left hand blue"). Discrete random variables are a bit more intuitive to think about since we can enumerate all the possibiliities, but the math for analyzing them can be a bit more cumbersome. Continuous RVs are a little bit weird to think about, but we can manipulate them quite nicely with fun, basic calculus!
