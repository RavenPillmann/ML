# Deep Reinforcement Learning

## RL Recap
- Model-Based Learning require a model, like dynamic programming methods such as value and policy iteration, require a known transition and reward model (we have to know the one-step dynamics)
- Model-Free Learning, like Monte Carlo and Temporal Difference Methods, don't require a model and instead explore

## Deep RL
- Relatively recent movement to use NN/MLPs to solve reinforcement learning problems.
- Traditional algorithms for RL use a finite structure for states and actions that can be taken. However, this isn't helpful when there are infinite different states and actions, and that's where Deep RL plays a role.

## Discrete vs. Continuous Spaces
- When states or actions are drawn from a finite set of states and actions, we call them discrete state and action spaces
- Discrete spaces allow us to use dictionaries or lookup tables to store values
- Discrete spaces are also critical to certain RL algorithms (like value iteration, for e.g.) where we need to update values and thus need a discrete set of values to update.
- Continuous spaces, by contrast, aren't characterized by a finite set.
- Generally, tasks that take places in physical spaces are continuous by nature.

### Dealing with Continuous Spaces
- There are a few ways we can modify our algorithm/representations to deal with continuous spaces: Discretization and Function Approximation

#### Discretization
- Converting a continuous space to a discrete space
- For e.g., if we are training a self driving car and looking at the coordinates as a state space, we can discretize it by splitting the environment into a grid. Then, we just find the coordinate on the grid the car is closest to and say it's at that space
- Discretization can apply to action spaces as well
- Discretization allows us to use existing discrete space RL algorithms, which is nice
- One problem here is that we might discretize to a point that we lose an ability to solve a problem. If the grid squares are too large, for e.g., and we block off the squares that have even a small obstacle in the way, then we might mislead the agent to thinking it can't navigate to wherever it needs to be.
- Non-uniform discretization is discretization where the partitioning is non-uniform

#### Tile-Coding
- With prior knowledge of the state space we can easily discretize spaces
- Tile-coding is a way to deal with arbitrary spaces. Basically we overlay a few offset tiling and can represent each tile as a bit in a bit vector. We can keep track of the weights of the tiles and iteratively adjust the weights. That is to say, the state value function is defined by a combination of bit vectors and the weights for each bit. 
- Adaptive Tile Coding is when we start with a coarse tiling and split tiles into two every time we find that we aren't getting much information from the current tiling
- When we need to split, we can decide which tile to split by looking at the difference in weights of the tile before and after split. Also, we can stop after a certain set of splits.
- In the end this maps continuous values to bit vectors of the tiles the point lies in.

#### Coarse Coding
- Like Tile Coding, but uses a sparser set of features to encode.
- For e.g., we can apply circles, spheres, and hyperspheres to the continuous state space. Then we can create bit vectors to represent each circle the state belongs to.
- Using smaller circles means there's less generalization across the space, although it may take longer to learn
- Larger circles relate to more generalization and a smoother value function
- We can also get more resolution across specific dimensions by extending circles in those directions (elongation)

#### Function Approximation
- discretization can get complicated with higher dimensions, and the smoothness of the value function can get lost with discretization as well. Sometimes we would rather use Function Approximation for dealing with continuous spaces.
- Our goal is to approximate the state and action value functions
- We can basically think of this as adding a weight vector w as a parameter to our approximations for v and q. This w should allow us to go from state to v(s, w), s and a to q(s, a, w), and s to q(s, a_1, w), ..., q(s, a_i, w)

##### Mapping s to v(s,w)
- Make sure we have x(s), a feature vector representing the state.
- Then we dot x(s) with w and boom
- We can tune w with gradient descent, for e.g.
- We can do the same for action value functions if we have an x that's a feature vector os the state action pair, ie x(s,a) = [x_1(s,a), ..., x_n(s,a)]
- 

##### Linear Approximation Limitations
- Only produces a linear function: if the value function isn't linear, it can't be well approximated by this method

##### Kernel Functions
- Each element of the feature vector x can be defined by a separate kernel (or basis) function
- for e.g., x_1(s) = s, x_2(s) = s**2, etc.
- Radial Basis Functions are examples of kernel functions. Essentially, the closer a point is to the center of a normal distribution, the higher the value returned by the function
- Although dotting x and w will be a linear function, because kernel functions can be nonlinear, they will be able to capture non-linear behavior

##### Nonlinear Function Approximation
- We can capture nonlinear functions though by passing the dot product into an activation function. We can update activation functions with G.D.
