# Hash Tables · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. Important Problems

:heavy_check_mark: :green_book: [1. Two Sum](https://leetcode.com/problems/two-sum/): <key, val> = <number, its index in nums> (using **enumerate(list)**) &nbsp;→ [solution](solutions.md#lc-1)

:heavy_check_mark: :orange_book: [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/): <key, val> = <char, index it is last seen> (**Sliding Window & Two Pointers**) &nbsp;→ [solution](solutions.md#lc-3)

:heavy_check_mark: :orange_book: [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/): <key, val> = <hash array for 26 letters, words belonging to this hash> (using **collections.defaultdict(list)**) &nbsp;→ [solution](solutions.md#lc-49)

:heavy_check_mark: :orange_book: [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/): put numbers in a set; only count a streak from x when x - 1 is not in the set &nbsp;→ [solution](solutions.md#lc-128)

:heavy_check_mark: :green_book: [290. Word Pattern](https://leetcode.com/problems/word-pattern/): (creating a **hashed pattern list** for an input) &nbsp;→ [solution](solutions.md#lc-290)

:heavy_check_mark: :green_book: [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/): (using **Counter()**) &nbsp;→ [solution](solutions.md#lc-350)

:heavy_check_mark: :orange_book: [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/): (**hashed pattern list & two pointers & sliding window**) &nbsp;→ [solution](solutions.md#lc-438)

:heavy_check_mark: :orange_book: [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/): <key, val> = <count, index at which this count is reached> (**prefix sum**) &nbsp;→ [solution](solutions.md#lc-525)

:heavy_check_mark: :orange_book: [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/): <key, val> = <prefix sum, frequency> (using **prefix sum & defaultdict(int) /  Counter()**) &nbsp;→ [solution](solutions.md#lc-560)

:heavy_check_mark: :green_book: [594. Longest Harmonious Subsequence](https://leetcode.com/problems/longest-harmonious-subsequence/): <key, val> = <number, frequency> (using **Counter()**) &nbsp;→ [solution](solutions.md#lc-594)

:heavy_check_mark: :closed_book: [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/): a very clever way of using index to record frequency, using input list as extra memory &nbsp;→ [solution](solutions.md#lc-41)

:heavy_check_mark: :green_book: [136. Single Number](https://leetcode.com/problems/single-number/): XOR everything, pairs cancel out (a ^ a = 0) &nbsp;→ [solution](solutions.md#lc-136)

:heavy_check_mark: :green_book: [169. Majority Element](https://leetcode.com/problems/majority-element/): <key, val> = <number, frequency> (using **Counter()**) &nbsp;→ [solution](solutions.md#lc-169)

:heavy_check_mark: :green_book: [1636. Sort Array by Increasing Frequency](https://leetcode.com/problems/sort-array-by-increasing-frequency/): (my solution **sorting defaultdict(int) by key using sorted(dict.items())**) &nbsp;→ [solution](solutions.md#lc-1636)

:heavy_check_mark: :orange_book: [1679. Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/): (a clever way using **Counter()**) &nbsp;→ [solution](solutions.md#lc-1679)

## 2. Supplemental Problems

:wavy_dash: :orange_book: [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/): <key, val> = <original node, its copy> &nbsp;→ [solution in Linked Lists](../linked-list/solutions.md#lc-138)

:wavy_dash: :orange_book: [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/): <key, val> = <substring, frequency> (using **Counter()**)

:wavy_dash: :green_book: [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/): <key, val> = <char, the order in which it appeared in string>

:wavy_dash: :green_book: [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/): <key, val> = <num, frequency> (using **Counter()**)

:wavy_dash: :green_book: [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/): <key, val> = <num, list of its appearance positions>

:wavy_dash: :green_book: [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/): <key, val> = <char, its appearance frequency>

:wavy_dash: :orange_book: [299. Bulls and Cows](https://leetcode.com/problems/bulls-and-cows/): (using **Counter()**)

:heavy_check_mark: :orange_book: [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/): Counter, then pop k items from a max-heap of (-freq, num) &nbsp;→ [solution in Heaps & Priority Queues](../heap/solutions.md#lc-347)

:wavy_dash: :green_book: [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/): (using **Set()**)

:wavy_dash: :green_book: [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/): (using **Counter()**)

:wavy_dash: :green_book: [389. Find the Difference](https://leetcode.com/problems/find-the-difference/): (hashing the ASCII of letters)

:wavy_dash: :green_book: [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome/): **Palindrome** - should have at most one odd occurrence (using **Counter()**)

:wavy_dash: :orange_book: [447. Number of Boomerangs](https://leetcode.com/problems/number-of-boomerangs/): <key, val> = <distance, number of pairs of points with that distance in-between> (using **Counter()**)

:wavy_dash: :orange_book: [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/): (using **Counter()**)

:wavy_dash: :orange_book: [454. 4Sum II](https://leetcode.com/problems/4sum-ii/): (using **Counter()**)

:wavy_dash: :orange_book: [508. Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/): (using **Counter() & DFS Tree Search**)

:wavy_dash: :orange_book: [554. Brick Wall](https://leetcode.com/problems/brick-wall/): <key, val> = <index of brick edge, frequency> (using **Counter()**)

:wavy_dash: :green_book: [645. Set Mismatch](https://leetcode.com/problems/set-mismatch/): (using **Counter() & Set()**)

:wavy_dash: :orange_book: [957. Prison Cells After N Days](https://leetcode.com/problems/prison-cells-after-n-days/): requires loop finding, non-trivial, saving all patterns inside dictionary (using **dictionary**)

:wavy_dash: :orange_book: [970. Powerful Integers](https://leetcode.com/problems/powerful-integers/)

:wavy_dash: :green_book: [1002. Find Common Characters](https://leetcode.com/problems/find-common-characters/): (my solution using **hashed pattern list**)

:wavy_dash: :green_book: [1160. Find Words That Can Be Formed by Characters](https://leetcode.com/problems/find-words-that-can-be-formed-by-characters/): (my solution using **Counter()**)

:wavy_dash: :green_book: [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/): (my solution using **hashed pattern list**)

:wavy_dash: :green_book: [1207. Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/): (using **defaultdict(int)**)

:wavy_dash: :green_book: [1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/): (using **defaultdict(int)**)

:wavy_dash: :green_book: [1539. Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number/): can be solved using binary search in O(logN) (using **defaultdict(int)**)
