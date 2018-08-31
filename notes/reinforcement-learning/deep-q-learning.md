# Deep Q-Learning
- Algorithm that uses NN to solve RL problems
- Expanded the range of problems we can tackle, especially with regard to continuous state spaces

## Neural Networks as Value Functions
- Remember that NN take in a vector and can output a real number
- We can transform our state s into a vector x(s), and then feed that into a NN. If we've trained a NN to approximate a value function, then we can basically do the damn thing.
- The W in this case is a vector of NN weights. So basically the flow is s -> x(s) -> v_π(s, w)

### Training the NN
- IF we know the true value function, we can get the loss by (v_π(s) - v(s, w))^2 and backpropogate through the network to adjust weights with G.D, for e.g.
- With GD, we need the derivatives of the value function with respect to the weights. This can be difficult to compute but certain deep learning libraries make it simple
- btw, the action value function can be treated like the state value function in these cases, where we can have a similar loss function and can use GD to approximate the value function
- The big problem however is that we don't know the true state or action value functions

#### Monte Carlo Learning
- We're motivated to find something to replace the knowledge we need of v_π(s)
- One possibility to to recall monte carlo learning, where we updated the V(s) by V(s) = V(s) + alpha * (G_t - V(s)) for a state s of S
- Instead of using v_π(s) in the loss function for GD, we can approximate by using the return, ie the G_t = R_t+1 + gamma * R_t+2 + gamma ^ 2 * R_t+3 + ...
- This update rule can be used by the NN
- We can use the same for action value functions

##### Monte Carlo with Function Approximation
- Initialize w with random values
- Initialize π with epsilon-greedy(q(s, a, w))
- Repeat until w converges:
- Evaluation step: generate an episode. Then, for t = 1 through T, update w by delta w = alpha * (G_t - q(S_t, A_t, w)) * ∂_w q(S_t, A_t, w)
- Improvement:  π = epsilon-greedy(q(s, a, w))

#### Temporal Difference Learning with Function Approximation
- Note the difference between MC and TD learning. In MC we use the actual return from an episode, and for TD we use the estimated return R_t+1 + gamma * V(S_t+1)
- Remember this estimation is the TD target, and based on the alg (TD(0), for e.g.) can be different
- Our update rule for delta w can be delta w = alpha * (R_t+1 + gamma * V(S_t+1, w) - V(S_t, w)) * ∂_w * V(S_t, w)

##### TD(0) Control with Function Approximation
- Initialize w randomly, π = epsilon-greedy(q(s,a,w))
- repeat for many episodes:
- Initialize S,
- while S is not terminal:
-- Choose action A from state S with policy π,
-- Take action A, observe R, S'
-- Choose action A' from state S' using policy π
-- Update w by alpha * (R + gamma * q(S', A', w) - q(S, A, w)) * ∂_w q(S, A, w)
-- S = S', A = A'

##### TD(0) Control with Function Approximation for Continuous Tasks
- just repeat forever rather than while S is not terminal

