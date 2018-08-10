# RL Solution
- We can think of a solution for an RL problem as a set of action responses to any environment states

## Policy
- The simplest policy is a mapping of S to A (aka, given an input S, give an output A)
- This kind of mapping is called a deterministic policy, π
- A stochastic policy is a mapping π that maps to a range [0, 1], such that π(a|s) = P(A_t = a|S_t = s), and is the probabilty that the agent takes action a while in state s.
- Either way, a policy is a mapping from state s to action a. This is the solution to an RL problem.

## Picking the Best Policy
- State-Value Function: for each state, the state-value function yields the expected return if the agent started in that state and followed the policy for all time steps.
- State-value function v_π(s) = E_π[G_t|S_t=s]. In other words, the state-value of s is the expected return if the agent were to start at state s using π for all timesteps onward.
- State-value function will change with a change in policy

### Bellman Expectation Equation
- v_π(s) = E_π[R_(t+1) + gamma*v_π(S_(t+1)|S_t = s]
- This equation says that we can calculate the value at any state as the expected values of the immediate reward plus the discounted return of the state that follows.

### What does it mean for a policy to be better?
- A policy π' is better that π iff v_π'(s) > v_π(s) for all states s in S.
- It's possible that two policies cannot be compared like this.
- It's guaranteed that there is an optimal policy out there, which is what the agent is searching for, and it may not be unique.

### Action-value Function
- The value of taking action a in state s under policy π is q_π(s, a) = E_π[G_t | S_t=s, A_t = a].
- The action-value function for each state s and action a yields the expected return if the agent starts in state s and chooses action a then uses the policy to choose its actions at all future timesteps.

### Optimal Policy
- Optimal state-value and action-value functions are notated v_* and q_*

### Main Idea for finding optimal policy:
- Agent starts with an interaction, from that gets the optimal action function q_*, and from that optimal policy π_*
- Assume that we have an optimal action-value function and that we want to derive from that the optimal policy...
- Basically, given that we have an optimal action-value function, from each state we can simply do the highest-ranked action. If there's a tie between actions we can just pick one. This gives us an estimation for our optimal policy.
- The million dollar question is how we can get the optimal action-value function.


