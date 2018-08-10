# Introduction to RL
- When we learn in the world, we learn from previous interactions

## Reinforcement Learning
- The process of creating algorithms that learn from interactions.

## Applications
- Self driving cars (https://selfdrivingcars.mit.edu/)
- Board games (TDGammon, AlphaGO)
- Robotics (specifically, in teaching a robot to walk, https://deepmind.com/blog/producing-flexible-behaviours-simulated-environments/)

## Idea
- An agent (the subject of RL) has no previous knowledge
- The agent is either incentivized by positive reinforcement or punished by negative reinforcement
- The agent is faced with commands, and its response/the actions to take are numerous/the agent doesn't have background information about various actions
- The agent will at first pick an action at random. Then, the agent waits for a response (positive or negative).
- The more the agent acts and sees response, the more it learns.

## Exploration and Exploitation Dilemma
- Exploration: exploring potential hypotheses for how to choose actions
- Exploitation: exploiting limited knowledge about what is already known that should work well
- This has inspired a subfield of RL

## Excerpts
- Can follow this textbook https://s3-us-west-1.amazonaws.com/udacity-drlnd/bookdraft2018.pdf
- https://github.com/udacity/rl-cheatsheet/blob/master/cheatsheet.pdf

## RL Problems
- RL problems can be defined by a policy, a reward signal, a value functions, and an optional model of the environment
- Policy: Defines how an agent may behave at a given time, a mapping from different states to the actions the agent can take.
- Reward Signal: Defines the goal of a RL problem. At each time step the environment sends the agent a reward (positive or negative) and the reward signal is the primary basis for changing a policy.
- Value Function: the value function of a state is the total amount of reward that might be accumulated starting at that state. It's the long term prediction of the benefits, and is made up of reward signals over a period of time.
- Models are optional, but they are basically used to try and predict how an environment will respond to the agent's actions. They are used in planning, or the act of trying to pick a move based on perceived future actions.
