# 9 Dynamic Programming

## Introduction to algorithms

First there was the general notion of a language. Then this was distinguished between undecidable and decidable languages. There are uncountable many undecidable languages but countably many decidable ones.

Withing the decidable languages there are P (verified in polynomial time), NP, and NPC (NP-complete the hardest types of problems) languages.

## Optimal Similar Substructure

This refers to the idea that if you had the answer to a similar, smaller problem, it would be much easier to solve a larger, more complex, hard problem. This is similar to recursion. However, in recursion, if the same problem needs to be solved multiple times throughout then there can be repeated computation. 

For example, in Fibonacci F_n = F_n-1 + F_n-2 and F_n-1 = F_n-2 + F_n-3. So, a recursive solution would calculated F_n-2 twice. In, fact the F_n-k would be called F_k times in calculating F_n.

Two strategies to deal with re computation:

1. Memo-ization of subproblems - after we solve a sub problem, we write a memo with the solution. Before we solve a problem, we check to see if the problem has been solved.
2. Do the subproblems in the right order - ensure that the problems are done in order such that the solution to the subproblem already exists.

## Edit Distance Problem

The problem of sequence alignment or edit distance. Minimize the number of edits that needed to occur to transform one sequence to another considering a cost function.

Consider sequences X = (X_1, ..., X_n) and Y = (Y_1, ..., Y_N) over an alphabet Sigma.

An Alignment is a subset A \subset {1...m} x {1...n} such that for any (i, j) and (i', j): i != i' j!= j', i < i' => j < j'.

## Sequence Alignment Cost

Cost(A) = # of unmatched characters + cost of matching 2 characters

# of unmatched characters: (n + m - 2|A|)

cost of matching 2 characters: \sum alpha(x_i, y_j), typically alpha = 0 when x_i = y_j

## Prefix Substructure

The key optimal substructure for the sequence alignment problem comes from aligning prefixes of the sequence that we want to align.

Let c(i, j) be the minimum cost of aligning x_1, ..., x_i with y_1, ..., y_j. Goal is to find c(m, n).

case 1: match the last two characters - c(m-1, n-1) + alpha(X_m, Y_n)

case 2: leave x_m unmatched - c(m-1, n) + 1

case 3: leave y_n unmatched - c(m, n-1) + 1

c(m, n) = min({c(m-1, n-1) + alpha(X_m, Y_n),  c(m-1, n) + 1, c(m, n-1) + 1})

base case c(0, j) = j; c(i, 0) = i

## Sequence Alignment Algorithm

Fill in an m x n array representing the cost. Each entry in the array is determined based on the recurrence relationship. The alignment can be determined by tracing back the entries that lead to the minimum cost.

## Sequence Alignment Summary

Theorem: There is an algorithm which given two sequences of lengths m and n, finds the minimum cost alignment in O(mn) time.

The optimal similar substructure is the minimum cost of aligning prefixes

The recurrence is c(m, n) = min({c(m-1, n-1) + alpha(X_m, Y_n),  c(m-1, n) + 1, c(m, n-1) + 1})

## Chain Matrix Multiplication

Given matricies A_1 ... A_n of sizes m_0 x m_1, m1 x m_2, ..., m_n-1 x m_n, compute the product A_1 ... A_n efficiently. Recall that multiplying an m x n and a n x p matric results in an m x p matrix and requires approximately mnp operations. Also recall that due to associative rule so (AB)C = A(BC). While the product is the same, the number of operations is not the same.

## Subchain Substructure

Let c(i, j) be the minimum cost of multiplying A_i...A_j.

C(i, j) = min_{i<=k<=j} (C(i, k) + C(k+1, j) + m_{j-1} m_k m_j)

## CMM Algorithm

The resulting problem can be representing as a triangular array as there are C(n,2) sub problems from splitting the matrices. The base case is along the diagonal as 0 operations are required when multiplying any matrix with itself. The end goal i s to find the top right entry. In general any entry requires knowing everything to the left and below it.

```
for i = 1 to n:
	c(i,i) = 0

for s = 1 to n-1:
	for i = 1 to n - s:
		j = i + s
		c(i, j) = min_{i<=k<=j} (C(i, k) + C(k+1, j) + m_{j-1} m_k m_j)

return c(1,n)
```

## CNN Summary

Theorem: There is an algorithm that gives the dimensions of n matrices gives the expression tree that requires the fewest scalr multiplication in O(n^3) time.

The optimal similar substructure is the minimum cost of sub chains.

The recurrence is C(i, j) = min_{i<=k<=j} (C(i, k) + C(k+1, j) + m_{j-1} m_k m_j)

## All Pairs Shortest Path

Given a graph G = (V, E) and w: E -> R find a shortest path between u and v for all (u, v) \in V x V. i.e. minimix \sum_{e \in P} w(e)

Recall that for single source, Dijstra's algorith O(|V| log|V| + |E|) (Requires that w >= 0)

Bellman-Ford O(|V||E|)

## Shortest Path Substructure

Subpaths of shortest paths are shortest paths. However this isn't immediately useful. i.e. solving for the shortest path from u to a neighbor of v to solve the shortest path from u to v can result in circular dependencies.

We can refine the to recursing on path length. If we know the shortest path of length k-1 to a neighbor of v, then we know the shortest path of length k to v.

Let delta(u, v, k) be the weight of the shortest path that uses at most k edges.

delta(u, v, k) = min{delta(u, v, k-1), min(delta(u,x,k-1) + w(x, v))}

Yields the O(|V|^3 log|v|) matrix multiplication algorithm.

An alternative is to recurse on potential intermediate vertices. WLOG assume V = {1, ..., n}

Consider solving the problem without N and then allowing N to be used. The shortest path will either be the original shortest path, or a new shortest path that goes through N.

## The Floyd-Warshall Algorithm

```
for i = 1 to n:
	for j = 1 to n:
		if (i,j) in E:
			d[i][j] = w(i,j)
			path[i][j] = i
		else if i = j:
			d[i][j] = 0
			path[i][j] = null
		else:
			d[i][j] = inf
			path[i][j] = null

for k = 1 to n
	for i = 1 to n
		for j = 1 to n
			d[i][j] = min{d[i][j], d[i][k] + d[k][j]}
			if d[i][j] > d[i][k] + d[k][j]:
				path[i][j] = path[i][k] + path[k][j]
```

## All Pairs Summary

Floyd-Warshall algorithm finds the shortest path in a weighted graph for all pairs of verticies in O(|v|^3) time via dynamic programming

The optimal similar substructure involved finding optimal paths via a restricted set of verticies 1..k

Recurrence: d[i][j] = min{d[i][j], d[i][k] + d[k][j]}

## Transitive Closure

Consider a relation R over a set A i.e. R subset of AxA.

The relation is really a set of ordered pairs. Which can be thought of as a directed graph. This can lead to inferences as a result of any direct path that runs from one point to another through other paths. 

Running Floyd-Warshall with existing ordered pairs = 1 and everything else as inf will lead to a solution (all resulting weighted paths represent a relationship). This can be optimized by using boolean true/false instead of ints + inf.

## Conclusion

Dynamic programming is a good strategy to use. If there is a recursive solution, think of if there is a repetitive part to the recursion that can be used.

Dynamic programming won't work for every problem (create a poly time solution to any recursive problem). There is the satisfiability of an equation problem that has recursive elements but not enough overlap to make use of DP.
