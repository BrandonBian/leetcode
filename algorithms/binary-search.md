# Binary Search | [[Selected Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode-Problems.md#section-1-binary-search-problems--study-guide--solutions)]
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
- **Selected LeetCode Problems by others**: [List](https://leetcode.com/list/xls4oirv/)
- **Conceptually**: Basically, **BINARY SEARCH splits the search space into two halves and only keep the half that probably has the search target and throw away the other half that would not possibly have the answer**. In this manner, we reduce the search space to half the size at every step, until we find the target. Binary Search helps us reduce the search time from linear O(n) to logarithmic **O(log n)**.

## :white_check_mark: General Template - VERY GOOD, PLEASE MEMORIZE
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

---

- **An example with MINIMIZING / MAXIMIZING positions**: :heavy_check_mark: :orange_book: [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```
# basic checking
if not nums:
    return [-1, -1]

first_pos, last_pos = -1, -1

####################################
# find first position (MINIMIZING) #
####################################

def sufficient_minimize(num):
    # find least number that is >= target
    return num >= target

left, right = 0, len(nums) - 1

while left + 1 < right:
    mid = (left + right) >> 1

    if sufficient_minimize(nums[mid]): # look left for smaller solution
        right = mid
    else:
        left = mid

# for MINIMIZING position, check "left" first
if nums[left] == target:
    first_pos = left
elif nums[right] == target:
    first_pos = right

###################################
# find last position (MAXIMIZING) #
###################################

def sufficient_maximize(num):
    # find max number that is <= target
    return num <= target

left, right = 0, len(nums) - 1

while left + 1 < right:
    mid = (left + right) >> 1

    if sufficient_maximize(nums[mid]): # look right for larger solution
        left = mid
    else:
        right = mid

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
