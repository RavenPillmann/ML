# Monte Carlo Methods

## The Prediction Problem
- Given a policy π, determine v_π or q_π
- Recall that environment dynamics are unknown, so the value function needs to be estimated by interacting with the environment
- It's possible to generate episodes from a given policy b, and use that to estimate v_π, and this is an off-policy method
- On-policy method uses a policy π and then uses the episodes to estimate v_π.


### Estimating v_π from π
- Start by generating a bunch of episodes
- Then, for each state, we can look across all episodes for the occurances of that state.
- If we are using the first-visit MC method, then we can average the returns gained from the first visit to the state in every episode and estimate that that value is the value function for that state.
- If we are using the every-state MC method, we can average the returns gained from all visits
- The choice to use either MC method is ours.
- Remember: We need to average the returns! Which means we need to calculate the expected future returns and divide by the occurances of the state, not just the rewards.

### Estimating q_π from π
- In a similar way to estimating v_π with MC methods, we can calculate the average returns from pairs of state/actions for each state and action pair
- Note however we can only estimate action values for state/action pairs that we visit in episodes. This means that if the policy is deterministic/doesn't allow for certain state-action pairs, we will not get action-value estimates for those unallowed states. 
- If we make this a stochastic policy and arrange a small percentage of different actions, then we can get around this. That is, if we at least make it possible for an action to occur for each state, we can deal with this weirdness.

## The Control Problem
- Determine the best policy, π_*, from interaction with the environment.

### Generalized Policy Iteration
- Refers to the general process of looping between evaluation and improvement steps to find π_*.

### MC Control Algorithm
- Generally, starting with a Q(s,a) = 0 and a starting policy, repeat: generate an episode using π, use the episode to update Q, then use Q to improve π.

#### Iterative Means
- Instead of calculating the mean value after all episodes, we can calculate the mean iteratively by setting mean = mean + 1/k (x_k - mean), where x_k is the kth return for a state/action pair

#### Policy Evaluation
- We can modify our policy evaluation step to allow us to improve the policy after each episode. Specifically, we can use the iterative mean function to update the action value at each time step for a given state and action.

#### Policy Improvement
- So in the Dynamic Programming problem we followed a greedy algorithm for improving policy, namely picking the action at each state that maximizes the reward.
- However, in the case of this sort of policy iteration where we want to update the policy after each episode, doing something like this is a rough idea because we may not end up maximizing expected return just because we might rule out an action that could possibly yield high returns but hadn't up to the point that we chose an optimal action.
- Instead, we can set a stochastic policy

##### Epsilon-Greedy Policy
- π(a | s) = 1 - epsillon + (epsillon / number of actions for state s) if a = the best action, and epsillon / number of actions for state s otherwise. 
- Epsillon is a small positive number (between 0 and 1).
- This allows us to be really close to the optimal greedy policy without ruling out other possibilities.

##### Exploitation-Exploration Dilemma
- We want our policy to converge, but as we know following a purely greedy policy and exploiting our knowledge can lead to suboptimal policies.
- Therefore, we need to explore other possibilities as well
- The Epsilon-Greedy Policy is a way to balance all of this. We can deal with the Exploitation-Exploration Dilemma by modifying epsillon and changing its value around to see what sticks.

##### Setting Epsillon, in Theory
- Given that, at the beginning, an agent knows very little about the environment, it should probably focus on exploring. Setting epsillon to 1 allows for an equiprobable random policy that accomplishes this.
- Later on, we should probably move epsillon towards 0, because the agent knows more and more and we should lean towards the greedy approach of allowing the agent to make what it thinks will be optimal.

##### Greedy in the Limit with Infinite Exploration (GLIE)
- GLIE refers to two conditions that must be met for us to guarantee that MC Control converges to the optimal policy π_*. In particular: 1) Every state-action pair s, a is visited infinitely many times and 2) the policy converges to a policy that is greedy with respect to the action-value function estimate Q.
- These conditions ensure that the agent continues to explore for all time steps and the agent gradually exploits more and explores less.
- One way to satisfy these conditions is to modify the value of epsillon. In particular, if ep_i is epsillon at the ith step, GLIE is met if: 1) ep_i > 0 for all time steps i, and 2) ep_i decays to zero in the limit as time step i approaches infinity.

##### Setting Epsillon, in Practice
- Even though convergence is not guaranteed by the math here, we can often get better results by either 1) using fixed epsillon, or 2) letting epsillon decay to a small positive number like 0.1.

#### MC Control 
- This is generally how MC Control should work: https://cl.ly/3J340Q1Q132Z

##### Constant Alpha
- Currently, we are updating Q(s,a) = Q(s,a) + (1/N(s,a)) * (G_t - Q(s,a)).
- We can think of G_t - Q(s,a) as an error, where G_t is the actual return we expect and Q(s,a) is the current estimate of the action-value for that state. If G_t - Q(s,a) is > 0, then we are getting more return than we estimate getting, so we want to increase Q(s,a). If it's less than 0, then the return we're getting is less than our estimate, so we decrease Q(s,a).
- When we adjust Q(s,a), though, the amount of change is diminished as N(s,a) increases. The change is proportional to 1/N(s,a).
- Instead, if we swap 1/N(s,a) with a constant step size alpha (for e.g., first time step is 0.1, second is 0.2, so on...), then the most recent returns are emphasized more than those received in the past. This is important because the policy is constantly becoming more optimal, which means that later time steps are more important than earlier ones.
- Alpha should be greater that 0 and less than or equal to 1, where when alpha = 0 the action-value function estimate is never updated by the agent and when alpha is 1 the final value estimate for each state-action pair is always equal to the last return experienced by the agent.
- Lower alphas ensure the agent considers a longer history of changes, higher alphas means it considers a shorter history.
