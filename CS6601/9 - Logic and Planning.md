# 9 - Logic and Planning

Logic is useful because you can make observations about the world and then reason from that point without having to "worry" about the world anymore and the conclusions will remain true so long as the original observations are true.

AI started with a lot of focus on logic. Some things worked well but others didn't for 2 reasons:

1. Boolean logic doesn't work that well because the world is probabilistic so the logic of probability is a better fit to the world.
2. It's too laborious to write out all of the formal logic by and so now there is a larger emphasis on machine learning from data

Logic is still useful. In many domains we can eliminate a large amount of the uncertainty of the world and instead solve the logic problem.

Thoughts on the future:
- Learning by example
- Transfer learning across domains where we learn to play in one area and execute in another error
- Interactive learning - focus on interface and explain things in human-understandable terms so humans can add and adapt to solve problems

Things to pay attention to Resolution algorithm, graph plan, value iteration

## Introduction

AI is challenging. Tackles 3 areas of complexity: complexity in terms of agent (start with reflex, then goal-based and utility-based), environment (simple environment, then partial observability, stochastic actions, and multiple agents), representation (model of the world becomes more complex). Logic can be used to better model the world

## Propositional Logic

Say we have B, E, A, M, and J. Like probabilistic logic these variables can be true or false. But unlike probabilistic logic, or belief in the truth of the variables is not a number. Our belief is that these are true, false, or unknown.

We can make statements by combining them with logical operators i.e. 

```Tex
(E \vee B)\Rightarrow A \text{ (E or B implies A)}
A \Rightarrow (J \wedge M) \text{ A implies (J and M)}
J \Leftrightarrow M \text{ J is equivalent to M}
J \Leftrightarrow \neg M \text{ J is equivalent to not M}
```
## Truth Tables

Truth tables list all possible values for all of the variables and all of the resulting truth false values for the compute propositional logic sentence.

Truth tables don't necessarily correlate with "English" logic i.e. or is either or both whereas English is ambiguous on xor or or. Implies also works differently. English implies a causal relationship, however, from propositional logic true implies true, so "5 is odd" implies "Paris is the capital of France"

A valid sentence is one that is true in every possible model for every combination of values of the propositional symbols.

A satisfiable sentence is one that is true in some models, but not necessarily in all models.

## Propositional Logic Limitations

Propositional logic is powerful for what it is designed to do and there are very efficient inference mechanisms for determining validity and satisfiability. However there are several limitations:

- It can only handle true and false. It cannot handle uncertainty.
- Can only talk about true or false. Can't talk about objects with non-boolean properties like size, weight, etc.
- There are no shortcuts to succinctly talk about a lot of different things happening i.e. if you had a room that had a 1000 rooms and they were all clean you'd have to have a 1000 conjunctions to state that.

First order logic addresses the first two issues.

## First-order Logic

Relationship to other forms of logic:

- First order logic - world: Relations, objects, and functions; belief: relations are true, false, or unknown
- Propositional logic - world: facts; belief : facts are true, false, or unknown
- Probability theory - world: facts; belief [0..1]

Another way to look at representation is to break the world up into representations that are **atomic** meaning that representation of a state is simply the state, with no additional pieces. This is used in problem solving and search i.e. states could be A or B one might be a goal or not, but there is no internal structure to the state.

In propositional and probability theory the world is broken up into facts so this is called **factored**.

The third type of representation is **structured**. In structured, an individual state is not just a set of values for variables, but can include relationships between objects, branching structure, and complex relationships between one object and another.

### Models

In propositional model is represented by a single variable i.e. {P: T, Q: F}

In first order logic models can contain more complicated objects. The objects and be defined with objects. There are also other properties that include:

For example a set of 4 scrabble tiles A_1, B_3, C_3, D_2.

- **Constants** that can be mapped to the objects.The mapping to constants does not need to maintain a 1-to-1 relationship. i.e. {A, B, C, D, 1, 2, 3, CEE} 
- **Functions** that maps objects to objects. i.e. Numberof: {A->1, B->3, C->3, D->2}
- **Relations** i.e. Above: {[A, B], [C, D]} (binary relation set of tuples of length 2) Vowel{[A]} (unary - set of tuples of length 1) Rainy {} or {[]} (relations over no objects empty set for not rainy set of an empty tuple for rainy because no objects)

### Syntax

Like propositional logic there are sentences that describe whether facts are true or false. Unlike propositional logic we also have terms that describe objects.

The atomic sentences are predicates corresponding to relations i.e. vowel(A) or above(a, b) is an atomic sentence. There is also a distinguished relation and equality relation. i.e. 2=2. The equality relation is in every model. Sentences can be combined with all of the operators from propositional logic i.e and, or, not, implies, equivalent, and parentheses.

Terms which refer to objects can be constants i.e. A, B, 2. They can be variables which are typically denoted in lowercase, and functions i.e. numberof(A) which is just another name or expression for referring to 1 in the model.

Quantifiers: Unique to first order logic are \forall, and \exists. These quantifiers are commonly used. Typically forall is paired with a conditional because we rarely are making a statement about every single objects, instead just a subset for which the condition is true. Typically exists doesn't contain a conditional since it's just talking about a particular object.

### What does first order mean? 

First order implies the relations are on objects, but not on relations. In higher order logic there could be a transitive relation that talks about relations themselves i.e. for all R transitive(R) is equivalent to for all a, b, and c R(a, b) and R(b, c) implies R(a, c)

### Axioms

When establishing Axioms it is generally more useful to use equivalent rather than implies ore just an assertion as it has an if and only if implication.

## Planning

AI is the study and process of finding appropriate actions for agent so planning is core to a lot of AI. Previously problem solving and search over a state space which can be used in a variety of state however this is limited to deterministic and fully observable environments.

