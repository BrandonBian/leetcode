# Hash Tables · Guide

[← All topics](../../README.md) &nbsp;·&nbsp; **Guide** &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; [Solutions](solutions.md)

- **LeetCode Problems**: [List](https://leetcode.com/tag/hash-table/)
- **Reference 1**: [Study Guide in C++](https://leetcode.com/tag/hash-table/discuss/1068545/HASH-TABLE-and-MAP-POWERFUL-GUIDE-!!!)
- **Reference 2**: [List of Selected Problems in Study Guide](https://leetcode.com/list/504wrexe/)
---

## Python dict and Counter (and how to sort them)
- **Using Python Counter()**:
```python
from collections import Counter

# Initialization
cnt = Counter()           # then populate it, e.g. cnt[key] += value
cnt = Counter(nums)       # or build it directly from a list

# Counter.items():
for key, val in cnt.items():
    pass  # key is the element, val is its count

# Counter.most_common(k):
for key, val in cnt.most_common(k):
    pass  # the k most frequent elements, most frequent first
```

- **Using sorted() / .sort() to sort major iterables (e.g., List & Counter & Dictionary)**:
- Reference for using sorted(): [link](https://docs.python.org/3/howto/sorting.html)
```python
# Sorting Lists
list1 = [3,5,6,1,7,4,4,5]

s1 = sorted(list1)
# s1 = [1, 3, 4, 4, 5, 5, 6, 7] (same as list1.sort())

s2 = sorted(list1, reverse = True)
# s2 = [7, 6, 5, 5, 4, 4, 3, 1] (same as list1.sort(reverse = True))

######################################################

# Sorting Counters
x = Counter({'a':5, 'b':3, 'c':7})

x = x.most_common()
# We will get [('c', 7), ('a', 5), ('b', 3)], sorted by decreasing frequency
# So x is now a list of (element, frequency) tuples

x1 = sorted(x, key = lambda x: x[0], reverse=True)
# x1 = [('c', 7), ('b', 3), ('a', 5)], by element in decreasing order

x2 = sorted(x, key = lambda x: x[1])
# x2 = [('b', 3), ('a', 5), ('c', 7)], by frequency in increasing order

######################################################

# Sorting dictionary
dictionary = {'a': 1, 'b': 2, 'c': 3}

list(dictionary.items())
# [('a', 1), ('b', 2), ('c', 3)], change dictionary items to a list of tuples

# Then proceed with the above method by sorting the tuples based on keys or values
```

## Core Hashing Techniques

- **About Hashing A Pattern**: (see LeetCode Problem 290)

```python
# Hash the pattern
# Example:
# Input = ["abba"]
# Output = [0, 1, 1, 0]

hashed_pattern, seen, counter, mapping = [], set(), 0, {}

for char in pattern:
    if char not in seen:
        seen.add(char)
        mapping[char] = counter
        counter += 1

for char in pattern:
    hashed_pattern.append(mapping[char])
```

- **About Hashing Letters as ASCII**: (see LeetCode Problem 389)
    - Char to ASCII: **ord(char)**
    - ASCII to Char: **chr(ASCII)**
    - There are **26** alphabetical characters in English, so initialize hash to be **[0] * 26**
    - Similarly we can also use **defaultdict(int)**: <key, val> = <char, frequency> (see LeetCode Problem 438)

```python
# Example hashing (considering the frequency of appearances of letters):
# "abcd" -> [1, 1, 1, 1, 0, ..., 0]

hash_s = [0] * 26

for char in s:
    hash_s[ord(char) - ord('a')] += 1
```


---
