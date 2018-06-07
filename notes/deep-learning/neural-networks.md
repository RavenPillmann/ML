# Neural Networks
Vaguely operate how the brain operates, NN are used in all kinds of applications.
 
## Perceptrons (revisit)
- Perceptrons can take linear equations and put them into a node-based model. https://cl.ly/3K01130p2p20

### Training a Perceptron
- We want the decision boundary in perceptrons to train itself. We can accomplish this with a trick where we compare the misclassified points to the weights of the line and add or subtract accordingly https://cl.ly/1u071F2X3R3w
- We do this trick for all misclassified points. Note that we add the point coordinates * learning rate to the weights and bias if the prediction was 0 (ie, false negative) and subtract the point coordinates * learning rate if the prediction is 1 (false positive). https://cl.ly/1u3l0t1K252C

### Preceptron Problems
- Perceptrons can handle linear divisions well, but they can't handle non-linear boundaries, so we need something else to deal with non-linear divisions.

## Error Function and Gradient Descent
- We want to take advantage of an error fuction to do gradient descent.
- For GD, we need the error function to be continuous.

### Error Function
- We want an error function that's continuous rather than discrete
- For that, we need the predictions to be continuous rather than discrete as well
- In order to change from discrete to continuous predictions, we can use the sigmoid function rather than the step function

#### Sigmoid Function
- Sigmoid(x) = 1 / (1 + e ^ -x)
- We can think of x as being some distance from the decision boundary. In that case:
- Sigmoid(-infinity) -> 0
- Sigmoid(infinity) -> 1
- Sigmoid(0) = 0.5
- We can think of these values (0, 1, 0.5) as the probabilities that the point belongs to the class 1.

#### How to get to sigmoid
- We can plug Wx + b into the sigmoid to get our prediction y_hat. Ie: https://cl.ly/1L1b3G3g1r0M
- Basically, if we think of the perceptron model we had, we can simply replace the step function with the sigmoid function: https://cl.ly/1C11001g0t3d

## Multiple Classes
- For multiple classes, we use the softmax function rather than the sigmoid function
- The softmax function is defined as e^z_i / (e^z_1 + ... + e^z_n) = P(class i), where z_j is the linear function score for the jth class.

## One-Hot Encoding
- One-Hot Encoding is a technique to go from categorical variables to numerical variables. Specifically, for each category, we create a new variable (which will have its own weight) and assign it a 1 if the inputer category is that category and 0 otherwise. Aka, we make dummy variables.

## Maximum Likelihood
- When we are picking a model, we pick the one that gives the highest probability to the events that occured (or the existing labels)
- These models us the sigmoid/softmax functions to assign higher probabilities based on where points are and on which side of the decision boundary they lie. Therefore, if we pick the model that will have the highest probability (and assuming all the points are iid, we calculate the probability by multiplying each points' probability together (which again is determined by the sigmoid function)), the model with the highest probability of giving the events/labels that occured will be the one that most correctly separates the data. https://cl.ly/2b0Z3c2y1o2r
- In practice, because this involves multiplying a lot of probabilities together, we want to avoid using products. This means we want to use logarithms so that we can add the logs of the probabilities together, rather than multiplying probabilities together. 
- Because logarithms of less than 1 are negative, we'll take the negative log and sum those, ie -log(ab) = -log(a) - log(b)

## Cross Entropy
- A very important concept, the cross entropy is the sum of the negatives of the probabilities.
- We want a low cross entropy. A good model will have high probabilities, and the log of a high probability is a small negative number. Because we sum these logs and take the negative of the sum, we expect a small number for a good model.
- Furthermore, we can think of the -log(P) of a point in the model as the error of that point. Points that are misclassified will have small probabilities which means their negative log will be large.
- Therefore, our goal in finding the model is to minimize the cross entropy. Cross Entropy is an error function of this model.
- CE = - ∑(1, M) y_i * log(p_i) + (1 - y_i) * log(1 - p_i) where y_i is 1 or 0 based on whether something is classified as 1 or 0 and p_i is the probability that the i^th case is 1. This is for a binary class problem.
- REMEMBER: the probabilities here are the probability that each point is it's true color given how the model splits the data.

## Multi class Cross Entropy
-https://cl.ly/1z0g333o1p1B

## Logistic Regression
- The Error function is the CE divided by the number of data points, whether binary or multiclass.
- Probabilities in the error function are given by the sigmoid function, which takes as input Wx + b.
- We'll use gradient descent to minimize this function, which will adjust W and b into W' and b' 

## Gradient Descent
- We do a bunch of steps in which we find out gradient and move in that direction. https://cl.ly/0Q3Z2r0k0d2t
- The gradient is simply the vector sum of the derivatives of the error function in the negative direction.
- There's some math involved, but basically we can adjust each weight w' <- w - alpha/m * (y - y_hat) * x_i and b' <- b - alpha/m * (y - y_hat) where y is the label and y_hat is the prediction. So basically we adjust each weight (and bias) by a scalar value of the x coordinate attached to that weight. That scalar is dictated by a learning step and by the different between the label and the prediction. This sounds similar to the perceptron algorithm.

### Psuedo Algorithm
- Start with random weights W and b.
- For each point x_1 through x_n, update the weights and b according the the formula up top
- Do this until the error is small
