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

