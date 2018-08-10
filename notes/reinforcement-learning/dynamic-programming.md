# Dynamic Programming
- In the DP setting, the agent has full knowledge over the markov decision process characterizing the environment.
- Aka, it knows how the environment decides reward and how the environment decides the next state. The goal is still to find the optimal policy.

## Given a policy, how do we determine a value function
- Given a policy π, we can find v_π(s) for all states s of S.
- Basically, recall that the v_π(s) can be derived from the bellman equation. For each state s in S, we can do an iterative approach of guessing the value function for each state by going through them all and adjusting/updating the value at each state with the bellman equation. This algorithm converges to the true value function, and is known as the Iterative Policy Evaluation
- We can also sometimes directly solve the value function for each state with a system of equations, but in practice the iterative approach works best.

## Iterative Policy Evaluation
- Input: MDP, policy π
- Output: State-value function approximation v_π.
- Set all values of states to some initial value, for instance 0.
- Repeat: for each state s of S, use the bellman expectation equation to estimate a new value for that state.
- We can stop repeating when the max change in all states is smaller than some small positive number.

## Getting an Action-Value Function 
- Given a state-value function (or estimate of a state-value function), we can convert to an action-value function
- Recall that an action-value for a particular state can be derived as the immediate reward for the action and the value function of the resulting state.
- Therefore, if you know the immediate reward and the state-value function, you can get the action-value function.
- However, also recall that this is the case in deterministic action functions, but that many action functions are stochastic. In this case, we can still derive this, but we need to keep in mind that q_π(s,a) will be the expected value of the sum of the immediate reward r + gamme * v_π(s').

## Policy Improvement
- Policy evaluation gets us halfway to the goal. However, we should used the value function to improve the policy.
- We can take the evaluation of the policy to get a value function, then we can use policy improvement to get better policies.
- This works like this: policy π -> Iterative Policy Evaluation -> value function v_π -> Policy Improvement -> policy π', where π' > π
- Policy Improvement is therefore the step from value function to new policy.

### Two Steps
- Convert the state-value function to an action-value function.
- To get from an action-value function to a policy, we just need to look at our action value function, and at each state, the policy will be to do the action that maximizes value.
- If multiple actions would maximize this, we can just pick one randomly OR make it a stochastic policy

### Policy Iteration
- Policy Evaluation is deriving a state value function from a policy
- Policy Improvement is deriving a policy from a state value function (by way of an action value function)
- We can combine the two into a Policy Iteration algorithm


#### Policy Iteration: Algorithm
- Input: MDP, small positive number Theta
- Output: Policy π which is approx π_*
- Begin with a random policy, like the equiprobable policy
- Repeat: Policy Evaluation to get V (using MDP, π, Theta), Policy Improvement to get π', if π = π' then break, else set π to π'
- Return π 

## Different versions of this algorithm

### Truncatred Policy Evaluation
- Policy Evaluation can be modified to make it quicker. Instead of stopping the repetition in the Iterative Policy Evaluation, we can use a definite number of steps. This is okay because we'll use Policy Improvement to make it better.
- We may want to then modify policy iteration so that we stop when the difference between the the old value function and the new value function is sufficiently small instead of unchanged

### Value Iteration
- Policy Evaluation is modified down to a single sweep. Evaluation and improvement both occur over a single sweep of the state space.
- Basically, we don't need to evaluate-improve over and over again: we can just move through it one time. This is a guaranteed way to get the optimal policy.

## Cheat Sheet
https://github.com/udacity/rl-cheatsheet/blob/master/cheatsheet.pdf

