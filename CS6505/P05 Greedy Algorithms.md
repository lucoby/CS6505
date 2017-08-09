# Greedy Algorithms

Sometimes things like dynamic programming are overkill.

Greedy algorithms take the locally optimal "path" to the solution that looks most efficient currently and "hopes" that it leads to the globally optimal solution. This doesn't always happen for all problems.

## Objectives

- Design a greedy algorithm
- Prove that problem demonstrates greedy-choice and optimal substructure
- Demonstrate that a problem can be reframed as a matroid problem, and doing so would give an almost automatic solution as a greedy algorithm

## Activity Selection Problem

This is the problem of scheduling several competing activities that require exclusive use of common resource.

S = {a_0, ..., a_n-1} activities

Each activity, a_i, has a start time and end time, s_i and f_i.

a_i and a_j are compatible if their time windows do not overlap:

- iff [s_i, f_i) intersect [s_j, f_j) is empty (disjoint)
- iff s_i >= f_j or s_j >= f_i

Goal: select a maximum-size set of mutually compatible activities

Assume activities are ordered in terms of finish times

### Dynamic programming solution

Optimal substructure:

let s_ij denote the set of activities starting after f_i and finishing at or before s_j

Let A_ij = maximum sized set of mutually compatible activities in s_ij

Assume activity a_k is in A_ij. Define A_ik = A_ij intersect S_ik and A_kj = A_ij intersect S_kj

so, A_ij = A_ik union {a_k} union A_kj and |A_ij| = |A_ik| + |A_kj| + 1

This leads to the recurrence relationship because finding A_ik and A_kj leads to A_ij. Use an array c[i][j] == |A_ij|.

c[i][j] = 0 or max_{k in S_ij} (c[i][k] + c[k][j] + 1)

### Greedy solution

Realize that given A_ik, the a_k that is a part of A_ij will be the a_k that has the earliest end time but starts after the elements in A_ik have ended.

```
def select_activities(S):
	\\ assumming S is sorted in order of finishing time
	n = len(s)
	A = {S[0]}
	prev_finished = 0

	for i in range (1,n):
		if S[i].s >= S[prev_finished].f:
			A = A.append(s[i])
			prev_finished = i

	return A
```

### Proof

Let S_k be the set of activities starting after f_k and A_k is the max sized subset of mutually compatible activities in S_k. 

Let a_l be the activity in S_k with the earliest finish time. Let a_j be the activity in A_k with the earliest finish time. Either a_k is a_l and the solution satisfies the greedy property, or a_l can be substituted for a_j because it won't compromise compatibility with any activities in A_k (because it ends before a_j) and it doesn't conflict with anything from before because it is still a part of S_k.

## Designing greedy algorithms

1. Rephrase the problem in terms of making a choice and solving a single subproblem
2. Prove that there is an optimal solution to the problem that involves the greedy choice (greedy-choice property)
3. Prove the optimal substructure property, that the greedy choice combine with the optimal substructure solution leads to the optimal solution

## Fractional knapsack problem

n items: S = {a_0, ..., a_n-1} with value v_i, and weight w_i

The goal is to take the most valuable set of items with total weight less than W. Unlike normal knapsack, 0-1 knapsack, you can take a fractional portion of the items that will have fractional value and weight to the original value and weight.

### Rephrase in greedy choice

Rephrase in terms of Greedy Choice q_q = v_i / w_i. 

The greedy choice is as much of the a_i as possible where a_i has the highest q_i.

Subproblem W - w_i, {a_i, ..., a_i-1, a_i+1, ..., a_n-1}

```
def frac_knapsac(S, W):
	\\ assume sorted in decreasing order of q value
	n = len(s)
	amt = [0] * n
	wt_rem = W

	for i in range(i,n):
		amt[i] = min(wt_rem, s[i].w)
		wt_rem -= amt[i]
	
	return amt
```

### Prove the greedy choice

