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

## Training the Model
- Many things can go wrong with training the model. Here are some things about training the model that we can adjust and their effects.

### Early Stopping
- A low number of epochs will mean that we underfit. However, a large number of epochs will overfit.
- We can use a model complexity graph to see what number of epochs is just right
- Early Stopping is an algorithm for finding this point, in which we do gradient descent until our testing error stops decreasing and starts decreasing. At that point we stop running gradient descent.

### Regularization
- It turns out that two models with weights and biases that are scalar multiples of each other will provide different errors for classifying the same points (although they are the same lines, the errors are smaller when we have a larger scaler multiple of a boundary). This doesn't mean that larger scalar multiples of boundaries are better, but it does mean that they are too certain.
- The takeaway here is that large coefficients can lead to overfitting, so we want to avoid them.
- Regularization is the process of taking our error function (the cross entropy) and adding a term that's large when our coefficients are large, thereby punishing large coefficients. We can do this by either summing the absolute values of the weights or the squares of the weights, and multiplying it by some lambda coefficient that allows us to control how much we want to penalize large weights. 
- L1 Regularization is for the absolute values, L2 for the squares.
- L1 leaves us with sparse errors, which means that small weights tend to zero, which means that L1 is good for feature selection (helping us choose the important features).
- L2 gives us better results for training models, generally.

### Dropout
- In NN, sometimes one part of the network has large weights and ends up dominating the training process. Another process might have a small weight doess't get trained in turn.
- In training, we may randomly turn off some of the nodes during any given epoch, forcing other nodes to take over.
- We do this by giving the model a parameter the represents the probability that any given node will be shut off during an epoch. For e.g, if the param is 0.3, each node has a 30% chance of being turned off during that epoch.
- This method is called dropout.

### Getting Stuck in a Local Minimum
- We may get stuck in a local minima. 
- Random Restarts is a good way to deal with this.
- Basically, we run gradient descent a number of times with different starting points. This increases the chance that we'll get to the global minimum, or at least a really good local minimum.

#### Momentum
- Another way to deal with this issue is to try and have enough momentum to get through local valleys.
- The idea is to look at the last few steps and combine their gradients somehow, and use that.
- Using the momentum (ß) between 0 and 1, we take the previous step plus the one before * ß + the one before that by ß^2, and so on. The steps a long time ago are less than important than the previous ones. This generally seems to get us to the global minimum, and although we may overshoot it, we tend to get there.

### Vanishing Gradient
- When we move towards sigmoid function values of 0 or 1, the function starts plateauing and the gradient becomes close to 0. In this case, gradient descent takes very small steps.
- To avoid this, we can try different activation functions, like the hyperbolic tangent and ReLU.

### Batch vs. Stochastic Gradient Descent
- For each epoch in gradient descent, we run the entire dataset through feedforward in gradient descent. We get our predictions and errors and backpropogate. This is known as batch gradient descent, and can take a lot of time and computation.
- Stochastic gradient descent is the process of taking small subsets of data, running an epoch of gradient descent, then continuing. We'll do this by splitting the data into several batches, then going through the batched one by one for each epoch. This is just an estimate of the best steps, but in practice is faster and sometimes best in practice.

### Learning rate
- Small learning rates can take a long time to reach the minimum.
- Large learning rates can bounce around, and might even miss the minimum.
- In general, the best learning rates are the ones where the steps get smaller as we arrive at a solution. We can also plot out how the error function changes against epochs: if it's a sharp decrease, it's good. If it's a long decrease or fails to converge, it's probably not the best.

### Keras Optimizers
- In Keras, we can use a few different optimizers, including:
- 1) SGD (Stochastic Gradient Descent) in which we can define the learning rate, the momentum, and the Nesterov momentum (which slows down the gradient when close to the solution)
- 2) Adam (adaptive movement estimation), a more complicated exponetial decay.
- 3) RMSProp (Root Mean Squared Error), which decreases the learning rate by dividing it by an exponentially decaying average of squared gradients.
- More information can be found here: https://keras.io/optimizers/ and http://ruder.io/optimizing-gradient-descent/index.html#rmsprop

## NN Regression
- We can actually use NN to do regression of nonlinear data. Basically, we can great a piecewise linear function to fit the data. We do this by making a NN with ReLU functions as the activation functions, which basically means that they just return the max between Wx+b and 0 of the previous values. For the output, we don't apply an activation function.
- Our error function is (y - y_hat)^2.
- We can really do this with any activation functions (sigmoid, for instance). We just need to not use a final activation function and instead accept the final answer, and that's it.

## Visual NN Explanation
- A visual guide to NNs http://jalammar.github.io/visual-interactive-guide-basics-neural-networks/

## Activation Functions
- More about different activation functions can be found here: http://cs231n.github.io/neural-networks-1/#actfun

### Sigmoid Function
- A very common activation function that maps any real number to the range of 0 to 1. This can be viewed as a function that maps a score = Wx + b to a probability.
- However, the sigmoid function has recently fallen out of favor for some potential drawbacks:
- When the output of the function is near 0 or 1, the gradient becomes nearly zero, which means that the model doesn't adjust much if at all during back propogation
- Also, if the weights are too large, the function becomes saturated, and the model barely learns anything
- Sigmoid outputs are non-zero, and therefore it's kind of weird to use them in hidden layers.

### Tanh (Hyperbolic Tangent Function)
- Maps a real number to a range of -1 to 1. Although Tanh saturates as well, it is zero-centered, so it's almost ALWAYS PREFERRED TO SIGMOID in practice. Note that tanh(x) = 2*sigma(2*x) - 1

### ReLU (Rectified Linear Unit Function)
- Returns the max between 0 and any real number x.
- It has been found to greatly accelerate the convergence in stochastic gradient descent, probably due to it's non-saturation
- The simplicity of the function makes it quick to compute (no exponents or tangents) when compared to sigmoid and tanh
- With a high learning rate, sometimes its possible for a large gradient to flow though a relu and kill the neuron by causing the gradient to be 0. Sometimes up to 40% of neurons could die during the training process. Monitoring and adjusting the learning rate can help.


