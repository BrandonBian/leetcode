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


---

## Chapter 3: Introduction (Part III)

:green_book: [13 · Implement strStr()](https://www.lintcode.com/problem/13/): Naive for loop


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

## Auxiliary Problems: Ch. 1 ~ 7

:green_book: [1790 · Rotate String II](https://www.lintcode.com/problem/1790/description?_from=collection&fromId=161): String manipulation and modulus operation

```
class Solution:
    """
    @param str: An array of char
    @param left: a left offset
    @param right: a right offset
    @return: return a rotate string
    """
    def rotate_string2(self, str: str, left: int, right: int) -> str:
        
        if not str:
            return str

        offset = (left - right) % len(str) # the offset can wrap-around

        return str[offset:] + str[:offset]
```
:closed_book: [594 · strStr II](https://www.lintcode.com/problem/594/?_from=collection&fromId=161): Rabin-Karp (Hashing)

```
class Solution:
    """
    @param source: A source string
    @param target: A target string
    @return: An integer as index
    """
    def str_str2(self, source: str, target: str) -> int:

        # Rabin-karp Hashing
        BASE = int(1e6)

        if source is None or target is None:
            return -1

        m = len(target)

        if m == 0:
            return 0

        power = (31 ** m) % BASE # power of the first character that will be removed

        # calculate the hash for the target string
        target_hash = 0

        for char in target:
            # abc + d (considering char)
            target_hash = (target_hash * 31 + ord(char)) % BASE

        # calculate the hash for the source string
        source_hash = 0

        for i in range(len(source)):
            # abc + d (considering char)
            source_hash = (source_hash * 31 + ord(source[i])) % BASE

            # if we don't have m number of characters
            if i < m - 1:
                continue

            if i >= m:
                # abcd - a (remove first char, which is at position [i - m])
                source_hash = source_hash - (power * ord(source[i - m])) % BASE
                if source_hash < 0:
                    source_hash += BASE
            
            if source_hash == target_hash:
                if source[i - m + 1: i + 1] == target:
                    return i - m + 1

        return -1     
```
---

:closed_book: [841 · String Replace](https://www.lintcode.com/problem/841/?_from=collection&fromId=161): A enumerating method using Set and Dictionary

```
class Solution:
    """
    @param a: The A array
    @param b: The B array
    @param s: The S string
    @return: The answer
    """
    def string_replace(self, a: List[str], b: List[str], s: str) -> str:
        
        # find length options of replacement strings in a[] and construct idx map
        repLens = set()
        idxMap = {} 

        for i in range(len(a)):
            repLens.add(len(a[i]))
            idxMap[a[i]] = i 

        repLens = sorted(repLens, reverse=True) # prioritize longer options
        
        # find replacements
        result = ""
        start = 0

        while start < len(s):

            found = False

            for replen in repLens: # trying from longer ones to shorter ones
                end = start + replen
                if end > len(s): # if too long, just skip
                    continue
                
                str = s[start : end] # the substring to be replaced
                if str in a: # if it is in a, we replace with corresponding b
                    result += b[idxMap[str]]
                    found = True
                    start = end
                    break

            if not found:
                result += s[start]
                start += 1

        return result
```

---


## Chapter 8 (LIVE): Face-to-face Two Pointers & Partition Problems

:green_book: [56 · Two Sum](https://www.lintcode.com/problem/56/): Sort, then face-to-face two pointers

```
class Solution:
    """
    @param numbers: An array of Integer
    @param target: target = numbers[index1] + numbers[index2]
    @return: [index1, index2] (index1 < index2)
    """
    def two_sum(self, numbers: List[int], target: int) -> List[int]:
        
        if len(numbers) <= 1:
            return [-1, -1]
            
        # combine each element with its index 
        nums = [
            (number, idx)
            for idx, number in enumerate(numbers)
        ]

        left, right = 0, len(numbers) - 1
        nums.sort() # sorting a tuple will sort based on the first element

        while left < right:
            if nums[left][0] + nums[right][0] > target:
                right -= 1
            elif nums[left][0] + nums[right][0] < target:
                left += 1
            else:
                return sorted([nums[left][1], nums[right][1]]) # index1 < index2
        
        return [-1, -1]
```

---

:green_book: [607 · Two Sum III - Data structure design](https://www.lintcode.com/problem/607/): Data structure design, add and find functionalities

```
class TwoSum:
    """
    @param number: An integer
    @return: nothing
    """
    nums = []

    def add(self, number):
        self.nums.append(number)

    """
    @param value: An integer
    @return: Find if there exists any pair of numbers which sum is equal to the value.
    """
    def find(self, value):

        left, right = 0, len(self.nums) - 1

        self.nums.sort() # two pointers require the array to be sorted

        while left < right:
            two_sum = self.nums[left] + self.nums[right]

            if two_sum < value:
                left += 1
            elif two_sum > value:
                right -= 1
            else:
                return True

        return False
```

---

:orange_book: [57 · 3Sum](https://www.lintcode.com/problem/57/?_from=collection&fromId=161): Reduce to 2Sum, removing duplicates

```
class Solution:
    """
    @param numbers: Give an array numbers of n integer
    @return: Find all unique triplets in the array which gives the sum of zero.
             we will sort your return value in output
    """
    def three_sum(self, nums: List[int]) -> List[List[int]]:
        # write your code here
        def find_two_sum(left, right, target, result):
            while left < right:
                two_sum = nums[left] + nums[right]
                
                if two_sum < target:
                    left += 1
                elif two_sum > target:
                    right -= 1
                else:
                    result.append([-target, nums[left], nums[right]])
                    left += 1
                    right -= 1
                    
                    # skip duplicates
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1
        
        
        if len(nums) < 3:
            return None
        
        nums.sort()
        length = len(nums)
        result = []
        
        for i in range(0, length - 2):
            
            # skip duplicates
            if i > 0 and nums[i] == nums[i - 1]: # already visited
                continue
                
            # two pointers from the element next to this first element to the last
            left, right = i + 1, length - 1
            target = -nums[i]
                        
            find_two_sum(left, right, target, result)
        
        return result
```

---

:orange_book: [382 · Triangle Count](https://www.lintcode.com/problem/382/): Fix longest edge and 2Sum on shorter ones

```
class Solution:
    """
    @param s: A list of integers
    @return: An integer
    """
    def triangle_count(self, s: List[int]) -> int:
        # write your code here

        def get_triangle_count(s, max_edge_idx):
            count = 0
            # two pointers, each for one short edge
            left, right = 0, max_edge_idx - 1
            target = s[max_edge_idx]

            while left < right:
                two_sum = s[left] + s[right]
                if two_sum > target: # we can make a triangle
                    # if s[left] sufficient, then s[left] ~ s[right - 1] all valid, so a total of (right - 1 - left + 1) selections
                    count += right - left 
                    right -= 1 # we go smaller
                else:
                    left += 1 # we need to go larger

            return count

        # for a triangle to work, we need: sum(two short edges) > longest edge
        if not s or len(s) < 3:
            return 0

        s.sort()
        result = 0

        for i in range(2, len(s)): # fix longest edge and 2 Sum on shorter ones
            result += get_triangle_count(s, i)

        return result
```

---

:orange_book: [31 · Partition Array](https://www.lintcode.com/problem/31/): Two pointers from either end of array going to middle, exchange misplaced elements

```
class Solution:
    """
    @param nums: The integer array you should partition
    @param k: An integer
    @return: The index after partition
    """
    def partition_array(self, nums: List[int], k: int) -> int:

        if not nums:
            return 0

        # two pointers starting from either end of array
        left, right = 0, len(nums) - 1

        while left <= right:

            # find the numbers that are misplaced on either side
            while left <= right and nums[left] < k: 
                # stop when we found a nums[left] >= k, it is misplaced
                left += 1
            while left <= right and nums[right] >= k: 
                # stop when we found a nums[right] < k, it is misplaced
                right -= 1

            # exchange
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]

                left += 1
                right -= 1

        return left # index of the first element on the right partition
```

---

:orange_book: [148 · Sort Colors](https://www.lintcode.com/problem/148/): Quick sort partition (also two pointers)

```
class Solution:
    """
    @param nums: A list of integer which is 0, 1 or 2
    @return: nothing
    """
    def sort_colors(self, nums: List[int]):

        def partition(nums, k):
            # point to the last element in the (< k) region (i.e., a barrier)
            last_small_ptr = -1

            for i in range(len(nums)):
                if nums[i] < k:
                    # the barrier moves one step
                    last_small_ptr += 1

                    # exchange
                    nums[last_small_ptr], nums[i] = \
                    nums[i], nums[last_small_ptr]

        partition(nums, 1)
        partition(nums, 2)

        return nums
```

---

:orange_book: [143 · Sort Colors II](https://www.lintcode.com/problem/143/): Arbitrary number of colors, two pointers partitioning with Recursion

```
class Solution:
    """
    @param colors: A list of integer
    @param k: An integer
    @return: nothing
    """
    def sort_colors2(self, colors: List[int], k: int):
        
        if not colors or len(colors) < 2:
            return colors

        self.sort_colors(colors, color_from = 1, color_to = k, idx_from = 0, idx_to = len(colors) - 1)

    def sort_colors(self, colors, color_from, color_to, idx_from, idx_to):
        # this will run with Recursion

        # stopping condition: only one color in this region, we are done here
        if color_from == color_to:
            return

        # select a middle color and partition around it
        mid_color = (color_from + color_to) // 2

        # two pointers - partitioning (put elements <= mid color to left, > mid color to right)

        left, right = idx_from, idx_to

        while left <= right:

            # find elements that do not belong to that side
            while left <= right and colors[left] <= mid_color:
                left += 1
            while left <= right and colors[right] > mid_color:
                right -= 1

            if left <= right:
                # exchange
                colors[left], colors[right] = colors[right], colors[left]
                left += 1
                right -= 1

        # left = index of first element on the right partition
        # right = index of last element on the left partition

        # index_from --- right -- left -- index_to

        # sort left portion
        self.sort_colors(colors, color_from, mid_color, idx_from, right)
        self.sort_colors(colors, mid_color + 1, color_to, left, idx_to)
```

---

:green_book: [539 · Move Zeroes](https://www.lintcode.com/problem/539/): Two pointers, one for filling, one for moving

```
class Solution:
    """
    @param nums: an integer array
    @return: nothing
    """
    def move_zeroes(self, nums: List[int]):
        
        if len(nums) <= 1:
            return nums

        fill_ptr, move_ptr = 0, 0

        while move_ptr < len(nums):
            if nums[move_ptr] != 0:
                if fill_ptr != move_ptr: # this statement can be removed, will become slower
                    nums[fill_ptr] = nums[move_ptr]
                fill_ptr += 1
            move_ptr += 1

        # replace all remaining elements with 0
        while fill_ptr < len(nums):
            nums[fill_ptr] = 0
            fill_ptr += 1
```

---

## Chapter 9 (LIVE): Binary Search

:orange_book: [447 · Search in a Big Sorted Array](https://www.lintcode.com/problem/447/): Exponential Backoff & Binary Search

```
class Solution:
    """
    @param reader: An instance of ArrayReader.
    @param target: An integer
    @return: An integer which is the first index of target.
    """
    def searchBigSortedArray(self, reader, target):
        # first find the maximum range (Exponential Backoff)
        range_total = 1
        while reader.get(range_total - 1) < target:
            range_total *= 2 # O(logN)

        # binary search on index
        left, right = 0, range_total
        
        while left + 1 < right:
            mid = (left + right) >> 1

            # minimize the index s.t. nums[mid] >= target
            if reader.get(mid) >= target:
                right = mid
            else:
                left = mid

        if reader.get(left) == target:
            return left
        if reader.get(right) == target:
            return right

        return -1
```

---

:orange_book: [460 · Find K Closest Elements](https://www.lintcode.com/problem/460/): Binary Search find upper closest index then compare bottom and left interleavingly

```
class Solution:
    """
    @param a: an integer array
    @param target: An integer
    @param k: An integer
    @return: an integer array
    """

    def find_upper_closest(self, a, target): 
        # using binary search to find least index s.t. a[index] >= target
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

        return target - a[left] <= a[right] - target # compare distance

    def k_closest_numbers(self, a: List[int], target: int, k: int) -> List[int]:
        
        # -- [left] -- [target] -- [right] ---

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


