# Divide and Conquer

1. Divide
2. Conquer
3. Combine

## Maximum Subarray Problem

Given predictions of prices of a share across set of dates. The goal is to maximize the profit buy buying and selling once over the set of dates.

The naive / greedy strategy is to either fix the sell date at the max price date or the buy date at the min price date, however this doesn't always work.

The brute force solution is to consider every combination of buy and sell dates. This requires us to compare C(n,2) dates which is theta(n^2)

### Divide and conquer with days difference

#### English

List of daily changes O(n) to compute. Finding the buy and sell date is equivalent to finding the subarray where the sum of the subarray is the max sum.

A[l:r] is split into A[l:m] and A[m:r]. The max sum sub-array can be will either be entirely in the left partition, the right partition or cross over the midpoint. If it is entirely in the left or right partition then that can be accomplished through recursion. The calculation of the cross-over case can be accomplished in O(n).

#### Pseudo code

```
def max_subarray(A, l, r):
	if r - l == 1:
		return(l, r, A[l])
	m = (l + r) / 2
	(l1, r1, s1) = max_subarray(A, l ,m)
	(l2, r2, s2) = max_subarray(A, m, r)
	(l3, r3, s3) = max_crossing_subarray(A, l, m, r)

	return (li, ri, si) such that si == max(s1, s2, s3)
```

#### Proof

Base case: if there is only one element then the element is the subarray that has the largest sum

Inductive step:
Suppose max_subarray(A, l, r) returns the max subarray whenever r - l <= k and suppose that r-l = k + 1. Then, A[l:m] and A[m:r] are subarrays of length <= k, whenever k>=2, and if k = 1 the base case applies instead. max_crossing_subarray(A, L, m, r) returns the max subarray not entirely contained in A[l:m] or A[m:r]. Since all subarrays of A[l:r] fall in one of these three categories, the max subarray will be returned.

#### Performance

T(n) = {theta(1) if n = 1, 2T(n/2) + theta(n) for n>1}

Therefore T(n) = theta(n log n) for time

For space theta(1) needed per call. Only need memory as far as the recursive call therefore theta(log n) needed in total.

## Integer Multiplication

Typically we assume operations are O(n) but here we're looking at the bit level so n is the number of bits. For addition we're looking at O(n) to add two integers together.

The grade school algorithm involves multiplying each of the n digits of one number by the n bits of the other number which results in o(n^2) operations. Adding the n numbers together requires o(n), but this is dominated by the o(n^2) operations.

### Karatsuba's algorithm

Assume x, y each n bits. The algorithm does 3 multiplications of half size of the original.

x = x_1 * 2^(n/2) + x_0
y = y_1*2(n/2) + y_0

x_0, y_0 = remainders after dividing 2^*n/2) which is equivalent to >> (n/2) which can be done in O(n) time

```
x * y 	= (x_1 * 2^(n/2) + x_0)(y_1 * 2^(n/2) + y_0) 
		= x_1 * y_1 * 2^(n) + (x_1 * y_0 + x_0 * y_1) * 2^(n/2) + x_0 * y_0

z_2 = x_1 * y+1
z_1 = x_1 * y_0 + x_0 * y_1 = (x_1 + x_0) * (y_1 + y_0) - z_2 - z_0
z_0 = x_0 * y_0
```

The multiplications by 2^(n) or 2^(n/2) are equivalent to bit shifts so are o(n).

This results in a recurrance relationship

T(n) = {theta(1) for n < 4, 3T(n/2) + theta(n) for n>= 4)

#### Pseudocode

```
def karatsuba(x, y):
	n = max(xize(x), size(y))
	if n < 3:
		return grade_school(x, y)

	x1 = x >> (n >> 1)
	x0 = x % (1 << (n >> 1))
	y1 = y >> (n >> 1)
	y0 = y % (1 << (n >> 1))
	a, b = x0 + x1, y0 + y1

	z2 = karatsuba(x1, y1)
	z0 = karatsuba(x0, y0)
	z1 = karatsuba(a, b) - z0 - z2
	p1 = z2 << n
	p2 = z1 << (n >> 1)
	return p2 + p1 + z0
```

#### Correctness

Inductive proof again using strong induction because recursion doesn't use k-1 step

#### Performance

At each level d there are 3^d terms of Cn/2^d work

sum_{d=0}^{log_2n} Cn (3/2)^d = theta(n^log_2 3)

## Master Theorem

Theorem: Let a >= 1, b > 1 be a constant, f(n) an eventually nonnegative function, and T(n) be defined by:

T(n) = {Theta(1), n <= n_0, aT(n/b) + f(n), n > n_0

Then if:

1. f(n) = O(n^((log_b a) - epsilon)) for some epsilong > 0, then T(n) = theta(n^log_b a)
2. f(n) = theta(n^log_b a) T(n) = theta(n^(log_b a) log n)
3. f(n) = omega(n^(log_ba) + epsilon) for some epsilon > 0, and a*f(n/b) <= c f(n) for some c < 1 and large enough n, then T(n) = theta(f(n))

### Karatsuba:

T(n) = {theta(1), for n <= 3, 3T(n/2) + c*n, n > 3

Since, c*n => n^1 = O(n^(log_2(3) - epsilon)) => T(n) = theta(n^log_2(3))

### Merge Sort

T(n) = {theta(1), for n <= 1, 2T(n/2) + c*n, n > 1

since C*n = theta(n^log_2 n) = theta(n)

=> T(n) = theta(n log n)

## Proof of Master Theorem

T(n) = {Theta(1), n <= n_0, aT(n/b) + f(n), n > n_0

1. Use assumption that n = b^k for k >= 0

n = b^i for i >= 1

Claim 1: Let a>= 1, b > 1, f(n) non negative defined on b^i, i >= 1 and define T(n) as above

T(n) = theta(n^(log_b a)) + sume_{d=0}^{log_b n) - 1} a^d f(n/b^d)
