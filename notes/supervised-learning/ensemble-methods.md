# Ensemble Methods
- Bagging: Get each model to predict a value, then combine them in some way (averaging values or choosing the value that comes up most).
- Boosting: Choosing model that can work best on each individual task.
- Weak Learners: Models that are joined into a stronger model.
- Strong Learner: The combination of the weak learner models.
- In general, we just need these models to do slightly better than random chance at their task.

## Bagging
- We can take subsets of data and train a model on each subset (don't have to be mutually separated, just randomly choose). Then we can take all models into consideration in some way.

## Adaboosting
- A popular boosting algorithm. 
- The general idea is that we train a weak learner by minimizing errors. Then, we weigh all points we misclassified heavier, and retrain the model (ie, we punish the model more for the misclassified points). We do this until we have a bunch of weak learners, then we combine in some way (like voting, etc).

### Weighting the Data
- We can start by having each point associated with a weight of one. Let the error function be Error = Sum of weights of misclassified points.
- After we train the first weak learner, we want to increase the weights. We see that there is a sum of A for correct points and B for incorrect points, so we multiply the weights of the incorrect points by A/B so that the sums of correclt and incorrectly classified points are the same. Then, we train another weak learner, and so on.

### How to Combine the Weak Learners
- We can weigh each weak learner by how useful it is. For completely accurate models, we can assign a large positive weight. For completel inaccurate models, a large negative weight (so that we take whatever the model predicts and do the opposite). For a so-so model, we want a weight near 0, because it isn't really useful. 
- Specifically, we can assign weights by y = ln (x / (1 - x)), where x is the accuracy of the model. (accuracy = TP + TN / All Points)

#### Voting
- Now, to combine, we take the weight of each model into consideration. Specifically, we can sum all the model regions up and distinguish classes by whether the sum is positive or negative. https://cl.ly/3L422r3D1T1C

### Resources for Adaboosting:
- https://cseweb.ucsd.edu/~yfreund/papers/IntroToBoosting.pdf
- https://people.cs.pitt.edu/~milos/courses/cs2750/Readings/boosting.pdf
- http://rob.schapire.net/papers/explaining-adaboost.pdf


