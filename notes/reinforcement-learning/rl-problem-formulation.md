# Problem Formulation

## Basic Idea
- An agent is presented with a situation from the environment, and does an action. The environment passes back a reward, as well as a new situation (observation). This process repeats.
- This observation is the state.

## More Mathematical
- Environment gives state S0 to the agent, and the agent responds with action A0.
- Environment then given reward R1 for S0 and A0, as well as S1. Agent then chooses A1 and responds.
- Essentially, this interation is characterized as S0 A0 R1 S1 A1 R2 S2 A2..., where the rewards are always the most significant quantity for the agent.
- Goal of the agent is to maximize expected cumulative reward

## Episodic vs. Continuing Tasks
- Episodic tasks are tasks that have a well defined ending point. For a self driving car, for e.g., this ending point is crashing.
- When task is accomplished, we call it an episode. When an episode is completed, we look at the total reward received to see how well it did.
- Based on this, it will start from scratch with added knowledge about past life. Agents should make better decisions as they live out many episodes.
- Continuing Tasks are tasks that have no defined ending point, like a stock market buy/sell program. The agent lives forever, so it needs to learn as it does its job.
- Remember, a task is an instance of a reinforcement learning PROBLEM.

## Rewards
- Sparse rewards are rewards that are sparse, like rewards that only come at the end of an episode (+1 if you won a game, -1 if you lost). These can make RL extremely difficult because you get limited insight regarding what went wrong during the episode, just that altogether the agent did poorly.

## Reward Hypothesis
- States that all goals can be framed as the maximization of *expected* cumulative reward.

### Cumulative Reward
- Cumulative reward is an important concept, in the sense that we can't just try to maximize reward at each step. Sometimes the maximization of a certain step won't mean a high cumulative, overall reward/success.

#### Return at Timestep T
- We define the return at timestep t as G, which is R_t+1 + R_t+2 + ....
- At each t, the agent picks A_t to maximize expected G_t.

### Discounted Returns
- A reward in the future is usually less certain. For e.g., I am less likely to know what kind of reward I'll get in a year from doing one thing now.
- Discounted returns is motivated by this, and is defined as G_t = R_t+1 + gamma * R_t+2 + gamma^2 * R_t+3...
- This causes the goal to care more about closer rewards. Gamma is the discount rate, which is between 0 and 1.
- Gamma is set by the trainer, not the agent.
- Discounted Returns is more relevant for continuing tasks

## Markov Decision Processes, MDPs
- A formal definition for RL problems
- A is the action space, the set of all possible actions an agent can take
- S is the state space, which is a set of all nonterminal states
- S^+ is the sets of all states including terminal states
- A_s is the set of actions available for a certain state s. For example, a given state might naturally limit the actions available, so we denote that as A_s.
- Actions happen from certain states, and there are probabilities regarding what the next state is. Based on which state happens next, the reward is given.
- Note that the new state/reward of timestep t+1 are solely determined by the state and action at t.

- An MDP is defined by S, A, R (set of rewards), the one-step dynamics of the environment, and a discount rate
- Discount rates are always set to a number close but less that 1, for e.g. 0.9
- The agent knows S, A, and a discount rate, but doesn't know the one step dynamics or rewards

### Finite MDP
- Finite MDPs are defined as MDPs where S and A have a finite number of elements.

