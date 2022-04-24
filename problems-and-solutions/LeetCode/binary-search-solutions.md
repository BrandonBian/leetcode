# LeetCode - Binary Search | Solutions
- **Language**: all solutions in **Python 3**
- **Implementation**: ideas taken from various sources, organized and coded by me
- **Problem Search**: use "Ctrl + F" to search for problem ID (there may contain a few LintCode problems so be sure not to mix them up)
---

### 1. "FIND POSITION" Problems - usually can be reduced to "MINIMIZING" Problems:

:heavy_check_mark: :green_book: [704. Binary Search](https://leetcode.com/problems/binary-search/): the most basic problem

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:

        # basic edge case check (if applicable)
        if not nums:
            return -1
        
        # range of search space (in this case, index of nums)
        left, right = 0, len(nums) - 1
    
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if nums[mid] < target: # too small, search on right
                left = mid
            elif nums[mid] > target: # too large, search on left
                right = mid
            else: # we found target, return
                return mid
                
        # if finding FIRST position of target, check "left" first then "right"
        # if finding LAST position of target, check "right" first then "left"
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right

        return -1
```

---

:heavy_check_mark: :orange_book: [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/): minimizing and maximizing positions

```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        
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

---


:heavy_check_mark: :orange_book: [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/): treat **2D matrix as 1D array**, then classic binary search

```
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        
        # Reference
        # https://leetcode.com/problems/search-a-2d-matrix/discuss/26201/A-Python-binary-search-solution-O(logn)
        
        rows, cols = len(matrix), len(matrix[0])
        left, right = 0, rows * cols - 1
        
        while left + 1 < right:
            mid = (left + right) >> 1

            num = matrix[mid // cols][mid % cols] # transform 1D index into position in 2D array
            
            # then just classic binary search
            if num == target:
                return True
            elif num < target:
                left = mid
            else:
                right = mid
        
        if matrix[left // cols][left % cols] == target or matrix[right // cols][right % cols] == target:
            return True
        
        return False
```

---

:heavy_check_mark: :orange_book: [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/): array rotated at a pivot, find pivot and search on either side

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        
        left, right = 0, len(nums) - 1
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if nums[mid] == target:
                return mid

            if nums[mid] > nums[left]:
                if nums[left] <= target <= nums[mid]:
                    right = mid
                else:
                    left = mid
            else:
                if nums[mid] <= target <= nums[right]:
                    left = mid
                else:
                    right = mid
            
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
        return -1
                
```

---

:wavy_dash: :orange_book: [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/): array not sorted (but peaks present)

```
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        
        # Reference
        # https://leetcode.com/problems/find-peak-element/discuss/1290642/Intuition-behind-conditions-or-Complete-Explanation-or-Diagram-or-Binary-Search
        
        if len(nums) == 1:
            return 0
        
        if nums[0] > nums[1]:
            return 0
        
        if nums[-1] > nums[-2]:
            return len(nums) - 1

        left, right = 1, len(nums) - 2

        while left + 1 < right:
            mid = (left + right) >> 1
            
            if nums[mid] > nums[mid - 1]:
                left = mid # go right
            else:
                right = mid # go left
        
        return left if nums[left] > nums[right] else right
        
        return -1
```

---

### 2. "MINIMIZING" Problems

:heavy_check_mark: :green_book: [278. First Bad Version](https://leetcode.com/problems/first-bad-version/): minimize k, s.t. isBadVersion(k) is True

```
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:

class Solution:
    def firstBadVersion(self, n: int) -> int:
        
        # set up boundary to include all possibilities
        left, right = 1, n
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if isBadVersion(mid):
                right = mid
            else:
                left = mid
            
        if isBadVersion(left):
            return left
        if isBadVersion(right):
            return right
        
        return -1
        
```

---

:heavy_check_mark: :orange_book: [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/): minimize k, s.t. there are at least n ugly numbers <= k (**least common multiple & greatest common divisor**)

- [About LCM and GCD](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/gcd-lcm.md)
```
class Solution:
    def nthUglyNumber(self, n: int, a: int, b: int, c: int) -> int:
        
        def gcd(a, b) -> int:
            if b == 0:
                return a
            return gcd(b, a % b)
        
        def lcm(a, b) -> int:
             return (a * b) / gcd(a, b)
        
        def sufficient(num) -> bool:
            # check whether there are at least n ugly numbers <= num
            # ugly number: divisible by a, b, or c
            
            # F(N) = N/a + N/b + N/c - N/lcm(a, c) - N/lcm(a, b) - N/lcm(b, c) + N/lcm(a, b, c) (lcm = least common multiple)
            total = num//a + num//b + num//c - num//ab - num//ac - num//bc + num//abc
            return total >= n
            
        
        left, right = 1, 10**10
        # About the gcd look at this post
        # https://leetcode.com/problems/ugly-number-iii/discuss/387539/cpp-Binary-Search-with-picture-and-Binary-Search-Template
        ab = lcm(a,b)
        ac = lcm(a,c)
        bc = lcm(b,c)
        abc = lcm(a,bc)
        
        while left + 1 < right:
            mid = (left + right) >> 1
            if sufficient(mid):
                right = mid
            else:
                left = mid
        
        if sufficient(left):
            return left
        if sufficient(right):
            return right
        
        return -1
```

---

:heavy_check_mark: :green_book: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/): minimize k, s.t. k * k > mid is True, return left - 1

```
class Solution:
    def mySqrt(self, x: int) -> int:
        
        if x == 0:
            return 0
        if x == 1:
            return 1
        
        left = 1
        right = x
        
        while left + 1 < right:
            mid = left + (right - left) // 2
            if mid * mid > x:
                right = mid
            else:
                left = mid
                
        # left is the minimum value satisfying left^2 > x
        if left * left > x:
            result = left
        elif right * right > x:
            result = right
                
                
        return result - 1 
```

---

:heavy_check_mark: :green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): minimize k, s.t. nums[mid] >= target

```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        
        # notice the len(nums) here instead of len(nums-1) for placing integer at end
        left, right = 0, len(nums) 
        
        def feasible(num):
            return num >= target
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if feasible(nums[mid]):
                right = mid
            else:
                left = mid
                        
        print(left)
        print(right)
        
        if feasible(nums[left]):
            return left
        else:
            return right
```

---

:heavy_check_mark: :orange_book: [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/): minimize k, s.t. capacity k is feasible to ship all packages

```
class Solution:
    def shipWithinDays(self, weights: List[int], days: int) -> int:
        
        # Reference Solution:
        # https://leetcode.com/tag/binary-search/discuss/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems
        
        
        # if there's still room for the current package, we put this package onto the conveyor belt, 
        # otherwise we wait for the next day to place this package. 
        # If the total days needed exceeds D, we return False, otherwise we return True
        
        def feasible(capacity) -> bool:
        # whether it is possible to ship all packages within D days
            day = 1
            total = 0
            
            for weight in weights:
                total += weight
                if total > capacity:  # too heavy, wait for the next day
                    total = weight
                    day += 1
                    if day > days:  # cannot ship within D days
                        return False
            return True
        
        left, right = max(weights), sum(weights)
        # weight capacity can be from least weight to total weight
        
        while left + 1 < right:
            mid = left + (right - left) // 2
            if feasible(mid): # least capacity that is feasible to ship all packages
                right = mid
            else:
                left = mid
                
        if feasible(left):
            return left
        else:
            return right
```

---

:wavy_dash: :orange_book: [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/): minimize k, s.t. k is a divisor that the obtained result is <= threshold

```
class Solution:
    def smallestDivisor(self, nums: List[int], threshold: int) -> int:
        
        def sufficient(divisor) -> bool:
            # it is a divisor that the result is <= threshold
            total = 0
            for num in nums:
                total += math.ceil(num / divisor)
                
            return total <= threshold
        
        left, right = 1, max(nums)
        
        while left + 1 < right:
            mid = left + (right - left) // 2
            if sufficient(mid):
                right = mid
            else:
                left = mid
            
        if sufficient(left):
            return left
        if sufficient(right):
            return right
        
            
```

---

:wavy_dash: :orange_book: [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/): minimize k, s.t. she can eat all n piles of bananas within h hours

```
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        
        # minimum k such that she can eat all bananas within h hours
        def feasible(speed) -> bool:
            return sum(math.ceil(pile / speed) for pile in piles) <= h
        
        # backbone of binary search
        left, right = 1, max(piles)
        
        while left + 1 < right:
            mid = (left + right) >> 1
            if feasible(mid):
                right = mid
            else:
                left = mid
                
        if feasible(left):
            return left
        if feasible(right):
            return right
        
```

---

:wavy_dash: :orange_book: [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/): minimize k, s.t. we can make m bouquets after waiting for k days

```
class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        # minimum # of days to wait to make m bouquets
        
        if len(bloomDay) < m * k: # we need at least m * k flowers
            return -1
        
        def feasible(days) -> bool:
            # whether feasible to make at least m bouquets after waiting for days days
            flowers, bouquets = 0, 0
            
            for day in bloomDay:
                if day > days:
                    flowers = 0 # this flower will never bloom in time
                else: # this flower can bloom in time
                    bouquets += (flowers + 1) // k
                    flowers = (flowers + 1) % k # flowers that are left after making bouquets
                
            return bouquets >= m # can make at least m bouquets

        
        left, right = 1, max(bloomDay)
        
        while left + 1 < right:
            mid = left + (right - left) // 2
            if feasible(mid):
                right = mid
            else:
                left = mid
                
        if feasible(left):
            return left
        if feasible(right):
            return right
        
        return -1
```

---

:wavy_dash: :closed_book: [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/): minimize k, s.t. we can split the array into m subarrays without breaching the threshold k

```
class Solution:
    def splitArray(self, nums: List[int], m: int) -> int:
        
        # Minimize largest sum s.t. all m subarrays have sum <= it
        
        def feasible(threshold) -> bool:
            # whether we can split the array into m subarrays without violating the threshold
            count = 1
            total = 0
            
            for num in nums:
                total += num
                if total > threshold:
                    # we create a new subarray
                    total = num
                    count += 1
                    if count > m:
                        return False
            return True

        # Base binary search
        left, right = max(nums), sum(nums)
        
        while left + 1 < right:
            mid = left + (right - left) // 2
            if feasible(mid):
                right = mid
            else:
                left = mid
                
        if feasible(left):
            return left
        if feasible(right):
            return right
```

---

:wavy_dash: :orange_book: [2187. Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips/): minimize time, s.t. all buses complete at least totalTrips (given) trips at this time

```
class Solution:
    def minimumTime(self, time: List[int], totalTrips: int) -> int:
        
        def sufficient(t):
            # at this time, all buses completed at least totalTrips trips
            total = 0
            for bus in time:
                total += t // bus
            return total >= totalTrips
            
        # lower bound 1 because we need at least 1 time
        # upper bound is 1e14 because slowest bus needs 1e7 to complete 1 trip, maximum total trips = 1e7, so 1e7 * 1e7 = 1e14
        left, right = 1, int(1e14)
        
        while left < right:
            mid = left + (right - left) // 2
            if sufficient(mid):
                right = mid
            else:
                left = mid + 1
        
        return int(left)
```

---

:wavy_dash: :orange_book: [1760. Minimum Limit of Balls in a Bag](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/): minimize penalty, s.t. we can achieve this penalty without exceeding limit of operations

```
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        
        def feasible(num):
            # whether we can achieve this penalty without exceeding the maxOperations
            # for each bag, split it into bags with value [num]
            operations = sum((a - 1) // num for a in nums)
            return operations <= maxOperations
        
        
        # these are the possible penalties
        left, right = 1, int(1e9)
        
        while left < right:
            mid = (left + right) >> 1
            if feasible(mid):
                right = mid
            else:
                left = mid + 1
                
        return left
```

---

:wavy_dash: :closed_book: [887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/): usually solved by **dynamic programming**, but can be solved using the template

```
class Solution:
    def superEggDrop(self, k: int, n: int) -> int:
        
        def x_choose_y(x, y):
            # x is number of moves, y is number of eggs
            result = 1
            for i in range(0, y):
                result *= (x - i)
            for i in range(1, y + 1):
                result = result // i
            return result
        
        def feasible(x):
            # see if we can determine f with these number of moves
            summation = 0
            term = 1
            i = 1
            
            # Find sum of binomial coefficients
            # xCi (where i varies from 1 to k, number of eggs).
            # If the sum becomes more than K
            
            # Ref: https://www.geeksforgeeks.org/eggs-dropping-puzzle-binomial-coefficient-and-binary-search-solution/
            
            while i <= k:
                # term *= x - i + 1
                # term /= i
                # summation += term
                # i += 1
                summation += x_choose_y(x, i)
                i += 1
                
            return summation >= n # we can cover at least n (all the) floors
                    
        
        # number of moves (at least one move; at most move once on each floor)
        left, right = 1, n
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if feasible(mid): # if [mid] moves satisfy condition, look for possible smaller answers
                right = mid
            else:
                left = mid
                
        if feasible(left):
            return left
        if feasible(right):
            return right
        
```

---

:wavy_dash: :orange_book: [LintCode 585 · Maximum Number in Mountain Sequence](https://www.lintcode.com/problem/585/): minimize index, s.t. num[index] is the maximum in the mountain sequence

```
class Solution:
    """
    @param nums: a mountain sequence which increase firstly and then decrease
    @return: then mountain top
    """
    def mountain_sequence(self, nums: List[int]) -> int:
        # write your code here

        # binary search on array index
        left, right = 0, len(nums) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            if nums[mid] < nums[mid + 1]:
                left = mid
            else:
                right = mid
            
        return max(nums[left], nums[right])
```

---

:wavy_dash: :orange_book: [159 · Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/159/)

```
class Solution:
    """
    @param nums: a rotated sorted array
    @return: the minimum number in the array
    """
    def find_min(self, nums: List[int]) -> int:
        # write your code here

        if not nums:
            return -1

        # binary search on the index of the nums
        left, right = 0, len(nums) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            # not using nums[mid] > nums[left] because the array can be not rotated
            if nums[mid] > nums[right]: # go right
                left = mid
            else:
                right = mid

        return min(nums[left], nums[right])
```

---

### 3. "MAXIMIZING" Problems

:heavy_check_mark: :orange_book: [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children/)

```
class Solution:
    def maximumCandies(self, candies: List[int], k: int) -> int:
        
        def feasible(num):
            # whether we can split the piles of candies to give each child >= [num] candies
            count = 0
            for pile in candies: # see each pile can be divided into how many sub-piles of size [num]
                count += pile // num
            return count >= k # we can make this allocation (more piles than number of children)
                
        if sum(candies) < k:
            return 0
            
        left, right = 0, max(candies)
        
        while left + 1 < right:
            
            mid = (left + right) >> 1

            if feasible(mid): # look larger side
                left = mid
            else: # look lower side
                right = mid
        
        if feasible(right):
            return right
        if feasible(left):
            return left

```

---

:wavy_dash: :orange_book: [1898. Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters/)

```
class Solution:
    def maximumRemovals(self, s: str, p: str, removable: List[int]) -> int:
        
        def feasible(k):
            # after removing k characters from s, p is still a subsequence of s
            remove = set(removable[:k])
            
            # 2 pointers
            i, j = 0, 0
            while i < len(s) and j < len(p):
                if i in remove: # skip this character
                    i += 1
                    continue
                if s[i] == p[j]:
                    i += 1
                    j += 1
                else:
                    i += 1
                    
            return j == len(p) # see if entire p is matched
        
        # the range of k
        left, right = 0, len(removable)
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if feasible(mid):
                left = mid
            else:
                right = mid
                
        if feasible(right):
            return right
        if feasible(left):
            return left
        
        return -1
```

---

:wavy_dash: :closed_book: [LintCode 183 · Wood Cut](https://www.lintcode.com/problem/183/)

```
class Solution:
    """
    @param l: Given n pieces of wood with length L[i]
    @param k: An integer
    @return: The maximum length of the small pieces
    """

    def wood_cut(self, l: List[int], k: int) -> int:
        # write your code here

        def feasible(L, mid) -> bool:
            count = 0
            for length in L:
                count += length // mid
            return count >= k

        if not l:
            return 0

        # binary search on the length of the small pieces
        left, right = 1, max(l)

        # if right < 1: # impossible
        #     return 0

        while left + 1 < right:
            mid = (left + right) >> 1

            if feasible(l, mid): # if feasible, look for larger results
                left = mid
            else:
                right = mid

        if feasible(l, right):
            return right
        if feasible(l, left):
            return left

        return 0
```

---

### 4. "K-th" Problems

:heavy_check_mark: :orange_book: [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/): minimize summation, s.t. there are at least k (given) pairs with sum <= this summation (**2 Pointers**)

```
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
    
        def feasible(summation) -> bool:
            # there are at least k pairs with sum <= this summation
            count = 0
            j = len(nums2) - 1
            for i in range(len(nums1)): # O(len(nums1) + len(nums2)) -> 2 pointers
                while j >= 0 and nums1[i] + nums2[j] > summation:
                    j -= 1
                count += j + 1
                if count >= k:
                    return True
            return False
        
        left, right = nums1[0] + nums2[0], nums1[-1] + nums2[-1]
        
        
        while left < right:
            mid = (left + right) >> 1
            if feasible(mid):
                right = mid
            else:
                left = mid + 1
                
        print(left)
        min_val = left
        result = []

        # get all pairs whose sum is <= min_val
        
        j = len(nums2) - 1
        
        for i in range(len(nums1)):
            while j >= 0 and nums1[i] + nums2[j] > min_val:
                j -= 1
                
            for c in range(j+1):
                result.append([nums1[i], nums2[c]])
            
        # sort to make pair in increasing order of sum
        result = sorted(result, key = lambda x: x[0]+x[1])

        # if we gave too many results, remove the ones with largest number of sum
        k = min(len(result), k)
        result = result[:k]
        
        return result
        
        
        
        # Time complexity : O(log(N) * O(count_function) )
        # where N = max_sum - min_sum
```

---

:heavy_check_mark: :orange_book: [LintCode 460 · Find K Closest Elements](https://www.lintcode.com/problem/460/): find upper closest index to target, compare left distance then right distance

```
class Solution:
    """
    @param a: an integer array
    @param target: An integer
    @param k: An integer
    @return: an integer array
    """

    def find_upper_closest(self, a, target):
        left, right = 0, len(a) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            if a[mid] >= target:
                right = mid
            else:
                left = mid

        if a[left] >= target:
            return left
        if a[right] >= target:
            return right

        return len(a)

    def is_left_closer(self, a, target, left, right):
        if left < 0:
            return False
        if right >= len(a):
            return True

        return target - a[left] <= a[right] - target

    def k_closest_numbers(self, a: List[int], target: int, k: int) -> List[int]:
        # write your code here

        right = self.find_upper_closest(a, target)
        left = right - 1

        result = []

        for _ in range(k):
            if self.is_left_closer(a, target, left, right):
                result.append(a[left])
                left -= 1
            else:
                result.append(a[right])
                right += 1

        return result
```

---

:wavy_dash: :closed_book: [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/): minimize k, s.t. there are at least k pairs with distance <= distance

```
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        # kth smallest distance pair
        
        nums.sort()
        n = len(nums)
        
        def enough(distance) -> bool:
            # there is at least k pairs with distance <= distance
            count, slow, fast = 0, 0, 0
            while slow < n or fast < n:
                while fast < n and nums[fast] - nums[slow] <= distance:  # move fast pointer
                    fast += 1
                count += fast - slow - 1  # count pairs
                slow += 1  # move slow pointer
            return count >= k
        
        # distance among pairs
        left, right = 0, max(nums) - min(nums)
        
        
        # if there is at least k pairs with distance <= distance, also for distance <= distance + 1
        # then we look at left side
        while left + 1 < right:
            mid = left + (right - left) // 2
            if enough(mid):
                right = mid
            else:
                left = mid
                
        if enough(left):
            return left
        if enough(right):
            return right
```

---

:wavy_dash: :orange_book: [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/): minimize element, s.t. there are at least k (k is given) elements <= this element

```
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        
        def feasible(element) -> bool:
            # there are at least k elements <= this element
            count = k # number of elements <= this element
            col = cols-1
            for row in range(rows):
                while col >= 0 and matrix[row][col] > element:
                    col -= 1
                count -= col + 1 
                if count <= 0:
                    return True
            return False
            
        left, right = matrix[0][0], matrix[-1][-1]
        rows, cols = len(matrix), len(matrix[0])
        
        while left < right:
            mid = left + (right - left) // 2
            if feasible(mid):
                right = mid
            else:
                left = mid + 1
        
        return left
```

:wavy_dash: :closed_book: [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/): more complexed design of the feasible function (logic using **lcm & gcd**)

```
class Solution:
    def nthMagicalNumber(self, n: int, a: int, b: int) -> int:
        
        def gcd(a, b):
            if b == 0:
                return a
            return gcd(b, a % b)
        
        def lcm(a, b):
            return (a*b) // gcd(a,b)
        
        def feasible(num):
            # see if (magical number <= num) >= n
            # count = the number of magic numbers <= num
            count = num // a + num // b - num // _lcm
            return count >= n
        
        left, right = min(a, b), n * max(a, b)
        _lcm = lcm(a, b)
        
        while left < right:
            mid = (left + right) >> 1
            
            if feasible(mid):
                right = mid
            else:
                left = mid + 1
                
        return left % (int(1e9) + 7)
```

---

:wavy_dash: :closed_book: [668. Kth Smallest Number in Multiplication Table](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/): minimize k, s.t. there are at least k (another variable given in question) values <= k

```
class Solution:
    def findKthNumber(self, m: int, n: int, k: int) -> int:
        # note that we only have the height and width of the multiplication table
        
        def enough(num) -> bool:
            # there are at least k values <= num
            count = 0
            for val in range(1, m+1): # for each row
                add = min(num // val, n) # every row in the Multiplication Table is just multiples of its index, limited by n columns
                if add == 0:  # early exit
                    break
                count += add
            return count >= k
            
        left, right = 1, m*n
        
        while left + 1 < right:
            mid = left + (right - left) // 2
            if enough(mid):
                right = mid
            else:
                left = mid
        
        if enough(left):
            return left
        if enough(right):
            return right
```

---
