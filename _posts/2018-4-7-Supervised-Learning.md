Supervised learning is the machine learning task of finding a function that _best_ maps input variables to output variables for a given data set (a.k.a., training set). Because all inputs and outputs are known, the algorithm can iteratively make predictions on the training data and be corrected. The learning process stops when the algorithm achieves an acceptable level of performance. Ultimately, the goal is to produce an inferred function which can be used to reliably predict new values which are not part of the original training set.

Supervised learning problems are categorized into two categories:
1. regression
2. classification 

In a **regression** problem, we are trying to predict results within a _continuous_ output, meaning that we are trying to map input variables to some continuous function. In a **classification** problem, we are instead trying to predict results in a _discrete_ output. In other words, we are trying to map input variables into discrete categories.

There are numerous regression and classification algorithms for different tasks.

## What is dicrete data?

Discrete data can only take particular values. There may potentially be an infinite number of those values, but each is distinct and there's no grey area in between. Discrete data can be numeric (like numbers of apples), but it can also be categorical (like red or blue, or male or female, or good or bad). 

## What is continuous data?

Continuous data is not restricted to defined separate values, but can occupy any value over a continuous range. Between any two continuous data values there may be an infinite number of others.

## What is categorical data?

> "Categorical data are variables that contain label values rather than numeric values [1]." 

For example:
* A "pet" variable with the values: "dog" and "cat".
* A "color" variable with the values: "red", "green", and "blue"
* A "country" variable with values: "France", "Germany", and "Austria"

## Example 1 (supervised learning):

Given data about the size of houses on the real estate market, try to predict their price. Price as a function of size is a continuous output, so this is a regression problem.

We could turn this example into a classification problem by instead making our output about whether the house "sells for more or less than the asking price." Here we are classifying the houses based on price into two discrete categories.

## Example 2

Regression: given a picture of a person, we have to predict their age on the basis of the given picture

Classification: given a patient with a tumor, we have to predict whether the tumor is malignant or benign

### References

[1] https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/
