# Math: GCD & LCM · Guide

[← All topics](../../README.md) &nbsp;·&nbsp; **Guide**

Practice: both examples below are solved with binary search, see [1201](../binary-search/solutions.md#lc-1201) and [878](../binary-search/solutions.md#lc-878) in the Binary Search solutions.

## **Formulae**

```python
def gcd(a, b) -> int:
    if b == 0:
        return a
    return gcd(b, a % b)
    
def lcm(a, b) -> int:
     return (a * b) / gcd(a, b)
```


## **Related Questions & Concepts**

### Example 1: :orange_book: [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/)

![lcm](../../assets/figures/lcm.png)

**F(N) = (total number of positive integers <= N which are divisible by a or b or c.)**

F(N) = a + b + c - a ∩ c - a ∩ b - b ∩ c + a ∩ b ∩ c

F(N) = N/a + N/b + N/c - N/lcm(a, c) - N/lcm(a, b) - N/lcm(b, c) + N/lcm(a, b, c)

```python
ab = lcm(a,b)
ac = lcm(a,c)
bc = lcm(b,c)
abc = lcm(a,bc)

total = F(num) = num//a + num//b + num//c - num//ab - num//ac - num//bc + num//abc
```
---

### Example 2: :closed_book: [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/)

**F(N) = (total number of positive integers <= N which are divisible by a or b.)**

```python
total = F(num) = num // a + num // b - num // lcm(a, b)
```
