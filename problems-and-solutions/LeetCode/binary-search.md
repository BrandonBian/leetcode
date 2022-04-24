# LeetCode - Binary Search | Problems
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---

## Binary Search Problems | [[Study Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/binary-search.md)] | [[Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/binary-search-solutions.md)]
### 1. "FIND POSITION" Problems - usually can be reduced to "MINIMIZING" Problems:

:heavy_check_mark: :green_book: [704. Binary Search](https://leetcode.com/problems/binary-search/): the most basic problem

:heavy_check_mark: :orange_book: [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/): minimizing and maximizing positions

:heavy_check_mark: :orange_book: [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/): treat **2D matrix as 1D array**, then classic binary search

:heavy_check_mark: :orange_book: [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/): array rotated at a pivot, find pivot and search on either side

:heavy_check_mark: :orange_book: [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/): same as above but removing duplicates (similar as [this procedure](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/two-pointers.md#face-to-face---4-sum---reduce-to-2-sum--removing-duplicates-heavy_check_mark-orange_book-18-4sum))

:wavy_dash: :orange_book: [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/): array not sorted (but peaks present)

---

### 2. "MINIMIZING" Problems

:heavy_check_mark: :green_book: [278. First Bad Version](https://leetcode.com/problems/first-bad-version/): minimize k, s.t. isBadVersion(k) is True

:heavy_check_mark: :orange_book: [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/): minimize k, s.t. there are at least n ugly numbers <= k (**least common multiple & greatest common divisor**)

:heavy_check_mark: :green_book: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/): minimize k, s.t. k * k > mid is True, return left - 1

:heavy_check_mark: :green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): minimize k, s.t. nums[mid] >= target

:heavy_check_mark: :orange_book: [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/): minimize k, s.t. capacity k is feasible to ship all packages

:wavy_dash: :orange_book: [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/): minimize k, s.t. k is a divisor that the obtained result is <= threshold

:wavy_dash: :orange_book: [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/): minimize k, s.t. she can eat all n piles of bananas within h hours

:wavy_dash: :orange_book: [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/): minimize k, s.t. we can make m bouquets after waiting for k days

:wavy_dash: :closed_book: [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/): minimize k, s.t. we can split the array into m subarrays without breaching the threshold k

:wavy_dash: :orange_book: [2187. Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips/): minimize time, s.t. all buses complete at least totalTrips (given) trips at this time

:wavy_dash: :orange_book: [1760. Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/): minimize penalty, s.t. we can achieve this penalty without exceeding limit of operations

:wavy_dash: :closed_book: [887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/): usually solved by **dynamic programming**, but can be solved using the template

:wavy_dash: :orange_book: [LintCode 585 · Maximum Number in Mountain Sequence](https://www.lintcode.com/problem/585/): minimize index, s.t. num[index] is the maximum in the mountain sequence

:wavy_dash: :orange_book: [159 · Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/159/)

---

### 3. "MAXIMIZING" Problems

:heavy_check_mark: :orange_book: [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/)

:wavy_dash: :orange_book: [1898. Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters/)

:wavy_dash: :closed_book: [LintCode 183 · Wood Cut](https://www.lintcode.com/problem/183/)

---

### 4. "K-th" Problems

:heavy_check_mark: :orange_book: [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/): minimize summation, s.t. there are at least k (given) pairs with sum <= this summation (**2 Pointers**)

:heavy_check_mark: :orange_book: [LintCode 460 · Find K Closest Elements](https://www.lintcode.com/problem/460/): find upper closest index to target, compare left distance then right distance

:wavy_dash: :closed_book: [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/): minimize k, s.t. there are at least k pairs with distance <= distance

:wavy_dash: :orange_book: [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/): minimize element, s.t. there are at least k (k is given) elements <= this element

:wavy_dash: :closed_book: [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/): more complexed design of the feasible function (logic using **lcm & gcd**)

:wavy_dash: :closed_book: [668. Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/): minimize k, s.t. there are at least k (another variable given in question) values <= k

---
