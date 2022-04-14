# Binary Search
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [Full List](https://leetcode.com/tag/binary-search)
- **Reference 1**: [Study Guide - Most Upvoted One with Template](https://leetcode.com/tag/binary-search/discuss/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)
- **Reference 2**: [Study Guide - Solving "K-th" Type of Problems](https://leetcode.com/tag/binary-search/discuss/1529866/Solving-kth-kind-of-problems)
- **Reference 3**: [Study Guide - Approach to Writing Bug-free Binary Search Code](https://leetcode.com/tag/binary-search/discuss/1089533/An-approach-to-writing-bug-free-Binary-Search-code)
- **Reference 4**: [Study Guide -Binary Search 101](https://leetcode.com/problems/binary-search/discuss/423162/Binary-Search-101)
- **Selected LeetCode Problems**: [List](https://leetcode.com/list/xls4oirv/)


## - General Binary Search Ideas (and a Template)
- **Conceptually**: Basically, it splits the search space into two halves and only keep the half that probably has the search target and throw away the other half that would not possibly have the answer. In this manner, we reduce the search space to half the size at every step, until we find the target. Binary Search helps us reduce the search time from linear O(n) to logarithmic **O(log n)**.

- **When should we consider using Binary Search**: If we can discover some kind of monotonicity, for example, if condition(k) is True then condition(k + 1) is True, then we can consider binary search

- **A General Template - Minimize k, s.t. condition(k) is True**: mainly needs a variation of three parts
    - Correctly **initialize the boundary variables left and right** to specify search space. Only one rule: set up the boundary to **include all possible elements**
    - Decide **return value**. Is it return left or return left - 1? Remember this: after exiting the while loop, **left is the minimal k satisfying the condition function**
    - Design the **condition / feasible function**. This is the most difficult and most beautiful part. Needs lots of practice.

```
# Motivation: minimize [mid] such that [mid] satisfies the given conditions (i.e., feasible(mid) is True)

def binary_search(array) -> int:

    def feasible(value) -> bool:
        # TODO - write your implementation to check whether [value] satisfies the given conditions
        pass

    left, right = min(search_space), max(search_space) # must include the ENTIRE search space
    
    while left < right:
    
        # Note: when an array has an even number of elements
        # it's your decision to use either the left mid (lower mid) or the right mid (upper mid)
        
        # if choosing left/lower mid -> when MINIMIZING (i.e., moving right when feasible)
        mid = (left + right) >> 1 
        
        # if choosing right/upper mid -> when MAXIMIZING (i.e., moving left when feasible)
        mid = (left + right + 1) >> 1
        
        if feasible(mid): # if this is good, we move to the lower portion (PRESERVING mid) to look for smaller options
            right = mid # (for MINIMIZING)
            # OR move to the upper portion to check for larger options: "left = mid" (for MAXIMIZING)
            
        else: # otherwise move to upper portion (EXCLUDING mid) to look for larger options
            left = mid + 1 # (for MINIMIZING)
            # OR move to the lower portion to check for smaller options: "right = mid - 1" (for MAXIMIZING)
            
    return left
```


## - Selected LeetCode Problems:

- **Typical examples using a variation of the "Minimize k , s.t. condition(k) is True" Template**

:heavy_check_mark: :green_book: [278. First Bad Version](https://leetcode.com/problems/first-bad-version/): minimize k, s.t. isBadVersion(k) is True

:heavy_check_mark: :green_book: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/): minimize k, s.t. k * k > mid is True, return left - 1

:heavy_check_mark: :green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): minimize k, s.t. nums[mid] >= target

:heavy_check_mark: :orange_book: [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/): minimize k, s.t. capacity k is feasible to ship all packages

:heavy_check_mark: :orange_book: [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/): minimize k, s.t. there are at least n ugly numbers <= k (**least common multiple & greatest common divisor**)

:wavy_dash: :orange_book: [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/): minimize k, s.t. k is a divisor that the obtained result is <= threshold

:wavy_dash: :orange_book: [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/): minimize k, s.t. she can eat all n piles of bananas within h hours

:wavy_dash: :orange_book: [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/): minimize k, s.t. we can make m bouquets after waiting for k days

:wavy_dash: :closed_book: [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/): minimize k, s.t. we can split the array into m subarrays without breaching the threshold k

:wavy_dash: :closed_book: [668. Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/): minimize k, s.t. there are at least k (another variable given in question) values <= k

:wavy_dash: :closed_book: [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/): minimize k, s.t. there are at least k pairs with distance <= distance

---

- **Solving various "K-th" kind of problems (more varied but using the template above if possible)**:

:heavy_check_mark: :orange_book: [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/): minimize summation, s.t. there are at least k (given) pairs with sum <= this summation (**2 Pointers**)

:heavy_check_mark: :orange_book: [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/): (a **maximizing alteration to the template**)

:wavy_dash: :orange_book: [658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/): minimize idx (start of length k array), such that x (given) <= the mid point of arr[idx] + arr[idx + k]

:wavy_dash: :orange_book: [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/): minimize element, s.t. there are at least k (k is given) elements <= this element

:wavy_dash: :orange_book: [2187. Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips/): minimize time, s.t. all buses complete at least totalTrips (given) trips at this time
