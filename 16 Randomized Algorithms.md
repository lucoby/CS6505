# 16 Randomized Algorithms

For completeness it could be worth to return to Turing machines and show that there is an equivalence. However, for simplicity assume the built in functions work. (Assume, because in reality they produce pseudo-random numbers).

## Verifying polynomial identities:

Input: Representations of polynomial A(x) and B(x) having degree d.

Goal: Decide if A(x) === B(x)

```
def polyequal(A, B, d):
	x = randint(1, 100*d) # Return random in in {1, ..., 100*d}
	return A(x) == B(x)
```

Unless A(x) - B(x) = 0, it can only have d roots. The "bad" case is if A and B are different and has all d roots in the range of 1, 100*d. In that case there is a 1/100 probability that we say they are the same.

## Discrete probability spaces

1. A sample space, omega, that is finite or countably infinite.
2. A probability function such that
	1. For any event in the sample space, 0 <= Pr(E) <= 1
	2. Pr(omega) = 1
	c. For any pairwise disjoint E_1, E_2, ..., Pr(union_{i>=1} E_i) = sum_{i>=1} Pr(E_i)

## Repeated Trials

1 method to improving the algorithm is increase the range (1,100*d). This may stretch the bounds of computing to appropriately decrease the probability of matching. An alternative method is to do repeated trials:

```
def polyequal(A, B, d):
	equal = True
	for i in 1..k:
		x = randint(1, 100*d) # Return random in in {1, ..., 100*d}
		equal = equal and A(x) == B(x)
	return equal
```

For k = 2, |omega| = (100d)^2. At most d^1 of these make polyequal fail.

## Independence and conditional probability

Def: The conditional probability of event E occurs given event F occurs is Pr(E|F) = Pr (E and F) / Pr(f)

A case of conditional probability is Pr(E|F) = Pr(E). In this case, E and F are independent. Or, more generally,

Def: Events E and F are independent if Pr(E and F) = Pr(E) * Pr(F)

note definition is more general as it is defined when Pr(F) = 0.

## Monte Carlo and Las Vegas

For the repeated trials polyequal:

* If A === B then PR[polyequal(A,B,d)] = 1
* If A !=== B then Pr[polyequal(A,B,d)] <= 100^-k

Because our algorithm can sometimes give an incorrect answer, it is called a "Monte Carlo Algorithm". And, because it only makes mistakes when the polynomials differ, it is called a "one-sided monte carlo algorithm"

These concepts can be extended to arbitrary languages.

Now consider if any error is unacceptable. It's still possible to use randomization!

```
def polyequal(A, B, d):
	order = sample(range(1, 100*d), d + 1) # d + 1 samples without replacement
	for x in order:
		if A(x) ~= B(x):
			return False
	return True
```

Now:

* If A === B then PR[polyequal(A,B,d)] = 1
* If A !=== B then Pr[polyequal(A,B,d)] = 0
 
 The fact the algorithm never returns an incorrect answer makes it a "Las Vegas Algorithm". The randomization can affect the running time, but it cannot affect the algorithm.

## Random Variables

Def: A random variable on a sample space omega is a function X: omega -> R. 

Example: Let X be the sum of two die throws. Then the sample space is Omega: {1, ..., 6} x {1, ..., 6} and X((i, j)) = i + j

Notation: (X = a) = {s in Omega | X(s) = a}

## Expectation

Def: The expectation of a random variable X is given by E[X] = \sum_{v \in X(Omega)} v * Pr(X = v) where X(Omega) is the  image X

Thmrm: Linearity of Expectation - For any random variables X and Y and constant a, E[X + Y] = E[X] + E[Y] and E[a*X] = a*E[X]

## Quicksort

Suppose quicksort is used to sort a list consisting of elements a_1 < a_2 < .. < a_n. Let E_ijk be the event that a_i is separated by a_j for the kth choice of a pivot. Let X_ij = 1 if the algorithm compares a_i with a_j and X_ij = 0 otherwise.

sum_{forall i, j} x_ij counts the number of comparisons used.

Claim: E[X_ij] = 2/(j - i + 1)

Proof: E[X_ij] = 1 * PR(x_ij=1)
= sum_k (pr(x_ij = 1 and E_ijk)
= sum_{k:Pr(E_ijk) > 0} PR(x_ij = 1| E_ijk) * Pr(E_ijk)

PR(x_ij = 1| E_ijk) = 2/(j - i + 1)

Therefore E[X_ij] = 2/(j - i + 1) because sum_{k:Pr(E_ijk) > 0} Pr(E_ijk) = 1

sum_{for all i< j} E[X_ij] = sum_{for all i< j} 2/(j - i + 1) <= sum_{i=1}^n sum_{j=1}^n 2/j = O(n log n)

# A minimum cut algorithm

Given: A connected graph G = (V, E) (S is a minimum"cut-set")

Output: A set S of minimum size such that (V, E - S) has two connected components.

```
def randMinCutSet(v, e):
	while |v| > 2:
		e = choice(E)
		(v, e) = contractEdge(V,E,e)
	return E
```

Theorem: The algorithm randMinCutSet outputs a min-cut set with probability at least 1 / (n(n-1))

Corollary: THe smallest set of those returned by n(n-1) / 2 ln(1/epsilon) calls to randMinCutSet is a minimum cut-set with probability 1 - epsilon.

Proof: Each call is independent so the probability that all fail is at most (1 - 2/(n(n - 1)))^(n(n-1)/2*ln(1/epsilon)). Using (1 - x) <= e^(-x) gives exp(-ln(1/epsilon)) = epsilon

Let C be a minimum cut set. Let E_i be the event that the edge chosen in iteration i is not in C. Then 

Pr(intersect_{i=1}^{n-2} E_i) = Pr(E_n-2 | intersect_{i=1}^{n-3} E_i) ... Pr(E_2 | E_1) Pr(E_1)

Claim: Pr(E_j+1 | intersect_{i=1}^{j} E_i) >= (n - j - 2) / (n - j)

Warmup:
