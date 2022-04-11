# Binary Search
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [Full List](https://leetcode.com/tag/binary-search)
- **Reference 1**: [Study Guide - Most Upvoted One](https://leetcode.com/tag/binary-search/discuss/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)
- **Reference 2**: [Study Guide - Solving "K-th" Type of Problems](https://leetcode.com/tag/binary-search/discuss/1529866/Solving-kth-kind-of-problems)
- **Reference 3**: [Study Guide - Approach to Writing Bug-free Binary Search Code](https://leetcode.com/tag/binary-search/discuss/1089533/An-approach-to-writing-bug-free-Binary-Search-code)
- **Selected LeetCode Problems**: [List](https://leetcode.com/list/xls4oirv/)


## - General Binary Search Ideas
- **Conceptually**: Basically, it splits the search space into two halves and only keep the half that probably has the search target and throw away the other half that would not possibly have the answer. In this manner, we reduce the search space to half the size at every step, until we find the target. Binary Search helps us reduce the search time from linear O(n) to logarithmic **O(log n)**.

- **When should we consider using Binary Search**: If we can discover some kind of monotonicity, for example, if condition(k) is True then condition(k + 1) is True, then we can consider binary search

- **A General Template - Minimize k , s.t. condition(k) is True**: mainly needs a variation of three parts
    - Correctly **initialize the boundary variables left and right** to specify search space. Only one rule: set up the boundary to **include all possible elements**
    - Decide **return value**. Is it return left or return left - 1? Remember this: after exiting the while loop, **left is the minimal k satisfying the condition function**
    - Design the **condition function**. This is the most difficult and most beautiful part. Needs lots of practice.

```
def binary_search(array) -> int:

    def condition(value) -> bool:
        pass

    left, right = min(search_space), max(search_space) # could be [0, n], [1, n] etc. Depends on problem
    
    while left < right:
        mid = left + (right - left) // 2
        if condition(mid):
            right = mid
        else:
            left = mid + 1
            
    return left
```


## - Selected LeetCode Problems:

- **Examples using a variation of the "Minimize k , s.t. condition(k) is True" Template**

:heavy_check_mark: :green_book: [278. First Bad Version](https://leetcode.com/problems/first-bad-version/): minimize k, s.t. isBadVersion(k) is True

:heavy_check_mark: :green_book: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/): minimize k, s.t. k * k > mid is True, return left - 1

:heavy_check_mark: :green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): minimize k, s.t. nums[mid] >= target

:heavy_check_mark: :orange_book: [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/): minimize k, s.t. capacity k is feasible to ship all packages

:heavy_check_mark: :closed_book: [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/):  minimize k, s.t. we can split the array into m subarrays without breaching the threshold k




