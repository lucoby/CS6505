# P01 Intro to Algoirthms Design & Analysis

## Sorting Problem

There are several bad algorithms:

- Bogosort: randomly generate permutation and check.
- Bozosort: random swap and check.
- Worstsort: Deterministic but heavily recursive

Sort a list of elements.

A common theme is trade-off of memory and speed.

## Prerequisites:

Asymptotic notation: O(n^2), omega, theta

## Algorithm Design

Generally take the following steps:

1. Describe the algorithm in English
2. Describe the algorithm in pseudocode
3. Prove the algorithm works. Loop invariants is a method for some problems. Note that there is not method to always prove the validity of every algorithm
4. Consider performance (best case, worst case, average case, etc.)

## Insertion Sort

### English

1. Start with a_1
2. Compare current element with all previously sorted elements
3. Move current element left until you reach a smaller element

### Pseudocode

```
def insertion_sort(A):
    for i in range(1, len(A)):
        j = i - 1
        while j >= 0 and A[j] > A[j + 1]:
            A[j - 1], A[j] = A[j], A[j - 1]
```

### Proof of correctness

Invariant: At the beginning of iteration i, the sublist A[0:i] consists of the numbers (A[i], ..., A[i-1]) originally in the first i slots of A, but in sorted order

Base case: A[0:1] = A[0] which is in sorted order.

Iterative step: If the invariant holds for i, then it holds for i+1:

- Before ith iteration: A[0:i] is sorted. After ith iteration: A[0:i+1] is sorted
- A[i] has not been touched before the ith iteration

Focusing on the first invariant requires the invariant for inner loop that, at the start of iteration j, A[0:j+1] and A[j+1:i+1] are sorted.

Assuming this is true, j == -1, or A[j] <= A[j+1]. In the first case A[0:0] is the empty list and can be ignored. In the second case it implies that A[0:j+1], which ends in A[j] and A[j+1:i+1] are sorted and A[0:i+1] is also sorted

### Performance

Number of operations (time) and number of bytes of memory (space). Analysis is relative to input size.

Asymptotic notation so that it's the same regardless of hardware or common overhead.

Assumptions:
- One processor
- RAM
- Usually count +,-,*,/,/,>>,<<, memory access as single ops
- Int and floating-point data types

Input size = usually $ of items in input (e.g. len(A)) sometimes # of bits needed to represent input

Running time = # of ops as function of n, asymptotic notation

Inside the inner most for loop is constant time. The while loop therefore is <= ci for ith iteration. Total ops in the for loop is worost case c/2 (n^2-n) = o(n^2)

#### Additional Analysis

Best case scenario: Input is sorted increasing order. inner loop is theta(1) resulting in theta(n). Best-case: all sorting algorithms are omega(n) because at the very least it must look at every single element.

Worst case scenario: input is sorted in decreasing order. We must do the full ci operations in the ith iteration for the inner loop. This results in 

## Merge Sort

Divide-and-Conquer

### English

1. Split input into two sublists (approx 1/2 in size)
2. Recursively sort the sublists
3. Merge the sorted sublists

### Pseudocode

```
def merge_sort(A,l,r):
	n = r - l
	if n > 1:
		merge_sort(A, l, l + n / 2)
		merge_sort(A, l + n / 2, r)
		merge(A, l, l + n / 2, r)

def merge(A, l, m, r):
	L = A[l:m] + [float('inf')]
	R = A[m:r] + [float('inf')]
	i, j = 0, 0

	while i + j < r - l:
		if L[i] <= R[j]:
			A[l + i + j] = L[i]
			i += 1
		else:
			A[l + i + j] = R[j]
			j += 1
```

### Correctness

#### Merge

S_i,j: Before iteration i,j:

1) A[l:l+i+j] contains the i+j smallest elements of L and R in sorted order
2) L[i] and R[j] are the smallest elements of L R not yet copied back into A

Proof:

Base case:

prove S_0,0: A[l:l] == [] has 0 smallest elements of L & R, sorted

Assume:

1) A[l:l+i+j] contains the i+j smallest elements of L and R in sorted order
2) L[i] and R[j] are the smallest elements of L R not yet copied back into A

Maintenance: S_i,j => S_i+1,j if L[i] <= R[j] or => S_i,j+1 if R[j] < L[i]

WLOG: L[i] <= R[j]

L[i] is the smallest element in L or R not yet copied back to A prior to iteration i,j

A[l:l + i + j + 1] will have the i + j + 1 smallest elements of L and R

Since L was sorted, L[i + 1] will be the smallest element of L not yet copied, and since R was touched R[j] is still the smallest element of R

Termination: S_i,j is true when i + j == r - l - 1 => true at the end of the iteration => true at the end of the loop

Therefore it must be true when i + j == r - l, A[l:l + r - l] == A[l:r] has all the finite elements of L and R in sorted order.

#### Merge sort

Base case: r - l <= 1. In this case the list contains only 1 element which must be sorted

Assumption: Given r - l. Suppose merge_sort works correctly for all n < r - l.

Induction. The recursive calls to merge_sort function correctly based on induction, and the merge combining the two sublists to a single sorted list function correctly (from the previous section). Therefore merge_sort works.

### Analysis

T(n) = theta(1) for n <= 1, 2*T(n/2) + theta(n) for n > 1

Given:
- a equally-sized subproblems
- n/b in size
- c(n) combine time
- D(n) divide time

T(n) = theta(1) for n <= n_0, a*T(n/b) + C(n) + D(n) for n > n_0

Branching tree. The root level that takes T(n) time makes 2 calls that take T(n/2)

This results in d*C leaves where C is constant. d is the depth which is log_2(n). Therefore T(n) = theta(n log n)

## Recap

- Framework
	1. Basic idea English
	2. Implement
	3. Prove Correctness
	4. Analyze for runtime & memory usage
- Loop Invariants
	1. Initialization - prove the invariant holds before the first iteration of the loop
	2. Maintenance - prove that if the invariant holds before iteration a, then it holds for iteration a + 1
	2. Termination - proof that the invariant holds when the loop terminates (usually trivial if the loop doesn't terminate part-way through)
- Divide-and-Conquer
- Analysis of which algorithm is most appropriate given consideration to memory and time
