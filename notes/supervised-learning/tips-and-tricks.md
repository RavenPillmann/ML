# Tips and Tricks

## Preprocessing the Data

### Skewed Data
- If data for a particular value is skewed/overwhelmingly tends towards a single value, but there are non-trivial higher values out there, then we can apply a log transformation. This will bring values closer together so they will be less skewed, but also not that we should apply a small value to avoid trying to take the log of 0.

### Normalizing Data
- Often it's a good idea to perform some scaling on numerical features. This doesn't change the shape of the distribution but does normalize the range, so that each feature is treated equally. 
- Note that after scaling, the values of the data won't have the same meaning as before. If we scale age down to a range of 0 to 1, for example, 0.5 will mean halfway through the range values, rather than 0.5 years.

### Categorical Variables
- Most learning algorithms will expect numerical rather than categorical variables. Therefore, we should convert categorical variables into dummy variables.
- One-hot encoding basically creates a new dummy variable for each value of categorical variable.

## Model Performance

### Naive Predictor
- Either from previous research results or from a model that doesn't take the data into account (ie randomly classifying or even just choosing the most present class and always predicting it), a naive predictor is a good comparison subject to see if our model does anything. Specficially, we can use it as a benchmark when making our predictions.

## Model Evaluation

### Feature Importance
- We can check a fitted model's feature_importances_ to find the most heavily weighted features.