There is an optimal solution to (S,W) such that min(w, w_0) (= m_0) of a_0 is chosen.

Suppose we only select weight m of a_0, then exchange m_0 - m of any other item in the bag for M_0 - m of a_0. Then the total value changes by  q_o * (m_0 - m) - q_q * (m_0 - m) > 0. Thus any solution not containing m_0 of a_0

### Optimal Substructure property

Taking m_0 = min(w, w_0) of a_0 and then taking the optimal solution to (s', w') gives an optimal solution to (s, w) where s' = {a_1, ..., a_n-1} and w' = w - m_0

let V = optimal value for (s', w'), then V + q_o * m_0 = value of combining the greedy choice with optimal solution to (s', w'). Suppose there was some V' > V + q_o * m_0. However this would imply there is some solution V' - q_o * m_0  that is > V. This contradicts the original assumption which implies the greedy choice leads to the optimal solution.

## Matroids

M = (S,\mathcal{I}), note S is finite set and I is a collection of subsets of S that are "independent"

several properties

1. I is nonempty.
2. I is hereditary. i.e. B in I and A is a subset of B then A is in I. Not this also implies that the empty set must be independent.
3. exchange property: if A, B in I, and |A| < |B| then there exists some element x in B - A such that A union {x} is independent.

Matroids derive from usage with matrices and linear independence among rows of a matrix, but don't necessarily need to be used in that context.

### Graphic matroid

Graph (V,E)
S = E
I = {A subset of E such that A is acyclic} 

Note: that acyclic implies a forest, and a forest is just a set of trees.

Claim: M_G = (S_G, I_G) is a matroid often called the graphic matroid. In order to check that it is matroid test to see that it satisfies the properties of a matroid.

- Set of edges is finite
- Empty set of edges is trivially acyclic
- Removing edges from an acyclic graph cannot create a cycle.
- Suppose A, B are acyclic, and |B| > |A|
	- Lemma: A forest F = (V_F, E_F) contains exactly |V_F| - |E_F| connected components. This follows from the fact that a tree with v verticies has v-1 edges. Suppose F has t connected components and connected component i has v_i verticies and e_i edges. Then |E_F| = sum e_i = sum (v_1 - 1) = |V_F| - t. Therefore t = |V_F| - |E_F|.
	- So G_A has |V| - |A| connected components and G_B has |V| - |B| connected components and |V| - |B| < |V| - |A|. By pigeon hole principle, at least one of B's connecteed components has to touch 2 or more connected components of A. Because these two are acyclic, there is an edge from B that can be added to connect the two acyclic components of A without creating a cycle.

### Terminology

M = (S, I), A in I then x (not in A) is an extension of A iff A union {x} in I. Also, {x} in I then x is an extension of the empty set.

A in I is maximal iff it has no extension. All maximal independent sets in S have the same size. If there was a maximal set, B, that was larger,  then by the exchange propery, a component from B could be added to A to make it bigger.

Note: spanning trees are maximal in the graphic matroid

A matroid M = (S, I) is weighted if w: S -> R+ and w:2^S -> R >= 0 is defined by w(A) = \sum_{x in A} w(x).

#### Minimum spanning tree of a matroid

Goal (as far as greedy algorithms): find a subset A in I such that w(A) is large as possible. i.e. an optimal independent subset (will be a maximal size independent subset)

```
def greedy_matroid(M, w): # M.S is a list and M.I is a set data structure
	A = {}
	n = len (M.S)
	sort M.S in decreasing order by w(S[i])

	for i in range(0, n):
		x = S[i]
		if A union {x} is in M.I:
			A = A union {x}

	return A
```

#### Proof of correctness

Since empty set is independent, and because the for-loop has the invariant that A is independent, the algorithm returns an independent set.

Greedy choice: Let x be the 1st element of S such that {x} in I. If such an x exists, there is an optimal set of S containing x (otherwise the empty set is optimal)

Proof: Assume such an x exists. Let B be an optimal set in S. Assume x is not in B. Assume no element of B has a greater weight than x (if there was an element y in B such that w(y) > w(x) then {y} is interdependent, because of hereditary, and this would lead to a contradiction as we assumed x is the largest independent element)

Construct A. Let A = {x} in I. Use exchange property to augment A until |A| = |B| and A in I. Let z be the remaining element in B. w(A) = w(x) + w(B) - w(z) >= w(B). Therefore A is optimal.

OSS: Let x be the 1st element chosen. Then subproblem of finding optimal subset containing x reduces to finding optimal subset of S' in the weighted matroid (S', I') where:

- S' = {Y in s such that extending {x, y} remains independent i.e. that {x, y} in I}
- I' is all subsets of S not containing such that adding x to B would be a part of the original independent set. i.e. that {B subset S - {x} such that B union {x} is independent} (note: this is called contraction of M by x)

Proof: if A is an optimal subset of S containing x, then A' = A - {x} is in I'. Conversely, any set A' in I' yields an independent set A = A' union {x} that is in I. In both cases, w(A) = w(x) + w (A'). Use contradiction to show that suppose there is an optimal subset other than A' of M', but that leads to a contradiction that one of the elements has a weight > w(x).

Claim: If x in S is not an extension of the empty set, then x is not an extension of any independent subset of S.

Proof if x is extension of A in I, then A union {x} is in I, so {x} is in I therefore x would be an extension of the empty set.

#### Analysis of runtime

sort is O(n log n)

determining if A is independent in the for loop can take varying time depending on the application (can be thought of as f(n)). Therefore overall run time is O(n log n + n f(n))

## Task scheduling problem

Demonstrates taking a problem and showing it can be framed as a matroid.

S = {a_0, ..., a_n-1} unit time tasks

a schedule is a permutation of S that represents order tasks need to be completed (all tasks finish by n)

a_i has deadline 1<= d_i <= n and a penalty p_i > 0 for failing to complete a_i before d_i.

Goal: come up with a schedule that minimizes penalty.

Fact: a schedule can be arranged into a "cannonical" form with all "on-time" tasks preceding all late t asks and with all on-time tasks sorted in increasing order by deadline and the penalty won't change. So this allows the problem to start to look more like a matroid, we're picking the tasks that can be ontime or will be late.

A subset S is independent if they can be scheduled so that non of the tasks are late. Let I = the independent sets of tasks

Lemma: For t = 0, ..., n, let N_t(A) be the number of tasks in A whose deadline is t or earlier. The following are equivalent:
- A is independent
- N_t(A) <= t for all t
- If tasks scheduled in increasing order by d_i, no task is late

Claim: (S,I) is a matroid.

- empty set is trivially independent
- if you can schedule a set of tasks and remove one from those scheduled then it's still independent.
- exchange: Let A, B in I and |B| > |A|. Define k = largest t such that N_t(B) <= N_t(A). This must exist because N_0(B) = 0 = N_0(A)  but at n, N_n(B) = |B| > |A| = N_n(A). At time k, N_k(B) <= N_k(A) but N_{k+1}(B) > N_{k+1}(A), so there must be more tasks with deadline k+1 in B than A. Pick x in B and not in A with deadline k+1. We can show that A union {x} in I.
	- For all t <= k, N_t(A') = N_t(A) <= t. For T > k, N_t(A') = N_t(A) + 1 <= N_t(B) <= t. So for all t, N_t(A') <= t, therefore A' in I

w(a_i) = p_i for all a_i in A. The run time is O(n log n + n^2).

## Recap

- Frame as choice + similar subproblem
- Greedy-choice property
- optimal-substructure property
- cast problems as weighted matroids to solve using a general greedy algorithm
- show that a problem can be cast as a matroid by:
	- show some set is independent
	- independence is hereditary
	- exchange property