##### Q-Learning with Function Approximation
- The main difference from TD(0) is the update rule, which is to adjust w by alpha * (R + gamma * max_a(q(S', a, w)) - q(S, A, w)) * ∂_w q(S, A, w)
- Q-learning is an off-policy method, remember
- We can modify for continuous tasks by repeating forever rather than while S is not terminal

##### SARSA (TD(0)) vs. Q-Learning
- Sarsa is on-policy, good online learning performance, and Q-values are affected by exploration
- Q-learning is off-policy, bad online performance, and Q-values are unaffected by exploration

##### Off-policy Advantages
- Decouples the actions the agent takes from the learning it's doing, which means that we can make different variations of our learning algorithm. For e.g., we can use more exploration while learning.
- Also, agents can learn from demonstration: humans can choose actions and the agent can just learn from the results of doing so
- Supports online or batch learning

### Deep Q Network
- Deep NN that functions as a function approximator
- Given data (pixel data, for e.g.), the deep NN produces an action vector, where the max value corresponds to the action to take
- There were a few techniques that modified the base Q-learning algorithm to avoid weight divergence and unstable policies, including Experience Replay and Fixed Q Targets

#### Experience Replay
- In Q-learning, we get a tuple of state, reward, actions and learn then discard
- However, we may want to hold on to some of this data. Some actions might be costly and some states might be hard to come by.
- The idea is to have a replay buffer where we store these tuples, then sample a small subset of them for learning
- One big benefit is that, in sequential order, these tuples are corelated: one action on one state leads to another state in a corelated way. However, we can instead grab a random, non-sequential subset of tuples to learn from if we do it with a replay buffer instead.
- The problem with this correlation isn't obvious, but in effect it causes the agent to potentially learn a subset of actions really well because they're more represented than other actions, but not learn other actions with as much efficiency
- Doing this allows us to turn an RL problem ultimately into a supervised learning problem. We basically generate a bunch of actions and labels regarding whether they were good or not, and then can used SL algorithms to learn from them.

#### Fixed Q Targets
- Recall that our TD target for Q-Learning relies on the max(q(S', a, w)), which relies on parameters w. However, this target is meant to replace the q_π(S, A), which we don't really know.
- However, that means the target is reliant on w whereas the true action-value function isn't, so this equivalency is mathematically incorrect. We only get away with it in practice because we often adjust w every time in the right direction (generally).
- However, it's likely that this can be broken when all q values are tied together as one function approximation
- Basically, every action affects the target in a complicated, unpredictable manner (often). Mathematically this happens because w is used to approximate q, but w is shifted every step.
- One way to avoid this is to fix the weights to w' and run Q-learning. What this looks like is an update rule delta w = alpha * (R + gamma * max_a(q(S', a, w') - q(S, A, w)))*∂_w q(S, A, w). BAsically, we keep w' for a few steps, then update w' with the latest w and repeat while learning. The target is decoupled and this makes learning much more stable.

### Deep Q Learning Algorithm
- This algorithm interleaves two main ideas: Experience Replay to get a store of experience tuples and Fixed Q Targets to learn: https://cl.ly/aac0ffd30f62
- Initialize the algorithm with replay memory D with capacity N, like a circular queue that keeps track of the N most recent experience tuples
- Initialize q with random weights w
- Initialize w' with w
- For each episode e 1 through M:
-- Initial input frame x_1 (remember that this is meant originally to learn to play video games, so we'll think in terms of images)
-- Prepare initial state S = phi(<x_1>), where phi is the image preprocessing step (reduce dimensions of image, greyscale, and stack with the next four sequential images)
-- For time step t from 1 to T: Sample and Learn

### DQN Improvements
- A number of improvements have been suggested:
- These are outlined: http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.73.3097

#### Overestimation of Action Values: Double Q-Learning
- There's a tendency to overestimate the action values in Q-learning
- Note that in Q-learning, the TD target involves grabbing the q value of the action that maximizes q-value. Initially especially, this can be wrong, because there isn't enough information to truely judge whether this is the best action or not
- Turns out, this usually results in an overestimation of action values 
- Double Q-Learning is the idea of using one set of parameters w to find the action with the max q-value, and other set of parameters w' to get the q-value for that action. This has been shown to work well in practice.
- This will make it so that if an action is chosen with w in mind but doesn't actually have a large q-value with another set of parameters w', it won't make a huge dent in the update.
- We can either have two action value functions for Double Q-Learning and randomly choose one each iteration to update, or, using fixed Q-targets, we can use that w' fixed parameter as the second w'.
- More information at https://arxiv.org/abs/1509.06461

#### Prioritized Experience Replay
- In experience replay, we keep a store of previous experience tuples. However, note that some experiences are more important than others to learning, and may be infrequent.
- If we sample this replay buffer uniformly, there will be a small chance of getting the important or infrequent experiences.
- Also, because of limited space, older experiences may get lost.
- Prioritized replay uses some sort of prioritization to decide what to prioritize when sampling. Some methods of prioritization:
-- Note that if TD Error is large, we're likely to learn something new from the experience. If we store the magnitude of the the TD Error R_t+1 + gamma * max_a q(S_t+1, a, w) - q(S, A, w), then we can use that to create a sampling probability by taking that TD error magnitude p_i and setting the prob of sampling to p_i / ∑_k p_k.
--- Note that if a TD Error is close to 0, it may not mean that we have nothing else to learn but could be close to 0 because of the limited experiences we've sampled to that point. To avoid this, we can add a small value e to every TD Error Magnitude.
--- Another issue is that we might reuse a particular experience over and over. We can add a parameter a to the sample probability, i.e. p_i^a / ∑_k p_k^a. a can be varied where a = 0 means we use complete randomness in sampling and a = 1 means we use only prioritization.
--- We need to change our update rule to delta w = alpha * ((1 / N)*(1 / p_i))^b ∂_i ∂_w q(S_i, A_i, w)
- More information https://arxiv.org/abs/1511.05952 

#### Dueling Networks
- For DQNs, we generally have lots of convolution layers followed by a densely connected layer
- For Dueling Networks, we can have a bunch of convolution layers followed by two different branches of densely connected layers: one for the state-value, one for the advantage values. These can combine back together for Q-values with Q(s,a) = V(s) + A(s,a)
- The idea behind this is that the values of states don't change much with values of corresponding actions. However, there is a bit of a difference, which is where the advantage values come in.
- More: https://arxiv.org/abs/1511.06581

## Implementing this with Keras: 
- https://keon.io/deep-q-learning/
