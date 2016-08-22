# Languages & Countability

## Functions

A function is a mapping from domain to range where each element of the domain is paired with exactly one element of the range.

This is a broader definition than the naive interpretation that a function is a series of computable steps to get from domain to range i.e. f(x) = 3x + 2

## Rules of the Game

- Alphabet - a finite set of symbols
- String - A finite sequence of symbols for an alphabet, or an empty set
- Output - sometimes an output is a string. Typically it is a binary output like an accept/reject.
- Language - a set of strings


## Operations of Languages

- Usual set operations, union, intersect, compliment
    - Compliment the set of all strings over the relevant alphabet that are not in the language
- Concatenation of 2 languages
    - Concatenation to the same language is A^2, and in general concatenation k times is A^k
- Kleen Star and Plus - (like regex)
    - A* = Union_i=0^inf A^i
    - A^+ = Union_i=1^inf A^i

## Countability

A set is countable if it is finite or there is a one-to-one correspondence with the natural numbers.

For any finite alphabet sigma, the set of strings, sigma*, is countable.

A countable union of countable sets is countable.

## Languages are uncountable

The set of languages over a finite alphabet is uncountable

Proof:

Suppose not. Let L_1, L2, ... be the set of languages over the alphabet. Let x_1, x_2, ..., be the set of strings in sigma*. Build a table where the columns corresponds to strings and the rows correspond to the languages. Each element of the table i,j is a 0 or 1 depending on if string x_i is in language L_j.

Consider the language {x_i | x_i \notin L_i}. This must be true for some L_k, for some k, but this is a contradiction. Therefore this enumeration is invalid. Diagonalization trick.

## Consequences

Let sigma be the set of all ASCII characters. {w|w represents a valid python (or any language) program} is a subset of sigma*.

Thus the set of all programs is countable.

For any language L, let f_L(x) = {1 if f in L else 0} All such functions are distinct, so {f_l|L is a language over sigma} is uncountable.

Thus there uncountable number of functions that we can't write programs for.

## Conclusions

There are problems that programs can't solve :(