### Problem Solving vs Planning

In a problem solving approach like the search of a state space, the "algorithm" is first to calculate the best path then execute the path without any real feedback from the environment. This tends to perform poorly and in fact even with humans blindfolded to a certain degree and told to walk in a straight path they often end up walking in convoluted paths.

### Planning vs Execution

Interleaving planning and execution is mostly necessary when properties of the environment make it difficult to deal with:

- Stochastic - In the environment we aren't 100% sure what an action will do.
- Multi-agent - The environment may have other actors who act in their own interest or act unexpectedly - this can't necessarily be predicted during planning.
- Partial observability - we don't necessarily know which environment or state we're in if we only have access to information by getting to a certain state

In addition to environment issues:
- Unknown - we might lack knowledge on our own part i.e. we don't have a accurate or complete map
- Hierarchical - often only high level plan is know i.e. get from A to B, but not get in car, turn key, press pedal etc.

## Observability

### Sensorless

In a deterministic, unobservable world we have to describe our initial belief as being in any possible state. However, as we take actions, because our actions are deterministic, we can narrow or belief of the world based on the actions that we took. Actions are no longer inverses like they are in the observable world because we don't return to a state of complete unknown by inversing our actions.

### Partial Observability

Actions and observations are cycled and the observation cycle can reduce or at least keep equal the number of possible states that we believe that we are in.

## Stochastic Environment

In a stochastic world immediately after our action we often end up in a belief state where there are more possible states than where we started. However the observation cycle reduces the uncertainty. 

Any plan that works in the deterministic world will maybe work in a stochastic world. However any finite plan has the possibility of having the stochastic nature work against it so we can't have absolute certainty. So far we don't have notation to describe infinite plans.

### Infinite Sequences

[S, R, S] - finite sequence doesn't work in stochastic environment

[S, while A: R, S] - if it's stochastic and independent it should achieve the goal but we can't really bound the steps

### Finding a successful plan

Search in a deterministic environment used a tree structure to represent the states as a result of actions and found the ideal path in the tree to reach the goal state. Trees and search can still be used to find a plan in a stochastic environment, however the trees are more complicated and might have branches that can be represented as a branch back to a previous state.

In order to guarantee a unbounded solution we need every leaf to be a goal state.

In order to guarantee a bounded solution we need to have no loops in the plan.

### Problem solving via mathematical notation

[A, S, F] sequence in a tree. If F is a goal state than this is a "good" plan for reaching the goal.

The mathematical equivalent is Result(Result(A, A->S), S->F) \in Goals

However in a stochastic world:

S' = Result(S, A), b' = Update(Predict(b, a), o)

This starts to get complicated because in even small worlds, belief states can encompass many possible states when enumerating every possible state. This can be improved upon by factoring the world into variables.

## Classical planning. 

Classical planning consists of:

- States:
    - State space consists of all possible assignments for k possible boolean variables (2^k variables)
    - World state consists of a complete assignment to each variable
    - Belief state consists of complete assignments previously, but we can also deal with partial assignments, arbitrary formula
- Action schema:
    - Action(Fly(p, x, y)) Precondition: Plane(p) and Airport(x) and Airport (y) and At(p, x) Effect: not At(p, x) and At (p, y)
    - Schema can be applied to propositional logic as well. The formula syntax resembles first order logic but it can thought of as variables to work with propositional

### Progression search:

Forward or progression state search is to branch different actions on our current state to produce all the child states and branch those child states based on the available actions until we find a goal state.

### Regression Search

Backwards or regression search is starting at the goal state and look at which actions would result in the goal state. Backwards search was possible if there was a single state, however now we can represent a variety of actual states based on the our classical planning representation of state using variables. This can narrow our search because only certain actions result in creating the conditions required by the goal state.

### Regression vs Progression

If the goal is to own book with ISBN 0136042597. In forward search there would be a branching factor of 1,000,000s if we're looking at all the different possible books that we can buy. In backwards search there is only one action schema and that schema can only match in one way that results in matching the goal state

 ### Plan Space Search

 Searching through the space of plans rather than the space of states.

 We start with a plan that goes from start state and finish state. We then add in operators and refine the plan to get to the goal state.

 This method was popular and so was backwards search previously, however currently forward search is popular. This is because we can come up with good heuristics to do a heuristic search. Because forward search deals with concrete plan states it's easier to come up with heuristics.

### Heuristics

In the sliding puzzle we have an action schema like:

```
Action(slide(t, a, b))
    Pre: On(t, a) and Tile(t) and Blank(b) and Adj(a, b)
    Eff: On(t, b) and Blank(a) and not On(t, a) and not Blank(b)
```
And it's possible for a human programmer to look at this and provide good heuristics, but it'a also possible for an AI agent itself to come up with good heuristics simply by relaxing the problem by removing heuristics (i.e. remove requirement where blank(b) Manhattan heuristic or that adj(a, b) misplaced requirement). Another class of heuristics is removing the negative effects.

## Situation Calculus

You can't express the notion of all in propositional languages like classical planning. but you can in first order logic. Situational calculus isn't a new form of logic but uses first order logic is a set of conventions for representing states and objects.

- Actions objects usually as functions i.e. Fly(p, x, y)
- Situations: objects S_0, S' = Result(s, a)
- Instead of describing actions that are applicable to a state as Actions(s), we use a predicate Possible(a, s)

fluents

what changes and what doesn't change

successor state axiom

don't do it for each action do it for a fluent for each predicate

```
for all a, s possible(a, s) then (fluent true iff a made it true or a didn't undo)
```

This ultimately gives situation calculus enough flexibility to describe any first order logic sentence. This is good because we don't need any special software, we already have theorem provers for first order logic so as long as we can express the problem fully in terms of first order logic we can use the theorem prover to solve the problem.
