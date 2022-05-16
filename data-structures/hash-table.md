# Hash Tables
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [List](https://leetcode.com/tag/hash-table/)
- **Reference 1**: [Study Guide in C++](https://leetcode.com/tag/hash-table/discuss/1068545/HASH-TABLE-and-MAP-POWERFUL-GUIDE-!!!)
- **Reference 2**: [List of Selected Problems in Study Guide](https://leetcode.com/list/504wrexe/)
---

## - Python Dictionary and Counters (and how to sort them)
- **Using Python Counter()**:
```
# Initialization
cnt = Counter()
    ... # Code to populate the counter (e.g., cnt[key] += value)
    ... # Or to create counter directly from list (e.g., cnt = Counter(list))

# Counter.items():
for key, val in counter.items():
    ... # key is the element, val is its count

# Counter.most_common(k):
for key, val in cnt.most_common(k):
    ... # key is the element, val is its count
```

- **Using sorted() / .sort() to sort major iterables (e.g., List & Counter & Dictionary)**:
- Reference for using sorted(): [link](https://docs.python.org/3/howto/sorting.html)
```
# Sorting Lists
list1 = [3,5,6,1,7,4,4,5]

s1 = sorted(list1) 
# s1 = [1, 3, 4, 4, 5, 5, 6, 7] (same as list1.sort())

s2 = sorted(list2, reverse = True) 
# s2 = [7, 6, 5, 5, 4, 4, 3, 1] (same as list1.sort(reverse = True))

######################################################

# Sorting Counters
x = Counter({'a':5, 'b':3, 'c':7})

x = x.most_common()
# We will get [('c', 7), ('a', 5), ('b', 3)], sorted by decreasing value
# So we changed x to a tuple of <element, frequency>

x1 = sorted(x, key = lambda x: x[0], reverse=True)
# x1 = [('d', 0), ('c', 7), ('b', 3), ('a', 5)], by value in decreasing order

x2 = sorted(x, key = lambda x: x[1])
# x2 = [('d', 0), ('b', 3), ('a', 5), ('c', 7)], by frequency in increasing order

######################################################

# Sorting dictionary
dictionary = {'a': 1, 'b': 2, 'c': 3}

list(d.items())
# [('a', 1), ('b', 2), ('c', 3)], change dictionary items to a list of tuples

# Then proceed with the above method by sorting the tuples based on keys or values
```

## - Selected LeetCode Core Problems: (with <key, val> implementation summary)

- **About Hashing A Pattern**: (see LeetCode Problem 290)

```
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
    - ASCII to Char: **char(ASCII)**
    - There are **26** alphabetical characters in English, so initialize hash to be **[0] * 26**
    - Similarly we can also use **defaultdict(int)**: <key, val> = <char, frequency> (see LeetCode Problem 438)

```
# Example hashing (considering the frequency of appearances of letters):
# "abcd" -> [1, 1, 1, 1, 0, ..., 0]

hash_s = [0] * 26

for char in s:
    hash_s[ord(char) - ord('a')] += 1
```


---







