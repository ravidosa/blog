---
title: "[Project Euler] Day 1: Multiples of 3 and 5"
date: 2025-01-01T00:00:00+08:00
mathjax: true
draft: false
---
> If we list all the natural numbers below $10$ that are multiples of $3$ or $5$, we get $3, 5, 6$ and $9$. The sum of these multiples is $23$.
Find the sum of all the multiples of $3$ or $5$ below $1000$.

[code](https://github.com/ravidosa/euler/blob/main/p1.py)
### Parameters
- $n = 1000$: upper bound on integers being checked
- $m = 2$: number of factors to check divisibility for
- $k = 5$: upper bound on factors

Naively, we can take advantage of Python's list comprehension to implement a solution. Iterating over every integer between $1$ and $1000$, we can check if the integer is divisible by $3$ or $5$.
```py
ans = sum([i for i in range(1, 1000) if (i % 3 == 0 or i % 5 == 0)])
```
Generalizing to arbitrary $n$, $m$, and $k$:
```py
ans = sum([i for i in range(1, N) if any([i % factor == 0 for factor in div_by])])
```
This would have a time complexity of $O(nm)$, since we check if every integer is divisible by every factor.

We can improve on this by using the [inclusion-exclusion principle](https://en.wikipedia.org/wiki/Inclusion%E2%80%93exclusion_principle). For finite sets $A$ and $B$:
$$ |A \cup B| = |A| + |B| - |A \cap B|$$
Here, $A$ and $B$ are the sets of integers divisible by $3$ and $5$, respectively. We can find the sum of all integers divisible $3$ or $5$ by finding the sum of all integers divisible by $3$ and by $5$, then subtracting the sum of all integers divisible by their least common multiple, $15$. To efficiently find the sum of all such integers, we can use the following properties relating to the $p$th term and $p$th partial sum of an [arithmetic sequence](https://en.wikipedia.org/wiki/Arithmetic_progression):
$$
\begin{align}
a_p &= a_1 + (p - 1) \cdot d \\\\\\\\
S_p & = (a_1 + a_p) \cdot \frac{p}{2} \\\\\\
\end{align}
$$
where $a_1$ is the first term and $d$ is the difference between successive terms. Here, $d = a_1$, so the sum simplifies to:
$$
\begin{align}
S_p &= (a_1 + (a_1 + (p - 1) \cdot a_1)) \cdot \frac{p}{2} \\\\\\
&= \frac{p(p + 1)a_1}{2}
\end{align}
$$
The number of terms in the sequence can be found by taking the floor of the smallest integer below the upper bound divided by the factor. Using this to find the sums of the integers divisible by $3, 5,$ and $15$:
```py
ans = (999 // 3) * (999 // 3 + 1) * 3 / 2 + (999 // 5) * (999 // 5 + 1) * 5 / 2 - (999 // 15) * (999 // 15 + 1) * 15 / 2
```
Generalizing to arbitrary $n$, $m$, and $k$:
```py
ans = 0
for intersection in range(1, len(div_by) + 1):
    for subset in itertools.combinations(div_by, intersection):
        a1 = math.lcm(*subset)
        n = (N - 1) // a1
        ans += (-1) ** (intersection + 1) * ((n + 1) * a1 * n // 2)
```
This would have a time complexity of $O(2^m\log k)$, since there are $2^m - 1$ nonempty subsets of the factors and finding the least common multiple has a time complexity of $O(\log k)$.