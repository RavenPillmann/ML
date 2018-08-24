# Temporal Difference Methods
- Thinking of AI of the future, agents will need to learn from online streaming data like we do.
- Agents won't have breaks from interaction with the work. MC methods on the other hand need tasks to be episodic, because it needed to estimate returns.
- For more realistic learning, we need something else, because (again) agents won't have breaks from interaction.
- The idea behind TD methods is that we estimate a probability that something will happen with every action: instead of waiting for a car to crash to learn or make a move, we estimate the chance of crashing for every timestep instead of waiting to crash to learn. 
- TD methods amend estimates at every time step.

## The Prediction Problem
- Given π, estimate v_π.
- In MC, generated an episode and used it to get Q. We did this for many episodes, which converged to π_*.
- In MC, recall that our update rule for the value function was V(s) = V(s) + alpha * (G_t - V(s))
- Using bellman equation, we can instead write this out as V(s) = V(s) + alpha * (R_t+1 + gamma * V(s_t+1) - V(s)). Note that this allows us to update V(s) without having to look at the returns at the end of the episode, and that we can update V(s) at every change in state (because a change from s -> s_t+1 will yield a reward R_t+1, so we have everything we need to update V(s)).
- Note that R_t+1 + gamma * V(s_t+1) is the TD Target, which is basically the new information we gained about the estimate of V(s) when we make a time step. The size of alpha will determine how much we take this TD Target into consideration. (alpha is between 0 and 1. If 0, we completely ignore target and if 1, we only take the target into consideration).

### One-step TD (TD(0))
- V(s) = 0 for all s in S
- Observe S_0
- t = 0
- Repeat for many timesteps:
-- Choose action A_t using policy π
-- Take action A_t and observe R_t+1 and S_t+1
-- V(S_t) = V(S_t) + alpha * (R_t+1 + gamma * V(S_t+1) - V(S_t))
-- t = t + 1
This returns V, should be about v_π if we do it long enough. 
This is also for continuous tasks. For episodic tasks, we repeat (Observe S_0...t =t+1) for many episodes and return V at the end. We loop until the terminal time step.

### Updating Action Values
- Instead of updating V, we update Q(s,a). We do this after every state/action pair in a task, rather than after every state update.

## The Control Problem 
- Determine π_*.
- We can update TD Prediction to update the policy after every time step. Recall that policy the policy for TD Prediction was unchanged after each action was updated. 

- Initialize Q(s,a) = 0 for all s in S and a in A
- Construct a policy π using epsillon-greedy, where epsillon is 1.
- After each action is observed:
-- Update Q(s, a)
-- Update the policy, π = epsillon-greedy(Q).

- This algorithm is known as Sarsa or Sarsa(0) and returns the approximate Q. Remember, the policy we're left with is an epsilon-greedy constructed from the latest approximation of Q, so by updating our estimate for Q we are effectively updating our policy.

### Sarsamax (Q-Learning)
- Sarsamax is a variation on Sarsa, but the output is the same
- In Sarsa, note that we update the policy after we get both a next state and next action from the epsilon greedy policy. With sarsamax, we update just after we get the next state.
- In Sarsa: S_0 A_0 R_1 S_1 A_1 (Update Q) R_2 S_2 A_2 (Update Q) etc
- Sarsamax: S_0 A_0 R_1 S_1 (Update Q) A_1 R_2 S_2 (Update Q) etc

- Initialize Q(s,a) to 0 for all s in S and a in A
- policy = eps-greedy(Q)
- After we get a next state and reward:
-- Q(s,a) = Q(s,a) + alpha(r' + gamma * max(Q(s', a)) - Q(s,a))
-- Update policy π = eps-greedy(Q)

- Note in particular that instead of using the Q(s', a') for the update we're using just the greedy policy max(Q(s', a)) 

### Expected Sarsa
- Closely resembles sarsamax but differs in the update step again. When choosing actions with sarsamax, actions are chosen based on which action maximizes the action-value for the next state. Expected sarsa instead uses the expected action value of the next state. In other words, instead of max(Q(s', a)) expected sarsa does ∑_a-in-A π(a|s')*Q(s',a)


## Generally
- Sarsa and Expected Sarsa are both On-policy
- Sarsamax is off-policy
- Online on-policy methods generally have better performance than off-policy methods
- Expected Sarsa generally works better than Sarsa


## All of the Algorithms Discussed Here can be Found Here
https://github.com/udacity/rl-cheatsheet/blob/master/cheatsheet.pdf
