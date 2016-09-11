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