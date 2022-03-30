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

:heavy_check_mark: :green_book: [1. Two Sum](https://leetcode.com/problems/two-sum/): <key, val> = <number, its index in nums> (using **enumerate(list)**)

:heavy_check_mark: :orange_book: [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/): <key, val> = <char, index it is last seen> (**Sliding Window & Two Pointers**)

:heavy_check_mark: :orange_book: [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/): <key, val> = <hash array for 26 letters, words belonging to this hash> (using **collections.defaultdict(list)**)

:wavy_dash: :orange_book: [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/): <key, val> = <original node, its copy>

:wavy_dash: :orange_book: [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/): <key, val> = <substring, frequency> (using **Counter()**)

:wavy_dash: :green_book: [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/): <key, val> = <char, the order in which it appeared in string>

:wavy_dash: :green_book: [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
