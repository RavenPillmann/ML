# Feature Scaling
- An important preprocessing step for a lot of ML algorithms

## What's the Issue
- The range/magnitude of different features, in particular the imbalance of them, means that different features have more of an impact on calculations.
- For example, say we want to determine a t-shirt size someone should get by height + weight of that person in ft and lbs, respectively. Then, weight will always dominate the calculation because the magnitude of weight is way greater than that of height.

## What is it?
- Feature Scaling is the act of reducing the range of features to being between 0 and 1, which takes care of this imbalance.
- x' = x - x_min / x_max - x_min

## Advantage
- Everything is between 0 and 1

## Disadvantage
- Outliers and noise can ruin this to a certain extent.

## When to use?
- In algorithms in which we mix dimensions to get distance. 
- For e.g., k-means and SVM calculate distance between points that involves more than one dimension
