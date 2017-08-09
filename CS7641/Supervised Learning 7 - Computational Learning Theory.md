# Supervised Learning 7 - Computational Learning Theory

## Introduction

Different learners produce different outputs for classifying data.

> Voronoi diagram diagram that shows the splits in space/plane

### Learning Theory

Three questions or issues:

1. Define learning problems
2. Show specific algorithms work
3. Show these problems are fundamentally hard

Tools for analyzing learning problems are similar to tools for analyzing algorithms in computing.

Some of the resources useful in analyzing learning algorithms are time and space just like normal algorithms. Furthermore, learning algorithms are measured in terms of the number of samples or data they need to be effective.

## Inductive Learning

Inductive learning is learning from examples. There are several useful quantities to define when talking about inductive learning:

- Probability of successful training - (1 - delta)
- Number of examples to train on - m
- Complexity of hypothesis class - Complexity of H. Double edged sword. A class simple hypothesis class can prevent you from expressing truly complex concepts. Whereas a class of complex hypotheses can potentially easily overfit the data requiring a lot of data to truly define the relationships.
- Accuracy to which target concept is approximated - epsilon
- Manner in which training examples presented - two in particular batch (all handed to the learner at once), or online (one at a time)
- Manner in which training examples selected

## Selecting Training Examples

There are several ways to select training examples:
1. Learner ask questions for the teacher - c(x)?
2. Teacher gives examples to help the learner - Teacher  chooses x, tells c(x)
3. From nature (fixed distribution) - x chosen from D and c(x) comes along
4. Evil - worst distribution

### Teaching via 20 questions

If the teacher is trying to teach the student it can ask 1 question to the learner.

If the learner is choosing the questions then the # of questions is roughly log_2|H| assuming you can ask yes/no questions that split the remaining hypotheses in half.

## Learning with Constrained Queries

The 20Q scenario is kind of misleading because in that scenario the teacher is allowed tp ask any question in the universe of questions but it isn't realistic.

A more classical scenario:

X: x_1, x_2, ... x_k (k bit input)
H: conjunctions of literals or negation
Hypothesis: x_1 and x_3 and not x_5

### Reconstructing hypothesis

| x_1 | x_2 | x_3 | x_4 | x_5 | h | 
|-----|-----|-----|-----|-----|---| 
| 1   | 0   | 1   | 1   | 0   | 1 | 
| 0   | 0   | 0   | 1   | 0   | 1 | 
| 1   | 1   | 1   | 1   | 0   | 0 | 
| 1   | 0   | 1   | 0   | 0   | 0 | 
| 1   | 0   | 1   | 1   | 1   | 0 | 

With a nice teacher you need 2 examples to show what's irrelevant. Then 1 example per relevant example (max k). So despite having 3^k hypotheses, only 2 + k examples need to teach a concept. 

### Learner with Constrained Queries

However when the learner is coming up with questions with constrained queries in the classical scenario, it's not possible to ask a question that splits the hypothesis in half. Given a "hard" question i.e. a hypothesis that involves all of the inputs, you'll need approx. 2^k guesses before finding a true statement of the 3^k hypothesis space. Once an input is found, it's easy to derive the actual hypothesis, but to get there you need an exponential number of guesses.

## Learner with Mistake Bounds

A similar scenario:

X: x_1, x_2, ... x_k (k bit input)
H: conjunctions of literals or negation

1. Input arrives
2. Learner guesses answer
3. wrong answer changed
4. Go back to 1

The aim is to bound the total number of mistakes. This gives a lot more power to the learner because it's free to make mistakes. In the previous example for example it can guess false and that would be fine (it doesn't count as not learning)

Algorithm:

1. Assume it's possible each variable is positive and negative
2. Given input, compute output.
3. If wrong, set all positive variables that were 0 to absent, negative variables that were 1 to absent.
4. Goto 2.

In this case there are a max of K mistakes.

## Definitions

Computational complexity: How much computation effort is needed for a learner to converge?
Sample complexity: batch, how many training examples are needed for a learner to create successful hypothesis?
Mistake bounds: Online - how many miss-classifications can a learner make over an infinite run?

Teachers:
Learner chooses
Teacher chooses
Nature chooses
Mean Teacher

