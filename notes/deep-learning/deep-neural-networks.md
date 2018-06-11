# Deep Neural Networks
- Where perceptrons and logistic regression can fail when data is nonlinearly separatably, deep NNs can work with nonlinear data.

## Combining Perceptrons
- NNs are also called multi-layered perceptrons. To achieve a nonlinear separation of data, we can take two or more linear functions, then add them together somehow.
- Because adding two probabilities may result in a number greater than 1, and because we want the result to be a probability, we can apply the sigmoid function to the resulting sum.
- Additionally, we can weight each model differently and add a bias before using the sigmoid function. https://cl.ly/2X2f363f2r3z
- NN can have an input layer, many hidden layers, and an output layer
- More input nodes in the input layer means that we're working in higher dimensions (3 input nodes for e.g. means we're working in 3 dimensions)
- More output nodes means that we're dealing with multiple classes
- More hidden nodes and hidden layers lets us make more complex non-linear functions.

### Multiclass Classification
- We can add nodes to the output layer, where each node represents the probability of the input being that class. Then we can use the softmax function to choose which class to use.

## Feedforward
- Layer by layer, we combine the inputs, weights, and biases (Wx + b) and take the sigmoid function of the result to get the value at each subsequent node. 

## Error Function
- We'll need an error function to train NN. Recall that the error function for perceptrons was E(W) = - (1/m) ∑(i=1 to m) y_i * ln(y_i_hat) + (1 - y_i) * ln(1 - y_i_hat). This same error function works on NN.

## Back Propogation
- After we get the error of our neural network, we can adjust weights and bias using back propogation to spread the error to different weights and biases
- Conceptually, we look at the layer before the output layer and decide to give more weight to the node that best gets us to where we want the final input to be. We do this all the way back to the input layer.
- Using gradient descent, we can calculate the gradient and adjust all weights accordingly. Because the error function is a function of y_hat, and because the gradient therefore is the derivative of the error function with respect to y_hat, and because y_hat is a function of the weights, we can think of the gradient as the derivative of the error function with respect to each of the weights.
- Knowing this, using the chain rule, we can calculate the derivative of the E with respect to a weight. This derivative will be used to update the weight, namely that W_ij^k <- W_ij^k - alpha * ∂E/∂(W_ij^k). In a simply NN where W_11^1 -> h1 -> h -> y_hat, this chain rul would look like ∂E/∂(W_11^1) = ∂E/∂y_hat * ∂y_hat/∂h * ∂h/∂h1 * ∂h1/∂W_11^1. 
