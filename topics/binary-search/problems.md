# Binary Search · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. "Find Position" Problems (usually reducible to "minimizing")

:heavy_check_mark: :green_book: [704. Binary Search](https://leetcode.com/problems/binary-search/): the most basic problem &nbsp;→ [solution](solutions.md#lc-704)

:heavy_check_mark: :orange_book: [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/): minimizing and maximizing positions &nbsp;→ [solution](solutions.md#lc-34)

:heavy_check_mark: :orange_book: [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/): treat **2D matrix as 1D array**, then classic binary search &nbsp;→ [solution](solutions.md#lc-74)

:heavy_check_mark: :orange_book: [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/): array rotated at a pivot, find pivot and search on either side &nbsp;→ [solution](solutions.md#lc-33)

:heavy_check_mark: :orange_book: [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/): same as above but removing duplicates (similar as [this procedure](../two-pointers/guide.md#face-to-face--4-sum-reduced-to-2-sum-with-duplicate-removal-18-4sum)) &nbsp;→ [solution](solutions.md#lc-81)

:heavy_check_mark: :orange_book: [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/): compare nums[mid] with nums[right] to decide which half holds the minimum &nbsp;→ [solution](solutions.md#lc-153)

:wavy_dash: :orange_book: [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/): array not sorted (but peaks present) &nbsp;→ [solution](solutions.md#lc-162)

## 2. "Minimizing" Problems

:heavy_check_mark: :green_book: [278. First Bad Version](https://leetcode.com/problems/first-bad-version/): minimize k, s.t. isBadVersion(k) is True &nbsp;→ [solution](solutions.md#lc-278)

:heavy_check_mark: :orange_book: [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/): minimize k, s.t. there are at least n ugly numbers <= k (**least common multiple & greatest common divisor**) &nbsp;→ [solution](solutions.md#lc-1201)

:heavy_check_mark: :green_book: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/): minimize k, s.t. k * k > mid is True, return left - 1 &nbsp;→ [solution](solutions.md#lc-69)

:heavy_check_mark: :green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): minimize k, s.t. nums[mid] >= target &nbsp;→ [solution](solutions.md#lc-35)

:heavy_check_mark: :orange_book: [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/): minimize k, s.t. capacity k is feasible to ship all packages &nbsp;→ [solution](solutions.md#lc-1011)

:heavy_check_mark: :orange_book: [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/): binary search on the value range counting nums <= mid, or Floyd cycle detection on i -> nums[i] &nbsp;→ [solution](solutions.md#lc-287)

:wavy_dash: :orange_book: [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/): minimize k, s.t. k is a divisor that the obtained result is <= threshold &nbsp;→ [solution](solutions.md#lc-1283)

:wavy_dash: :orange_book: [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/): minimize k, s.t. she can eat all n piles of bananas within h hours &nbsp;→ [solution](solutions.md#lc-875)

:wavy_dash: :orange_book: [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/): minimize k, s.t. we can make m bouquets after waiting for k days &nbsp;→ [solution](solutions.md#lc-1482)

:wavy_dash: :closed_book: [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/): minimize k, s.t. we can split the array into m subarrays without breaching the threshold k &nbsp;→ [solution](solutions.md#lc-410)

:wavy_dash: :orange_book: [2187. Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips/): minimize time, s.t. all buses complete at least totalTrips (given) trips at this time &nbsp;→ [solution](solutions.md#lc-2187)

:wavy_dash: :orange_book: [1760. Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/): minimize penalty, s.t. we can achieve this penalty without exceeding limit of operations &nbsp;→ [solution](solutions.md#lc-1760)

:wavy_dash: :closed_book: [887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/): usually solved by **dynamic programming**, but can be solved using the template &nbsp;→ [solution](solutions.md#lc-887)

## 3. "Maximizing" Problems

:heavy_check_mark: :orange_book: [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/) &nbsp;→ [solution](solutions.md#lc-2226)

:wavy_dash: :orange_book: [1898. Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters/) &nbsp;→ [solution](solutions.md#lc-1898)

## 4. "K-th" Problems

:heavy_check_mark: :orange_book: [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/): minimize summation, s.t. there are at least k (given) pairs with sum <= this summation (**2 Pointers**) &nbsp;→ [solution](solutions.md#lc-373)

:heavy_check_mark: :closed_book: [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/): binary search on the partition index of the shorter array (left max <= right min on both sides) &nbsp;→ [solution](solutions.md#lc-4)

:wavy_dash: :closed_book: [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/): minimize k, s.t. there are at least k pairs with distance <= distance &nbsp;→ [solution](solutions.md#lc-719)

:wavy_dash: :orange_book: [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/): minimize element, s.t. there are at least k (k is given) elements <= this element &nbsp;→ [solution](solutions.md#lc-378)

:wavy_dash: :closed_book: [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/): more complex design of the feasible function (logic using **lcm & gcd**) &nbsp;→ [solution](solutions.md#lc-878)

:wavy_dash: :closed_book: [668. Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/): minimize k, s.t. there are at least k (another variable given in question) values <= k &nbsp;→ [solution](solutions.md#lc-668)