Successful hypothesis doesn't necessarily need to mean a 100% accurate hypothesis exists. It simply tries to find the "best" hypothesis in the event that hypothesis space isn't complex enough to represent the concept.

## Version Spaces

True hypothesis: c \in H
Learn from Training: S \subset X, c(x) \forall x \in S
Candidate hypothesis: h \in H
consistent learner: Produces c(x) = h(x) for x \in S in other words it is a learner that is able to produce a hypothesis that is consistent with the data
Version space: VS(S) = {h such that h \in H consistent with S}

> xor is !=

## PAC Learning

### Error of h

Training error: Fraction of training examples misclassified by h
True error: Fraction of examples that would be misclassified on a sample drawn from D

Mathematically:
```tex
error_D(h) = Pr_{x~D}\left [ c(x) \neq h(x) \right ]
```

### Definitions of parameters

C: concept class
L: Learner
H: Hypothesis space
n: |H|, size of hypothesis space
D: distribution over inputs
0 <= \epsilon <= 1/2 error goal
0 <= \delta <= 1/2 certainty goal (1 - \delta)

Since we're drawing our training sample randomly from a distribution D it's possible to get unlucky and have a bad training set that results in higher error but it's okay because the probability of that happening is very low

PAC - Probably approximately correct or (1 - \delta) \epsilon h(x) = c(x)

### Formal definition of PAC Learnable

c is PAC_learnable by L using H iff learner L will, with p robability 1 - \delta, output a hypothesis h \in H such that error_D(h) <= \epsilon in time and samples in 1/\epsilon, 1/\delta, and n

## Epsilon Exhausted

A version space is epsilon exhausted if every hypothesis in the version space has an error less than epsilon. Mathematically:

VS(s) is \epsilon-exhausted iff \forall h \in VS(s) error_d(h) <= \epsilon

## Haussler Theorem - Bound True Error

Let error_D(h_1, ..., h_k \in H) > \epsilon High true error

How much data do we need to remove these hypotheses?

If the error is epsilon, the probability of match is 1 - \epsilon and that's a relatively low probability:

Pr_{x~D} (h-i(x) = c(x)) <= 1 - \epsilon

Across multiple (independent and random) samples the probability the hypothesis is consistent is:

Pr(h_i consistent with c on m examples) <= (1-\epsilon)^m

So above applies to one hypothesis. The probability that at least one of the bad hypothesis remains in the version space is (think of h_1 or h_2 or ... h_k and "or" is addition of probabilities k times)

Pr(at least one of h_1, ..., h_k consistent with c on m examples) <= k(1 - \epsilon)^m <= |H|(1 - \epsilon)^m

So using the fact that -\epsilon >= ln(1 - \epsilon)

> Calculus... derivative of ln(1 - x) is 1/(1 - x) which at 0 is -1, the same as the derivative of -x. But it decreases meaning that ln(1 - x) "falls faster" than -x

Multiply both sides by m and raise to e to the power gives:

(1 - \epsilon)^m <= e^(-\epsilon m)

So:

Pr(at least one of h_1, ..., h_k consistent with c on m examples) <= k(1 - \epsilon)^m <= |H|(1 - \epsilon)^m <= |H|e^(-\epsilon m) <= \delta

ln|H| - \epsilon m <= ln \delta, m >= 1/\epsilon(ln|H| + ln(1/\delta))

So for example
H={h_i(x) = x_i}
x is 10 bits
\epsilon: 0.1
\delta: 0.2
D: uniform

m>= 1/.1 (ln(10) + ln(1/.2)) >= 39.12  or 40 samples

## Summary

- Teachers and students (learners) and the interaction
  - What is learnable? like complexity theory for Machine Learning
- Sample complexity - data
- Types of interactions
  - Learners picks questions
  - teacher picks questions (very helpful)
  - nature picks questions (unfeeling / oblivious)
- mistake bounds
- PAC learning: version spaces, training / test / true error, distribution, epsilon-exhausted, sample complexity bound
  - m >= 1/\epsilon(ln|H| + ln(1/\delta))
  - If the true hypothesis isn't in the version space it's "agnostic" and needs to get the "best" in its own version space. Similar form and still polynomial but not as good
  - Bound depends on the hypothesis space which means if you have infinite hypothesis space this bound doesn't apply