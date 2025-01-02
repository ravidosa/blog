---
title: "[Project Euler] Day 3: Largest Prime Factor"
date: 2025-01-03T00:00:00+08:00
mathjax: true
draft: false
---
> The prime factors of $13195$ are $5, 7, 13$ and $29$.
What is the largest prime factor of the number $600851475143$?

[code](https://github.com/ravidosa/euler/blob/main/p3.py)
### Parameters
- $n = 600851475143$: integer to find largest prime factor of

The largest [prime factor](https://en.wikipedia.org/wiki/Integer_factorization) of a number is at most its square root. By dividing the number by its smallest prime factor, we can remove the smaller prime factors until only the largest is left. The bound on the largest prime factor can also be tightened based on the result of the division.

```py
ans = 600851475143
p = 2

while p <= math.sqrt(ans):
    if ans % p == 0:
        ans //= p
    else:
        p += 1
```
Generalizing to arbitrary $n$:
```py
ans = N
p = 2

while p <= math.sqrt(ans):
    if ans % p == 0:
        ans //= p
    else:
        p += 1
```
This would have a time complexity of $O(\sqrt{n})$, since in the worst case ($n$ is prime), it checks up to $\sqrt{n}$.