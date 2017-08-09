# 10 - Planning Under Uncertainty

This brings planning and learning together which is ultimately useful and truly allows robot agents to interact with their environments.

- Fully observable & deterministic - A*, Depth first, breadth first
- Fully observable & stochastic - MDPs
- Partially observable & stochastic - POMDP

MDP has states, actions, state transition matrix that are probabilistic. Furthermore there is a reward function.

Planning is now trying to maximize the reward.

## MDP Gridworld

Grid world is a simple MDP. Policy is a plan for each possible state.

Traditional (search tree) planning has issues because:

- Branching factor is large
- Tree is deep due to possible loops in the states
- Many states visited more than once

Policy overcomes all of these issues.

In addition to previously mentioned properties discount factor is often used to provide a sense of cost for taking actions.

### Value Iteration

Between the reward and discount factor we can derive a sense of value functions that iterates and converges on the "value" of each state. Based on these values we can find the optimal policy.

Value iteration is an algorithm for finding the value iteratively or recursively. Start of with random initial values for each state. Re-estimate the values based on the values of neighboring states. **Bellman's Equation!!!**

Optimal policy is the policy that maximizes the value, which is the argmax of a component of the value iteration update.

The reward on arbitrary steps can have a big impact on the optimal policy between states.

## POMDP

POMDP addresses problems with exploration vs observation. MDPs this isn't and issue because the world is fully observable so you don't need to explore.

Consider a forked path with 2 terminal states, one positive and one negative reward. There is a sign backwards in the path that can be read to know which state is the positive reward.

A deterministic situation you would know which fork has the reward and you could go directly towards it. If you take the two possible states you could be in and were to somehow combine the solutions this wouldn't solve the pomdp because you're directed towards the fork and there's no notion of being directed toward the sign. 

In reality the representation is best thought of as 3 belief states one with a deterministic positive reward on the right, one with a deterministic positive reward on the left, and an unknown state where the sign has a 50% chance of taking you to  either of the two deterministic states (introducing stochasticity). In this new belief space with partial observability you can use value iteration and the increased state space to determine an optimal policy.
