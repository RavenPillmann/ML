# Convolutional Neural Networks
- cNNs can solve lots of problems in a state of the art way

## Applications of CNN
- Audio to text
- NLP (cNNs can be used in NLP, like sentiment analysis)
- Computer Vision (image recognition)
- Training to play a game
- 

## CNN vs. MLPs
- MLPs (Multi layer perceptrons) are often outperformed by CNNs, especially in real world examples where datasets aren't necessarily clean.
- This is in part due to the fact that we need to convert images and other input into vectors for MLPs, but CNNs are built specifically for multi-dimensional input.
- Additionally, CNNs understand that image pixels closer in proximity to each other are more heavily related to each other than pixels far apart
- Similarities include the same loss functions, same optimizers, and the fact that both are built on a series of layers. However CNNs have different kinds of layers than MLPs.

## CNNs vs. MLPs for Image Classification
- MLPs only use fully connected layers, which means that they have lots of parameters and training time suffers as a result.
- MLPs also only accept vectors. This sacrifices information about how close in proximity two pixels might be, which is relevant information for image classification.
- CNNs use sparsely connected layers, where the connections between layers are informed by the 2D image.
- CNNs also accept matrices as input.

### How do CNNs Accomplish these Improvements?
- Instead of connecting each input node with each hidden layer node, we have each hidden node represent one part of the image. Then, we only connect those input nodes in that region of the image to the corresponding hidden node. This is local connectivity.
- Additionally, we want to share weights between these hidden nodes (that is, we want the connections between each region of input and hidden node to have the same weights). This weight sharing technique allows us to avoid having to train the image to recognize a cat in the left and in the right side: instead, we just train it to recognize a cat, period.

### Convolutional Layers
- One type of hidden layer in convolutional neural networks.
- Convolutional layers are made up of convolutional nodes. Each convolutional node is a matrix, where each item in the matrix corresponds to a region of the image.
- The value of each item in a convolutional node's matrix is created by applying a filter to the region of the image that the item corresponds to. Here is an example of this in play: https://cl.ly/1N3P0N3R0V2G
- Basically, we are creating a filter that represents the weights. The weights are common among all regions, so we apply this filter to all regions to get the value of each element of the convolutional node. There's also a bias involved.
- These weights are learned by the training algorithm, just as we learned weights for MLPs.
- These convolutional nodes are referred to as feature or activation maps in practice.

#### Filters
- Filters are designed to recognize a single pattern. This single pattern might correspond to a single thing it detects, like teeth or a tongue. If we want to be able to detect more patterns, we have to add more filters.
- Generally speaking, we'll want one convolutional node per filter. That convolutional node will have a filter with weights completely independent of the other nodes in that convolutional layer.
- Note that the filter is 2D when we have a greyscale image. When we introduce color, our filter will become 3D, with the depth being 3 (one layer for r, g, and b values)
- Then, to get the mapping to a convolutional node, we just sum across all layers of filters and take the relu: https://cl.ly/0r1z1G1U1J26
- When we have multiple filters, we can think of each feature/activation map as a layer in a stacked convolutional layer.
- Based on this stacked convolutional layer, we can pass that into another convolutional layer. This finds patterns in the patterns.

#### Hyperparameters
- The number of filters determines the number of feature maps.
- The size of the filter determines how big of a convolution window to use.
- The stride is the amount by which the filter moves over the image. A stride of one will make the convolutional layer have roughly the same size as the input image.
- The padding refers to the act of putting extra pixels at the edge of your image. At the edges (based on the stride), it's possible for the filter to cover a region that is only partially filled with pixels. When this is the case, we can either ignore the regions that aren't full of pixels (padding='valid' and we lose some information of the image) or we can pad the image with pixels with 0 values (padding='same').

#### Number of Parameters in a Convolutional Layer
- Number of parameters can be calculated as (num of filters)*(height of kernel)*(width of kernel)*(depth of previous layer) + (number of filters). This is because we have a different set of parameters for each filter, and within those filters we have (height*width*depth) weights, plus a bias unit.

