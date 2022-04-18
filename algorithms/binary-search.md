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
- **Conceptually**: Basically, **BINARY SEARCH splits the search space into two halves and only keep the half that probably has the search target and throw away the other half that would not possibly have the answer**. In this manner, we reduce the search space to half the size at every step, until we find the target. Binary Search helps us reduce the search time from linear O(n) to logarithmic **O(log n)**.

## :exclamation: General Template - VERY GOOD, PLEASE MEMORIZE
- **Can be used for solving: "Find Position / Minimum / Maximum / K-th" types of problems (see problems listed below)**

```
def binary_search(nums):

    ###################################
    # edge case check (if applicable) #
    ###################################

    if not nums:
        return -1

    #############################
    # define condition function #
    #############################

    def feasible(value) -> bool:
        # TODO - check whether value satisfies the certain condition
        pass

    #######################
    # define search space #
    #######################     

    # make sure to include the ENTIRE search space
    left, right = min(search_space), max(search_space)

    #################
    # binary search #
    #################

    while left + 1 < right:

        mid = (left + right) >> 1

        if feasible(mid):

            # for MINIMIZING problem (search on left for smaller solution):
            right = mid

            # for MAXIMIZING problem (search on right for larger solution):
            left = mid

        else:

            # for MINIMIZING problem (search on right for solution):
            left = mid

            # for MAXIMIZING problem (search on left for solution):
            right = mid

    #################
    # find solution #
    #################        

    result = -1

    # for MINIMIZING problem, check "left" first
    # result = minimum element that satisfies the sufficient() condition function

    if sufficient(left):
        result = left
    elif sufficient(right):
        result = right

    # for MAXIMIZING problem, check "right" first
    # result = maximum element that satisfies the sufficient() condition function

    if sufficient(right):
        result = right
    elif sufficient(left):
        result = left

    # return result
    return result
```

- **An Example (MINIMIZING and MAXIMIZING position):** :heavy_check_mark: :orange_book: [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) 

```
# basic checking
if not nums:
    return [-1, -1]

first_pos, last_pos = -1, -1

####################################
# find first position (MINIMIZING) #
####################################

left, right = 0, len(nums) - 1

while left + 1 < right:
    mid = (left + right) >> 1

    if nums[mid] == target: # look left for smaller solution
        right = mid
    if nums[mid] > target: # look left
        right = mid
    if nums[mid] < target: # look right
        left = mid

# for MINIMIZING position, check "left" first
if nums[left] == target:
    first_pos = left
elif nums[right] == target:
    first_pos = right

###################################
# find last position (MAXIMIZING) #
###################################

left, right = 0, len(nums) - 1

while left + 1 < right:
    mid = (left + right) >> 1

    if nums[mid] == target: # look right for larger solution
        left = mid
    if nums[mid] > target: # look left
        right = mid
    if nums[mid] < target: # look right
        left = mid

# for MAXIMIZING position, check "right" first

if nums[right] == target:
    last_pos = right
elif nums[left] == target:
    last_pos = left

##########
# return #
##########

return [first_pos, last_pos]
```

## - Selected LeetCode Problems:

- **"FIND POSITION" Problems**:

:heavy_check_mark: :green_book: [704. Binary Search](https://leetcode.com/problems/binary-search/) 

---

- **"MINIMIZING" Problems**:

:heavy_check_mark: :green_book: [278. First Bad Version](https://leetcode.com/problems/first-bad-version/): minimize k, s.t. isBadVersion(k) is True

:heavy_check_mark: :orange_book: [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/): minimize k, s.t. there are at least n ugly numbers <= k (**least common multiple & greatest common divisor**)

:heavy_check_mark: :green_book: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/): minimize k, s.t. k * k > mid is True, return left - 1

:heavy_check_mark: :green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): minimize k, s.t. nums[mid] >= target

:heavy_check_mark: :orange_book: [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/): minimize k, s.t. capacity k is feasible to ship all packages

---

- **"MAXIMIZING" Problems**:


---

- **"K-th" Problems**:

 

---


:wavy_dash: :orange_book: [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/): minimize k, s.t. k is a divisor that the obtained result is <= threshold

:wavy_dash: :orange_book: [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/): minimize k, s.t. she can eat all n piles of bananas within h hours

:wavy_dash: :orange_book: [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/): minimize k, s.t. we can make m bouquets after waiting for k days

:wavy_dash: :closed_book: [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/): minimize k, s.t. we can split the array into m subarrays without breaching the threshold k

:wavy_dash: :closed_book: [668. Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/): minimize k, s.t. there are at least k (another variable given in question) values <= k

:wavy_dash: :closed_book: [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/): minimize k, s.t. there are at least k pairs with distance <= distance

---

- **Solving various "K-th" kinds of problems (using Template 2 if possible)**:

:heavy_check_mark: :orange_book: [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/): minimize summation, s.t. there are at least k (given) pairs with sum <= this summation (**2 Pointers**)


:wavy_dash: :orange_book: [658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/): minimize idx (start of length k array), such that x (given) <= the mid point of arr[idx] + arr[idx + k]

:wavy_dash: :orange_book: [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/): minimize element, s.t. there are at least k (k is given) elements <= this element

:wavy_dash: :closed_book: [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/): more complexed design of the feasible function (logic using **lcm & gcd**)

---

- **Solving various "Minimum" kinds of problems (using Template 2 if possible)**:

:wavy_dash: :orange_book: [2187. Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips/): minimize time, s.t. all buses complete at least totalTrips (given) trips at this time

:wavy_dash: :orange_book: [1760. Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/): minimize penalty, s.t. we can achieve this penalty without exceeding limit of operations

---
- **Solving various "Maximum" kinds of problems (using Template 2 if possible)**:

:heavy_check_mark: :orange_book: [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/)


:wavy_dash: :orange_book: [1898. Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters/)


:wavy_dash: :orange_book: [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/)

---

- **Other atypical problems**:

:heavy_check_mark: :orange_book: [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/): treat **2D matrix as 1D array**, then classic binary search

:wavy_dash: :orange_book: [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/): array not sorted (but peaks present), variation of the classic template

:wavy_dash: :orange_book: [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/): array rotated at a pivot, find pivot and search on either side, variation of the classic template

:wavy_dash: :closed_book: [887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/): usually solved by **dynamic programming**, but can be solved using the second template with a special formula



