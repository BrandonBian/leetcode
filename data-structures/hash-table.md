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

## - Python Dictionary and Counters
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

:heavy_check_mark: :green_book: [1. Two Sum](https://leetcode.com/problems/two-sum/): <key, val> = <number, its index in nums> (using **enumerate(list)**)

:heavy_check_mark: :orange_book: [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/): <key, val> = <char, index it is last seen> (**Sliding Window & Two Pointers**)

:heavy_check_mark: :orange_book: [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/): <key, val> = <hash array for 26 letters, words belonging to this hash> (using **collections.defaultdict(list)**)

:heavy_check_mark: :green_book: [290. Word Pattern](https://leetcode.com/problems/word-pattern/): (creating a **hashed pattern list** for an input)

:heavy_check_mark: :green_book: [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/): (using **Counter()**)

:heavy_check_mark: :orange_book: [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/): (**hashed pattern list & two pointers**)

:heavy_check_mark: :orange_book: [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/): <key, val> = <count, index at which this count is reached>

:wavy_dash: :orange_book: [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/): <key, val> = <original node, its copy>

:wavy_dash: :orange_book: [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/): <key, val> = <substring, frequency> (using **Counter()**)

:wavy_dash: :green_book: [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/): <key, val> = <char, the order in which it appeared in string>

:wavy_dash: :green_book: [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/): <key, val> = <num, frequency> (using **Counter()**)

:wavy_dash: :green_book: [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/): <key, val> = <num, list of its appearance positions>

:wavy_dash: :green_book: [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/): <key, val> = <char, its appearance frequency>

:wavy_dash: :orange_book: [299. Bulls and Cows](https://leetcode.com/problems/bulls-and-cows/): (using **Counter()**)

:wavy_dash: :orange_book: [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/): (using **Counter() & counter.most_frequent(k)**)

:wavy_dash: :green_book: [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/): (using **Set()**)

:wavy_dash: :green_book: [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/): (using **Counter()**)

:wavy_dash: :green_book: [389. Find the Difference](https://leetcode.com/problems/find-the-difference/): (hashing the ASCII of letters)

:wavy_dash: :green_book: [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome/): **Palindrome** - should have at most one odd occurrence (using **Counter()**)

:wavy_dash: :orange_book: [447. Number of Boomerangs](https://leetcode.com/problems/number-of-boomerangs/): <key, val> = <distance, number of pairs of points with that distance in-between> (using **Counter()**)

:wavy_dash: :orange_book: [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/): (using **Counter()**)

:wavy_dash: :orange_book: [454. 4Sum II](https://leetcode.com/problems/4sum-ii/): (using **Counter()**)

:wavy_dash: :orange_book: [508. Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/): (using **Counter() & DFS Tree Search**)

:wavy_dash: :orange_book: [554. Brick Wall](https://leetcode.com/problems/brick-wall/): <key, val> = <index of brick edge, frequency> (using **Counter()**)