#### Shape of a Convolutional Layer
- If padding='same', then height = ceil(float(height_of_previous_layer) / float(stride)) and width = ceil(float(width_of_previous_layer) / float(stride))
- If padding='valid', then height = ceil(float(height_of_previous_layer - kernel_size + 1) / float(stride) and width = ceil(float(width_of_previous_layer - kernel_size + 1) / float(stride))

### Pooling Layers
- Another kind of layer in a convolutional neural network
- When we have lots of feature maps in a convoluational layer, there are more parameters to train, which can lead to overfitting. Pooling Layers are meant to reduce dimensionality and usually take convolutional layers as input.

#### Max Pooling Layer
- The idea of the max pooling layer is to keep the depth of the convolutional layer but reduce the number of nodes in each layer. It does this by defining a window and stride and running over each convolutional feature map. It simply maps the max value in each pooling window to the pooling layer. https://cl.ly/3n3z3x0W2A43

#### Global Average Pooling Layer
- We don't define window size or stride.
- Instead, we map each feature/activation map in the convolutional layer to a feature map in the pooling layer. We simply average all values in the convolutional layer's map to get the value in the pooling layer's map. This leaves the pooling layer as a vector with the depth of the previous convolution layer.

#### More on Pooling Layers
- Can be found here https://keras.io/layers/pooling/

### Arranging CNNs
- Some notes

#### Resizing the input
- We'll need to resize images in order to use a CNN, since all inputs need to be the same. Usually we'll use a square with the height/width equal to a power of 2.

#### Increasing Depth and Decreasing Height/Width
- We want to design our CNN to both increase depth of each layer as we get deeper (convolutional layers with mutliple features will accomplish this) and descrease height/width (pooling layers accomplish this).

#### Hyperparameters
- Kernel: Usually will range from 2x2 to 5x5
- Strides: Often 1, the Keras default
- Padding: 'same', which is not the default in Keras, is usually believed to get generally better results
- (Setting stride to 1 and padding to 'same' results in a convolutional layer with the same width/height as the previous layer)
- The number of filters generally increases as we go deeper, because we want to increase depth
- Activation function is usually ReLU in all of our layers

#### Ordering
- Usually we'll have a max pooling layer after each one or two convolutional layers. The convolutional layers are responsible for increasing the depth of our model as we go deeper.
- Setting pool_size and stride to 2, we can generally cut down the height and width to half of the previous layer, which accomplishes the reduction in spatial dimensions.
- The input layer is meant to represent the spatial information of the image. The array we get to at the end is meant to lose all of the spatial information (because the dimensionality is small) but contain information about what patterns are recognized in the image (because we have a large number of feature maps).
- Once we have an array, we can flatten it into a vector and feed it into one or more fully connected layers to classify the image.

#### More Tips and Tricks
- Always use ReLU activation functions with the exception of the output layer.
- The output layer should be a Dense layer with a softmax activation function and a size equal to the number of classes.
- A cheat sheet on using Keras can be found here: https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Keras_Cheat_Sheet_Python.pdf

#### Invariance
- When identifying things in images, we don't care about some things, like size or angle of the item.
- Scale Invariance: We don't want our item's size to matter for classifying.
- Rotation Invariance: The angle shouldn't matter either
- Translation Invariance: The shift of the item shouldn't matter (whether it's on the left or right)

##### Adding Invariance
- Basically, we can expand the training set by augmenting the data (add random rotations, scaling, and translation of images to the data).
- Data Augmentation also reduces the chance of overfitting because we get more general datasets.

### Transfer Learning
- We can use neural networks we've trained on certain datasets to detect things in other datasets. The way we can do this is called transfer learning.
- Generally, CNNs have filters that go from simple to complex patterns the deeper we go. In truth, only the final convolutional layers have feature maps that detect the things we've trained for detecting. Therefore, we can actually take the first part of the NN (all the layers that come before those layers that can identify the object), slap on new densely conncted layers, and train only the final layers.
- This all depends on the size of the dataset and the similarity of the datasets.

#### Different Approaches to Transfer Learning
- If the new dataset is large and it's similar to the original dataset, we fine tune the model
- If the new dataset is large and it's not similar to the original dataset, fine tune or retrain
- If the new dataset is small and it's similar to the original dataset, change the end of the convolutional network
- If the new dataset is small and it's not similar to the original dataset, change the start of the convolutional network

##### Small Data Set, Similar
- Cut off the end of the neural network (fully connected layer)
- Add a new fully connected layer that matches the number of classes in the new set
- randomize the weights of the new fully connected layer, freeze the weights of the pre-trained network
- train the network to update the fully connected layer's weights

- Small datasets often lead to overfitting, but we avoid this by freezing the pre-trained weights

##### Small Data Set, Not Similar
- Cut off most of the pre-trainer layers near the beginning of the network
- Add a fully connected layer, with classes to match the new data set
- freeze the pre-trained weights, randomize the fully connected layer
- train the weights of the fully connected layer

- Overfitting is a concern with small data sets, but we combat this by freezing the pre-trained weights
- Because the dataset isn't similar, we want to get rid of the convolutional layers that were more detailed, which are the ones that are deeper in the neural network.

##### Large Data Set, Similar
- Slice off the last fully connected layer and replace with a layer that has the number of classes of the new training set
- randomly initialize weights of the new fully connected layer
- initialize the pre-trained portion with the pre-trained weights
- re-train the network

- We can retrain the network because we aren't concerned with overfitting too much (large dataset). Also we use the entire pre-trained neural network because the datasets have similar features.

##### Large Data Set, Not Similar
- Either do the same thing as the Large/Similar approach, or...
- Remove last fully connected layer and replace as one with the same number of classes
- Retrain the entire network with randomly initialized weights

- Using the Large/Similar approach, although the dataset is different, may be faster. If it doesn't seem to produce a successful model, though, randomly initializing the weights might work well instead.

#### More on Transfer Learning
- https://arxiv.org/pdf/1411.1792.pdf
- Cancer Recognition with Deep Neural Networks: https://www.nature.com/articles/nature21056.epdf?referrer_access_token=_snzJ5POVSgpHutcNN4lEtRgN0jAjWel9jnR3ZoTv0NXpMHRAJy8Qn10ys2O4tuP9jVts1q2g1KBbk3Pd3AelZ36FalmvJLxw1ypYW0UxU7iShiMp86DmQ5Sh3wOBhXDm9idRXzicpVoBBhnUsXHzVUdYCPiVV0Slqf-Q25Ntb1SX_HAv3aFVSRgPbogozIHYQE3zSkyIghcAppAjrIkw1HtSwMvZ1PXrt6fVYXt-dvwXKEtdCN8qEHg0vbfl4_m&tracking_referrer=edition.cnn.com

## More on CNNs
- CNNs being used for object localization http://cnnlocalization.csail.mit.edu/Zhou_Learning_Deep_Features_CVPR_2016_paper.pdf
- Stanford's course materials on Convolutional Neural Networks for image recognition: http://cs231n.github.io/convolutional-networks/
-
