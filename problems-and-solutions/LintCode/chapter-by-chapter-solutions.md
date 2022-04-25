# LintCode - Chapter by Chapter Practice Solutions | [[Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LintCode/chapter-by-chapter.md)]
**All problems are LintCode problems, solved in Python.**

---

## Chapter 1: Introduction (Part I)
**No practice Problems**

---

## Chapter 2: Introduction (Part II)

:orange_book: [200 · Longest Palindromic Substring](https://www.lintcode.com/problem/200/): Enumeration from middle with two pointers / Dynamic Programming

- **Enumeration from middle with two pointers**
```
class Solution:
    """
    @param s: input string
    @return: a string as the longest palindromic substring
    """
    def longest_palindrome(self, s: str) -> str:
        if not s:
            return None

        answer = (0, 0) # (substring length, substring starting position)

        for mid in range(len(s)):
        
            # Note: max is comparing the first element of each tuple
            answer = max(answer, self.get_palindrome_from(s, mid, mid))
            answer = max(answer, self.get_palindrome_from(s, mid, mid + 1))

        # s[starting position : starting position + substring length]
        return s[answer[1] : answer[1] + answer[0]]
        
    def get_palindrome_from(self, s, left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            # two pointers move to either side
            left -= 1
            right += 1

        # right - left - 1 -> length of the palindrome substring
        # left + 1 -> index at which the palindrome substring starts
        return right - left - 1, left + 1
```

- **Dynamic Programming**

```
class Solution:
    """
    @param s: input string
    @return: a string as the longest palindromic substring
    """
    def longest_palindrome(self, s: str) -> str:
        if not s:
            return None

        n = len(s)

        # whether s[i] ~ s[j] is a palindrome substring
        is_palindrome = [[False for _ in range(n)] for _ in range(n)]

        # each single character is itself a palindrome
        for i in range(n):
            is_palindrome[i][i] = True

        # smaller than a single character is itself a palindrome (e.g., an empty string)
        for i in range(1, n):
            is_palindrome[i][i-1] = True

        start_pos, longest_length = 0, 1

        # first enumerate possible substring lengths, then enumerate start position for that length
        for length in range(2, n + 1):
            for start in range(n - length + 1):
                end = start + length - 1
                # is palindrome if two characters at either end are same and the substring in-between is palindrome
                is_palindrome[start][end] = is_palindrome[start + 1][end - 1] and s[start] == s[end]

                # if this is the longest palindrome substring we have encountered, record its start position and length
                if is_palindrome[start][end] and length > longest_length:
                    longest_length = length
                    start_pos = start

        return s[start_pos : start_pos + longest_length]
```

---

:orange_book: [667 · Longest Palindromic Subsequence](https://www.lintcode.com/problem/667/): Dynamic Programming

```
class Solution:
    """
    @param s: the maximum length of s is 1000
    @return: the longest palindromic subsequence's length
    """
    def longest_palindrome_subseq(self, s: str) -> int:
        
        # idea: Dynamic Programming

        if len(s) <= 1:
            return len(s)

        size = len(s)

        # dp[i][j] = the length of the longest palinidromic subsequence in range s[i ~ j]
        dp = [[0 for _ in range(size)] for _ in range(size)]

        # each single character is itself a palindrome of length 1
        for i in range(size):
            dp[i][i] = 1

        for i in range(size - 1, -1, -1):
            for j in range(i + 1, size):
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else: 
                    dp[i][j] = max(dp[i][j - 1], dp[i + 1][j])

        return dp[0][size - 1]
```

---

## Chapter 3: Introduction (Part III)

:green_book: [13 · Implement strStr()](https://www.lintcode.com/problem/13/): Naive for loop / Rabin-Karp

- **Naive for loop**

```
class Solution:
    """
    @param source: 
    @param target: 
    @return: return the index
    """
    def str_str(self, source: str, target: str) -> int:

        if len(source) < len(target):
            return -1

        for i in range(len(source) - len(target) + 1):
            if source[i:i+len(target)] == target:
                return i
                
        return -1
```

- **Rabin-Karp**

```
# Reference: https://www.lintcode.com/problem/13/solution/19657
# Usually it's OK to use the naive O(N^2) approach since this approach is too advanced for a normal interview
```

---

:orange_book: [187 · Gas Station](https://www.lintcode.com/problem/187/): Greedy algorithm

```
class Solution:
    """
    @param gas: An array of integers
    @param cost: An array of integers
    @return: An integer
    """
    def can_complete_circuit(self, gas: List[int], cost: List[int]) -> int:
        
        if sum(gas) < sum(cost):
            return -1
            
        index = 0
        gas_remain = 0
        
        for i in range(len(gas)): # we start at station No.0
            
            gas_remain += gas[i]-cost[i]

            if gas_remain < 0:
                gas_remain = 0 # reset to 0, indicating we are starting fresh from station No.[i+1]
                index = i+1
                
        # there is no looping back, so this solution assumes an answer exists
        # relevant proof: https://www.lintcode.com/problem/187/solution/17249
        return index
```

---

## Chapter 4: Algorithm Complexity & Two Pointers

:orange_book: [415 · Valid Palindrome](https://www.lintcode.com/problem/415/): Remove special characters, face-to-face two pointers

```
class Solution:
    """
    @param s: A string
    @return: Whether the string is a valid palindrome
    """
    def is_palindrome(self, s: str) -> bool:
        # write your code here
        if not s or len(s) == 1:
            return True

        left, right = 0, len(s) - 1

        while left < right:

            # skip the non-alphabetical characters
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1
            
            # compare the characters at both ends
            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1
        
        return True
```

---

:orange_book: [891 · Valid Palindrome II](https://www.lintcode.com/problem/891/): Face-to-face two pointers

```
class Solution:
    """
    @param s: a string
    @return: whether you can make s a palindrome by deleting at most one character
    """
    def valid_palindrome(self, s: str) -> bool:
        
        if len(s) == 1:
            return True

        # go from either end of string and find where the palindrome is broken
        left, right = 0, len(s) - 1

        while left < right:
            if s[left] != s[right]:
                break
            left += 1
            right -= 1
        
        if left >= right: # if we haven't break then s is already a palindrome
            return True

        # now [left] = starting position of where the palindrome is broken
        # now [right] = ending position of where the palindrome is broken

        # remove s[left] OR remove s[right]
        return self.is_palindrome(s, left + 1, right) or self.is_palindrome(s, left, right - 1)

    def is_palindrome(self, s, left, right):

        # see if s[left : right] is a palindrome
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        
        return True
```

---

## Chapter 5: Two Sorting Algorithms

:green_book: [463 · Sort Integers](https://www.lintcode.com/problem/463/): Quick sort / Merge Sort (see [this](https://www.lintcode.com/problem/463/solution/17194) for more)

- **Quick Sort**
```
from typing import (
    List,
)

class Solution:
    """
    @param a: an integer array
    @return: nothing
    """
    def sort_integers(self, a: List[int]):
        
        if len(a) <= 1: # base case, no need for sorting
            return a

        # quick sort the entire array
        self.quick_sort(a, 0, len(a) - 1)

    def quick_sort(self, a, start, end):

        if start >= end:
            return
        
        left, right = start, end
        # set pivot to be the middle index (pivot shouldn't be the start or end)
        # pivot should be the value not index
        pivot = a[(start + end) >> 1]

        # notice: using equivalence when comparing index, but no equivalence when comparing value
        while left <= right:
            while left <= right and a[left] < pivot:
                left += 1
            while left <= right and a[right] > pivot:
                right -= 1
            
            # now [left] = index of first element >= pivot counted from left
            # now [right] = index of first element <= pivot counted from right
            # exchange them if applicable
            if left <= right:
                a[left], a[right] = a[right], a[left]
                left += 1
                right -= 1

        # now [left] is at the right side of [right] after quiting the while loop
        # recursively quick sort the portions on either side of the pivot
        self.quick_sort(a, start, right)
        self.quick_sort(a, left, end)  
```

---

- **Merge Sort**

```
from typing import (
    List,
)

class Solution:
    """
    @param a: an integer array
    @return: nothing
    """
    def sort_integers(self, a: List[int]):
        
        if len(a) <= 1:
            return a

        tmp = [0] * len(a)
        self.merge_sort(a, 0, len(a) - 1, tmp)

    def merge_sort(self, a, start, end, tmp):
        if start >= end:
            return

        # set pivot to be the middle index
        pivot = (start + end) >> 1
        
        # sort the portions on the left and right of the pivot
        self.merge_sort(a, start, pivot, tmp)
        self.merge_sort(a, pivot + 1, end, tmp)

        # merge them in-order
        self.merge(a, start, end, tmp)

    def merge(self, a, start, end, tmp):

        # again, the pivot is set to be the middle index
        pivot = (start + end) >> 1
        left_start = start
        right_start = pivot + 1
        idx = start

        # create a new array to store the merged array
        merged_array = []

        while left_start <= pivot and right_start <= end:
            if a[left_start] < a[right_start]:
                tmp[idx] = a[left_start]
                left_start += 1
            else:
                tmp[idx] = a[right_start]
                right_start += 1
            idx += 1

        # there may still be elements left on either portion
        while left_start <= pivot:
            tmp[idx] = a[left_start]
            left_start += 1
            idx += 1
        while right_start <= end:
            tmp[idx] = a[right_start]
            right_start += 1
            idx += 1
        
        # replace original array with this new merged array 
        for i in range(start, end + 1):
            a[i] = tmp[i]
```

---

:orange_book: [5 · Kth Largest Element](https://www.lintcode.com/problem/5/description): Quick Select

```
class Solution:
    """
    @param k: An integer
    @param nums: An array
    @return: the Kth largest element
    """
    def kth_largest_element(self, k: int, nums: List[int]) -> int:
        
        if not nums:
            return -1

        return self.quick_select(nums, 0, len(nums) - 1, k)

    def quick_select(self, nums, start, end, k):

        if start == end:
            return nums[start]

        i, j = start, end
        pivot = nums[(start + end) >> 1]

        # from largest to smallest
        while i <= j:
            while i <= j and nums[i] > pivot:
                i += 1
            while i <= j and nums[j] < pivot:
                j -= 1

            if i <= j:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
                j -= 1 
            
        # start --- j - i --- end

        k_pos = start + k - 1
        
        if k_pos <= j:
            return self.quick_select(nums, start, j, k) 
        
        if k_pos >= i:
            return self.quick_select(nums, i, end, k - (i - start))

        return nums[j + 1]   
```

---

## Chapter 6: Binary Search (using Recursion)

:green_book: [366 · Fibonacci](https://www.lintcode.com/problem/366/): General Recursion is too slow, use Dynamic Programming


```
class Solution:
    """
    @param n: an integer
    @return: an ineger f(n)
    """
    def fibonacci(self, n: int) -> int:
        if n == 1:
            return 0
        if n == 2:
            return 1
        
        p, q, r = 0, 1, 1

        for i in range(3, n):
            p, q = q, r
            r = p + q
        
        return r
```

---

:green_book: [457 · Classical Binary Search](https://www.lintcode.com/problem/457/): Binary Search but using Recursion

```
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def findPosition(self, nums, target):    
        return self.binary_search(nums, 0, len(nums) - 1, target)

    def binary_search(self, nums, start, end, target):

        # we failed to find the target
        if start > end:
            return -1

        mid = (start + end) >> 1

        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            return self.binary_search(nums, mid + 1, end, target) # search on right
        else:
            return self.binary_search(nums, start, mid - 1, target) # search on left
```

---

## Chapter 7: Binary Search (introducing a template)

:green_book: [457 · Classical Binary Search](https://www.lintcode.com/problem/457/): Binary Search using the template

```
class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def findPosition(self, nums, target):
        
        if not nums:
            return -1

        left, right = 0, len(nums) - 1

        # idea: find the least index s.t. nums[index] >= target
        while left + 1 < right:
            mid = (left + right) >> 1

            if nums[mid] >= target:
                right = mid # go left to find possible smaller answers
            else:
                left = mid
        
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
        
        return -1
```

---

:green_book: [458 · Last Position of Target](https://www.lintcode.com/problem/458/): Binary Search using the template

```
from typing import (
    List,
)

class Solution:
    """
    @param nums: An integer array sorted in ascending order
    @param target: An integer
    @return: An integer
    """
    def last_position(self, nums: List[int], target: int) -> int:
        
        if not nums:
            return -1

        left, right = 0, len(nums) - 1

        # idea: find the maximum position s.t. nums[position] <= target
        while left + 1 < right:
            mid = (left + right) >> 1

            if nums[mid] <= target:
                left = mid # go right to find larger solutions
            else:
                right = mid
        
        if nums[right] == target:
            return right
        if nums[left] == target:
            return left
        return -1
```

---

