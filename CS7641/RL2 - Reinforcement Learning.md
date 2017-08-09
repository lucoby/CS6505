# RL2 - Reinforcement Learning

Think of RL as a API (Application Programmer Interface)

model (T, R) -> Planner -> Policy

Transitions \<s, a, r, s'>* -> Learner -> Policy

Solving an MDP takes models and turns them into policies.

On the other hand Learners take transitions and learn a policy which makes it learning, specifically reinforcment learning.

Why is it named reinforcement learning? Way back when psychologist discovered that if you present an animal with a stimulus that and reward the action this strengthens the response to this stimulus. Not really reinforcement... more like reward maximization.

## API

There are several additional variations to the reinforcement learning API

transitions -> modeler -> model

model -> simulator -> transitions

This leads to the idea of different possible ways of gluing the APIs together to do reinforcement learning:

RL-based planner*???: Model -> Simulator -> Transitions -> Learner -> Policy

Model-based RL: Transitions -> Modeler -> Model -> Planner -> Policy

* No common name in the field but a famous example is TDGammon by Tesauro that used a simulator to learn to play Backgammon

## Approaches to RL

policy: s -> pi -> a

Policy search algorithms: Directly use what you learn but learning is indirect and hence difficult due to the temporal credit assignment problem. The data set doesn't contain the actions you should take.

utility / value: s -> u -> v

Value-function based algorithms. It's possible to learn more about the world and estimate the utility as far as learning goes but it can be tricky turning this into a policy via a max function and some usage of the Bellman's equations.

s, a -> T, R -> s', r

Model based algorithm. If we can learn T (transition models) and R (rewards) then we can solve the Bellman's equations to get utility and then argmax that to get policy. This is the most direct as far learning goes but it's computationally indirect to turn the results of the learning into a policy.

Focus on the middle scenario: value based approaches!

## A new kind of Value function

The old value function

```
U(s) = R(s) + \gamma max_a \sum_{s'} {T(s, a, s') U(s')}
\pi(s) = argmax_a \sum_{s'} {T(s, a, s') U(s')}
```

Utility = reward + discounted future reward. Policy chooses the action that leads to the highest expected utility.

Q function (sometimes quality):

```
Q(s, a) = R(s) + \gamma \sum_{s'} {T(s, a, s')max_a' Q(s', a')
```

Value for arriving in S, leaving via a, proceeding optimally thereafter. Similar to Utility which is reard of the state + optimal utility thereafter, however Q ties you to action a.

U and pi can be definied via Q:

```
U(s) = max_a Q(s, a)
pi(s) = argmax_a Q(s, a)
```

## Q Learning

Q learning estimates the value of the Q function based on transitions and reward. However because it's reinforcement learning and not an MDP, we don't have R.

```
\<s, a, r, s'\>: \hat{Q}(s, a) <-^{\alpha} r + \gamma max_{a'} \hat{Q} (s', a')
```

The transition is the starting in stat s, taking action a and winding up with reward r and in state s'

We update Q by some learning rate alpha based on the imeediate reward r and discounted estimated value of the next state (\hat{Q}) because max_{a'} \hat{Q} (s', a') overall is the utility of the next state and the overall update represents the utility of the state.

Note that:

```
V <-^{\alpha} x

is equivalent to:
v <- (1 - \alpha) v + \alpha x
```

### Learning incrmentally

V_t <-^{\alpha_t} X_t

where x_t is a random variable and alpha_t has the properties that

```
\sum_{t=1}^{inf} \alpha_t = inf and
\sum_{t=1}^{inf} \alpha_t^2 < inf

which works for say alpha_t = 1 / t (1/t -> ln(t), 1/t^2 = pi^2 / 6)
```

Over time v converges to the expected value of x (E[x])

## Estimating Q from Transitions

So ultimately:

```
\hat{Q}(s, a) <-^{\alpha} r + \gamma max_{a'} \hat{Q} (s', a')

= E[r + \gamma max_{a'} \hat{Q} (s', a')]
= R(s) \gamma E_{s'} [max_{a'} \hat{Q} (s', a')]
= R(s) \gamma \sum_{s'} T(s, a, s') \hat{Q}(s', a')
```

The first step of the analysis is... questionable... because Q hat changes over time but there is a theorem that proves it actually converges

## Q Learning Convergence

```
Q hat starts anywhere
\hat{Q} (s, a) <-_{\alpha_t} r + max_{a'} \hat{Q} (s', a')

then: \hat{Q} (s, a) -> Q(s, a)
if s, a visited infinitely often
\sum_t \alpha_t = inf, \sum_t \alpha_t^2 < inf
s' ~ T(s, a, s'), r ~ R(s)
```

## Choosing Actions

Q-learning is a family of algorithms that vary along
- How to initialize Q hat?
- How to decay alpha_t?
- How to choose actions?
    - Dumb: always choose the same action (won't learn)
    - Could pick a random actions independent of what we're learning... (won't use what you learned)
    - Use Q hat (will use it) possible issues depending on how Q hat is initialized i.e if for all Q hat (s, a_0) is great and for all s, a != a_0 Q hat (s, a) is worse than the actual Q(s, a_0). Greedy but with a local min
    - Random restarts! (slow)
    - "simulated annealing" like approach of randomly taking actions sometimes i.e. pi hat = argmax_a Q_hat(s, a) with probability 1 - epsilon and with probability epsilon taking other actions

This last possibility has the ability to learn and avoid local minimum and uses what it learns!

## Epsilon Greedy Exploration

If GLIE (Greedy within the Limit with infinite Exploration or basically a decayed epsilon) two things happen:

- Q hat -> Q
- pi hat -> pi *

Exploration-Exploitation dilemma - Exploration is about getting the data that you need (learning) and exploitation is about actually using what you learned. The dilemma exists because there is only one agent interacting with the world with conflicting actions.

Exploration and exploitation dilemma come up in other scenarios and in model-based learning you can keep track of what you know and to what degree you know it.

Exploration Exploitation is the fundamental trade-off in reinforcement learning. Learning is well studied within Machine learning, planning is well studied in planning and scheduling so reinforcement learning really adds to this because learning and planning interact and depend on each other

## Summary

- Learn how to solve an MDP- Do not know T and R, can only interact via s, a, r, s'
- Approaches to RL
- Connection to planning
- Connection to function approximation, generalizing
- Q-learning: Convergence, family of algorithms, different behaviors and trade offs
- Exploration-Exploitation

One extra way to explore / exploit trade off is set all Q hats to be the maximal value (optimism in the face of uncertainty) and it explores many things as it eliminates actions and depresses them to their "actual" value
