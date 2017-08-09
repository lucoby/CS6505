# Lesson 5 - Probability

## Probability

Probability basics:

- Sum of probabilities is 1
- If you have a binary event a probability of an event is p then the probability of event not occurring is 1 - p
- If you have multiple events that are independent, the probability of all of the events occurring is the probability of the probabilities of individual events -  P(x)P(y) = P(x,y) - Joint probability is the product off marginal probabilities
- The probability that one of several outcomes occurs is the sum of the probabilities of each individual outcome

## Dependence

P(Y) = \sum_i{P(Y|X = i)}P(X = i)
P(not X | Y) = 1 - P(X|Y)

## Bayes Rule

P(A|B) = P(B|A) P(A) / P(B)

- P(B|A) is the likelihood
- P(A) is the prior
- P(B) is the marginal likelihood
- P(A|B) is the posterior

P(B) is often expanded as P(B) = \sum_a {P(B|A = a) P(A = a)}
