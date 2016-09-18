# P03 Probabilistic Analysis

Expected-case / average-case
Randomized algorithms

Prereqs:

- Random variables
- Expected value
- Independence

## Motivation: "Hiring problem"

c_i = cost to interview (small)
c_h = cost to hire (large)

If you you hire, the current employee is fired. While interviewing you continue with the hired employee. If you reject an employee, they are gone forever. You goal is to hire the best candidate, and the goal of the problem is to estimate the cost of hiring the best candidate.

Unlike other algorithm design where we're creating an algo, here, we're given the algorithm: Continue interviewing. When we find a candidate better than the current, fire the current and hire the new candidate.

```
def fire_best(A): 
	# A is a list of n candidates
	# A[i].interview() returns ith candidates qualifications
	# A[i].hire() hires ith candidate and fires current employee if one exists

	best_q = -1


	for i in range(0,n):
		this_q = A[i].interview()
		if this_q > best_q:
			A[i].hire()
			best_q = this_q
```

c_i = cost of calling interview

c_h = cost of calling hire

C(n) = n*c_i + X*c_h

where X = # of hired candidates

Worse case: A is sorted in order of increasing qualification resulting in n calls to hire(). This occurs 1/n! of the time.

Best case: A is sorted such that most qualified candidate comes first resulting in 1 calls to hire(). This occurs 1/n of the time.

What's the average case (considering the differing probabilities)?

rank(i) = candidate (i) candidate with the ith lowest qualification) i.e. rank(n) = best candidate), rank(1) = worst candidate.

Assume a uniform distribution (every ordering has an equal probability of occurring. P(ordering) = 1/n!). Note that the uniform assumption isn't necessary for the analysis, but without evidence of some bias, it makes sense to assume uniform distribution.

E[C(n)] = E[n*c_i + X*c_h] = n*c_i + c_h*E(x)

decomposition into indicator random variables

An indicator random variable is 1 if A occurs and 0 if A doesn't occur.

X = \sum_{i=0}^{n-1} 1_A_i, where A_i = candidate i is hired.

E(X) = \sum_{i=0}^{n-1} E(1_A_i)

E(1_A_i) = 1*P(A)

What is P(A_i)?

- Candidate 0 is hired always P(A_0) = 1
- Candidate 1 is hired if it is better than candidate 0, P(A_1) = 1/2
- Candidate 2 is hired if it is better than candidate 0 and 1, P(A_2) = 1/3

It can be seen that if each ordering is equally likely, then exactly 1/(i+1) ordering will have a given candidate be the best among the first i+1 candidates. This implies the cost of hiring is:

\sum_{i=0}^{n-1} P(A_i) = \sum_{i=0}^{n-1} 1/(i + 1) = theta (log n)