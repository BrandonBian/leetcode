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

:orange_book: [585 · Maximum Number in Mountain Sequence](https://www.lintcode.com/problem/585/): Locate whether [mid] is on increasing or decreasing side

```
class Solution:
    """
    @param nums: a mountain sequence which increase firstly and then decrease
    @return: then mountain top
    """
    def mountain_sequence(self, nums: List[int]) -> int:

        # binary search on array index
        left, right = 0, len(nums) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            if nums[mid] < nums[mid + 1]: # on increasing side
                left = mid
            else: # on decreasing side
                right = mid
            
        return max(nums[left], nums[right])
```

---

:orange_book: [159 · Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/159/): Locate which segment is [mid] at

```
class Solution:
    """
    @param nums: a rotated sorted array
    @return: the minimum number in the array
    """
    def find_min(self, nums: List[int]) -> int:

        if not nums:
            return -1

        # binary search on the index of the nums
        left, right = 0, len(nums) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            # not using nums[mid] > nums[left] because the array can be not rotated
            if nums[mid] > nums[right]:
                left = mid
            else:
                right = mid

        return min(nums[left], nums[right])
```

---

:orange_book: [62 · Search in Rotated Sorted Array](https://www.lintcode.com/problem/62/): Identify which segment is [mid] located at

```
class Solution:
    """
    @param a: an integer rotated sorted array
    @param target: an integer to be searched
    @return: an integer
    """
    def search(self, a: List[int], target: int) -> int:
        
        if not a:
            return -1

        left, right = 0, len(a) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            # numerous cases

              #
             # 
            #    
                 #
                #

            if a[mid] > a[right]: # on top-left segment
                if a[left] <= target <= a[mid]:
                    right = mid
                else:
                    left = mid

            else: # on bottom-right segment
                if a[mid] <= target <= a[right]:
                    left = mid
                else:
                    right = mid
        
        if a[left] == target:
            return left
        if a[right] == target:
            return right
        
        return -1
```

---

:orange_book: [75 · Find Peak Element](https://www.lintcode.com/problem/75/): Identify which segment is [mid] located at

```
class Solution:
    """
    @param A: An integers array.
    @return: return any of peek positions.
    """
    def findPeak(self, A):
        # write your code here
        if not A:
            return -1

        # idea: find the minimum index s.t. it is a peak

        if A[0] > A[1]: # first element is a peak
            return 0
        
        if A[-1] > A[-2]: # last element is a peak
            return len(A) - 1

        left, right = 0, len(A) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            if A[mid] > A[mid - 1]: # going upwards
                left = mid
            else: # going downwards
                right = mid
            
        return left if A[left] > A[right] else right
```

---

:closed_book: [183 · Wood Cut](https://www.lintcode.com/problem/183/): Maximize length of small pieces [mid] s.t. [mid] satisfies the given conditions

```
class Solution:
    """
    @param l: Given n pieces of wood with length L[i]
    @param k: An integer
    @return: The maximum length of the small pieces
    """

    def wood_cut(self, l: List[int], k: int) -> int:

        if not l:
            return 0

        def feasible(L, mid) -> bool:
            count = 0
            for length in L:
                count += length // mid
            return count >= k

        # binary search on the length of the small pieces
        left, right = 1, max(l)

        while left + 1 < right:
            mid = (left + right) >> 1

            if feasible(l, mid): # if feasible, look for larger results (maximize)
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

## Chapter 10: Queues

:green_book: [492 · Implement Queue by Linked List](https://www.lintcode.com/problem/492/): Implementing enqueue and dequeue (using Linked Lists)

```
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None

class MyQueue: # queue: first in first out
    """
    @param: item: An integer
    @return: nothing
    """

    def __init__(self):
        # two pointers, one pointing at the head Node and one at the tail Node
        self.head, self.tail = None, None

    def enqueue(self, item):
        if self.head is None: # if no element, add this item as the single element
            self.head = Node(item)
            self.tail = self.head # since we only have this one element
        
        else:
            self.tail.next = Node(item) # add item as the last element
            self.tail = self.tail.next

    """
    @return: An integer
    """
    def dequeue(self): # dequeue from head to tail
        
        if self.head is not None:
            item = self.head.val
            self.head = self.head.next
            return item
        
        return -1
```

---

## Chapter 11: BFS and Graphs

:green_book: [69 · Binary Tree Level Order Traversal](https://www.lintcode.com/problem/69/): BFS using single queue / double queue / dummy nodes

- **Single Queue**

```
class Solution:
    """
    @param root: A Tree
    @return: Level order a list of lists of integer
    """
    def level_order(self, root: TreeNode) -> List[List[int]]:
        # using single queue
        from collections import deque
        result = []

        if not root:
            return []
        if not root.left and not root.right:
            return [root.val]

        # put the root node in the queue
        queue = deque([root])

        while queue:

            # store the values of all nodes in this level
            result.append([node.val for node in queue])
            
            # expand each node in current level
            for _ in range(len(queue)):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

        return result
```

---

- **Double Queue**

```
class Solution:
    """
    @param root: A Tree
    @return: Level order a list of lists of integer
    """
    def level_order(self, root: TreeNode) -> List[List[int]]:
        # using two queues

        if not root:
            return []
        if not root.left and not root.right:
            return [root.val]

        result = []
        current_queue = [root] # current level

        while current_queue:
            next_queue = [] # next level
            result.append([node.val for node in current_queue])

            for node in current_queue:
                if node.left:
                    next_queue.append(node.left)
                if node.right:
                    next_queue.append(node.right)
            
            current_queue = next_queue
        
        return result
```

---

- **Dummy Nodes (not that recommended since it's not quite straightforward)**:

```
class Solution:
    """
    @param root: A Tree
    @return: Level order a list of lists of integer
    """
    def level_order(self, root: TreeNode) -> List[List[int]]:
        # using dummy nodes (appending None after storing all nodes of a level)
        from collections import deque

        if not root:
            return []
        if not root.left and not root.right:
            return [root.val]

        queue = deque([root, None])
        results, level = [], []

        while queue:
            node = queue.popleft()
            if node is None: # we have reached the end of this level
                results.append(level)
                level = []
                if queue:
                    queue.append(None)
                continue
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        return results
```

---

:orange_book: [1179 · Friend Circles](https://www.lintcode.com/problem/1179/): BFS with a dictionary to track visited nodes

```
from collections import deque

class Solution:
    """
    @param m: a matrix
    @return: the total number of friend circles among all the students
    """
    def find_circle_num(self, m: List[List[int]]) -> int:
        # BFS using queue
        return self.bfs(m)


    def bfs(self, m):

        n = len(m) # total number of people
        answer = 0 # if a person has not been visited, then we have a new circle

        visited = {} # note down who we have already visited
        for i in range(n):
            visited[i] = False # initially we have not visited any of them

        # enumerate each person, if it is not visted, do BFS from it
        for i in range(n):
            if not visited[i]: # first time we visited this person, so we have a new circle
                answer += 1
                visited[i] = True
                queue = deque([i]) # BFS from this person

                while queue:
                    # find a person
                    now = queue.popleft()
                    # visit its friends
                    for j in range(n): # if we find a person who is not visited and is friend
                        if m[now][j] == 1 and visited[j] == False:
                            visited[j] = True
                            queue.append(j)

        return answer
```


---

## Chapter 12: DFS Traversal on BST

:green_book: [480 · Binary Tree Paths](https://www.lintcode.com/problem/480/): DFS on BST

```
class Solution:
    """
    @param root: the root of the binary tree
    @return: all root-to-leaf paths
             we will sort your return value in output
    """
    def binary_tree_paths(self, root: TreeNode) -> List[str]:
        # DFS divide-and-conquer

        if not root:
            return []
        if not root.left and not root.right:
            return [str(root.val)]

        result = []
        self.dfs(root, "", result)
        
        return result


    def dfs(self, node, path, result):
        if not node.left and not node.right:
            result.append(path + str(node.val))
            return
        
        if node.left: # DFS on left subtree
            self.dfs(node.left, path + str(node.val) + "->", result)
        if node.right: # DFS on right subtree
            self.dfs(node.right, path + str(node.val) + "->", result)
```

---

:green_book: [93 · Balanced Binary Tree](https://www.lintcode.com/problem/93/): DFS on BST with multiple return values

```
class Solution:
    """
    @param root: The root of binary tree.
    @return: True if this Binary tree is Balanced, or false.
    """
    def is_balanced(self, root: TreeNode) -> bool:
        
        if not root or (not root.left and not root.right):
            return True

        return self.divide_and_conquer(root)[0]

    def divide_and_conquer(self, root):
        # [whether is balanced], [maximum depth]

        if not root:
            return True, 0

        is_left_balanced, left_height = self.divide_and_conquer(root.left)
        is_right_balanced, right_height = self.divide_and_conquer(root.right)
        root_height = max(left_height, right_height) + 1

        if not is_left_balanced or not is_right_balanced:
            return False, root_height
        
        # if left and right both balanced, check if their heights do not differ by more than 1
        if abs(left_height - right_height) > 1: # never differ by MORE THAN 1
            return False, root_height

        return True, root_height
```

---

:green_book: [97 · Maximum Depth of Binary Tree](https://www.lintcode.com/problem/97/): DFS on BST

```
class Solution:
    """
    @param root: The root of binary tree.
    @return: An integer
    """
    def max_depth(self, root: TreeNode) -> int:
        
        if not root:
            return 0
        if not root.left and not root.right:
            return 1

        return self.find_max_depth(root)

    def find_max_depth(self, root):
        # find the maximum depth of the subtree beginning from [root]
        if not root:
            return 0
        
        left_depth = self.find_max_depth(root.left) + 1
        right_depth = self.find_max_depth(root.right) + 1

        return max(left_depth, right_depth)
```

---

:green_book: [628 · Maximum Subtree](https://www.lintcode.com/problem/628/): DFS on BST with multiple return values

```
class Solution:
    """
    @param root: the root of binary tree
    @return: the maximum weight node
    """
    def findSubtree(self, root):
        sum, max_sum, max_subtree = self.get_sum(root)
        return max_subtree
    
    # sum, maximum sum, root
    def get_sum(self, root):

        if root is None:
            return -sys.maxsize, -sys.maxsize, None

        left_sum, left_max_sum, left_max_subtree = self.get_sum(root.left)
        right_sum, right_max_sum, right_max_subtree = self.get_sum(root.right)
        max_sum = sys.maxsize

        sum = root.val
        if root.left is not None:
            sum += left_sum
        if root.right is not None:
            sum += right_sum
        max_sum = max(left_max_sum, right_max_sum, sum)
        
        if sum == max_sum:
            return sum, max_sum, root
        if left_max_sum == max_sum:
            return sum, max_sum, left_max_subtree

        return sum, max_sum, right_max_subtree
```

---

## Chapter 12: Non-recursion Traversal on BST

:closed_book: [86 · Binary Search Tree Iterator](https://www.lintcode.com/problem/86/): Using stack

```
class BSTIterator:
    """
    @param: root: The root of binary tree.
    """
    def __init__(self, root):
        # do intialization if necessary
        self.stack = []
        while root: # go to the left most element and record path taken
            self.stack.append(root)
            root = root.left

    """
    @return: True if there has next node, or false
    """
    def hasNext(self):
        return len(self.stack) > 0

    """
    @return: return next node (return the next smallest element in the BST)
    """
    def _next(self):
        node = self.stack[-1] # the last element of the stack
        if node.right: # if it has a right child, find the smallest value in right subtree
            n = node.right
            while n:
                self.stack.append(n)
                n = n.left

        else:
            n = self.stack.pop()
            while self.stack and self.stack[-1].right == n: # this node is right child of parent
                n = self.stack.pop()
        
        return node
```

---

:green_book: [900 · Closest Binary Search Tree Value](https://www.lintcode.com/problem/900/): Basic binary search tree traversal

```
class Solution:
    """
    @param root: the given BST
    @param target: the given target
    @return: the value in the BST that is closest to the target
    """
    def closest_value(self, root: TreeNode, target: float) -> int:
        closest = root.val

        while root:
            if abs(closest - target) >= abs(root.val - target): # if we get a closer choice
                closest = root.val
            if root.val < target:
                root = root.right
            else:
                root = root.left

        return closest
```

---

:orange_book: [137 · Clone Graph](https://www.lintcode.com/problem/137/): BFS to find all nodes, then create mappings for new nodes and populate edge connections

```
from collections import deque

class Solution:
    """
    @param node: A undirected graph node
    @return: A undirected graph node
    """
    def clone_graph(self, node: UndirectedGraphNode) -> UndirectedGraphNode:
        # using BFS (queue)
        # 3 steps: find nodes, copy nodes, copy edges

        if not node:
            return None

        # find nodes by BFS from this node
        nodes = self.find_nodes_by_bfs(node)
        # copy nodes (the mapping)
        mapping = self.copy_nodes(nodes)
        # copy edges
        self.copy_edges(nodes, mapping)

        return mapping[node] # return the node in the cloned graph

    def find_nodes_by_bfs(self, node):
        queue = deque([node])
        visited = set([node]) # save all the visted nodes without repetition

        while queue:
            current_node = queue.popleft()
            for neighbor in current_node.neighbors:
                if neighbor in visited:
                    continue
                queue.append(neighbor)
                visited.add(neighbor)

        return visited

    def copy_nodes(self, nodes):
        # map each old node to new ones
        mapping = {}
        for node in nodes:
            mapping[node] = UndirectedGraphNode(node.label)
        return mapping

    def copy_edges(self, nodes, mapping):
        # populate the [neighbors] for the mapping
        for node in nodes:
            new_node = mapping[node]
            for neighbor in node.neighbors:
                new_neighbor = mapping[neighbor]
                new_node.neighbors.append(new_neighbor)
```

---

:closed_book: [120 · Word Ladder](https://www.lintcode.com/problem/120/): Change problem to a graph and run BFS with set

```
class Solution:
    """
    @param start: a string
    @param end: a string
    @param dict: a set of string
    @return: An integer
    """
    def ladder_length(self, start: str, end: str, dict: Set[str]) -> int:
        
        dict.add(end) # we must include the end word, but no need to add the start word
        
        from collections import deque
        queue = deque([start])
        visited = set([start])

        distance = 0
        
        while queue: # BFS
            distance += 1
            for i in range(len(queue)): # for each node in this level
                word = queue.popleft()
                if word == end: # if this word is the end, we have reached the end
                    return distance

                for next_word in self.get_next_words(word, dict):
                    if next_word in visited:
                        continue
                    queue.append(next_word)
                    visited.add(next_word)
        
        return 0

    def get_next_words(self, word, dict):
        # return a list of possible choices
        next_words = []

        for i in range(len(word)):
            left, right = word[:i], word[i + 1:]
            for char in "abcdefghijklmnopqrstuvwxyz": # change to each possible word
                if word[i] == char:
                    continue
                new_word = left + char + right # replace the i-th character to a new char
                if new_word in dict:
                    next_words.append(new_word)
            
        return next_words
```

---

:green_book: [433 · Number of Islands](https://www.lintcode.com/problem/433/): BFS on 2D matrix, need to identify whether step is valid

```
DIRECTIONS = [(1, 0), (0, 1), (-1, 0), (0, -1)]
from collections import deque

class Solution:
    """
    @param grid: a boolean 2D matrix
    @return: an integer
    """
    def num_islands(self, grid: List[List[bool]]) -> int:
        
        if not grid:
            return 0

        num_islands = 0
        visited = set()

        for i in range(len(grid)): # row
            for j in range(len(grid[0])): # col
                # if this point is an insland and hasn't been visited
                if grid[i][j] == 1 and (i, j) not in visited:
                    self.bfs(grid, i, j, visited)
                    num_islands += 1
        
        return num_islands

    def bfs(self, grid, x, y, visited):
        queue = deque([(x, y)])
        visited.add((x, y))
        while queue:
            x, y = queue.popleft()
            for delta_x, delta_y in DIRECTIONS:
                next_x = x + delta_x
                next_y = y + delta_y 
                
                if not self.is_valid(grid, next_x, next_y, visited):
                    continue

                queue.append((next_x, next_y))
                visited.add((next_x, next_y))

    def is_valid(self, grid, x, y, visited):
        if not (0 <= x < len(grid) and 0 <= y < len(grid[0])):
            return False
        if (x, y) in visited:
            return False
        return True if grid[x][y] == 1 else False
```

---

:orange_book: [611 · Knight Shortest Path](https://www.lintcode.com/problem/611/): BFS on 2D matrix, need to identify whether step is valid

```
OFFSETS = [(1, 2), (1, -2), (-1, 2), (-1, -2),
           (2, 1), (2, -1), (-2, 1), (-2, -1)]

class Solution:
    """
    @param grid: a chessboard included 0 (false) and 1 (true)
    @param source: a point
    @param destination: a point
    @return: the shortest path 
    """
    def shortest_path(self, grid: List[List[bool]], source: Point, destination: Point) -> int:
        
        if not grid:
            return -1
        from collections import deque

        cell_to_des_dist = {(source.x, source.y): 0}
        queue = deque([(source.x, source.y)])

        while queue:
            x, y = queue.popleft()
            if (x, y) == (destination.x, destination.y): # we reached destination
                return cell_to_des_dist[(x, y)]        

            for dx, dy in OFFSETS:
                next_x = x + dx
                next_y = y + dy

                if not self.is_valid(next_x, next_y, grid, cell_to_des_dist):
                    continue
                
                queue.append((next_x, next_y))
                # we went one more step but didn't reach the destination
                cell_to_des_dist[(next_x, next_y)] = cell_to_des_dist[(x, y)] + 1

        return -1
        
    def is_valid(self, x, y, grid, cell_to_des_dist):
        if x < 0 or x >= len(grid) or y < 0 or y >= len(grid[0]): # out of board range
            return False
        if (x, y) in cell_to_des_dist: # we have visited
            return False
        return True if grid[x][y] == 0 else False # this place should be empty
```

---

:orange_book: [616 · Course Schedule II](https://www.lintcode.com/problem/616/): Topological sorting with adjacency, in degree, and BFS manipulation

```
class Solution:
    """
    @param num_courses: a total of n courses
    @param prerequisites: a list of prerequisite pairs
    @return: the course order
    """
    def find_order(self, num_courses: int, prerequisites: List[List[int]]) -> List[int]:
        # idea: find the in-degree of each node, 
        # put each node with 0 in-degree into a queue as starting nodes
        # Repeat: take one node from queue and -1 in-degree for connected nodes
        # if we find a new node with 0 in-degree, add it to queue

        # create an adjacency matrix (store neighboring nodes)
        graph = [[] for i in range(num_courses)]
        
        # identify the in-degree of each node and construct adjacency matrix
        in_degree = [0] * num_courses
        for node_in, node_out in prerequisites:
            graph[node_out].append(node_in)
            in_degree[node_in] += 1

        # put each node with in-degree = 0 into a queue
        from collections import deque
        queue = deque()

        for i in range(num_courses):
            if in_degree[i] == 0: # we can learn this class
                queue.append(i)

        topo_order = []

        while queue:
            now_pos = queue.popleft()
            topo_order.append(now_pos)

            # for each neighboring node, in-degree decrement by 1
            for next_pos in graph[now_pos]:
                in_degree[next_pos] -= 1
                if in_degree[next_pos] == 0:
                    queue.append(next_pos)

        # return topological order if we have finished all required courses
        # else there are classes that is impossible to take what-so-ever
        return topo_order if len(topo_order) == num_courses else []
```

---

:closed_book: [892 · Alien Dictionary](https://www.lintcode.com/problem/892/): Topological sorting with adjacency, in degree, and BFS with heapify manipulation

```
from heapq import heappush, heappop, heapify

class Solution:
    """
    @param words: a list of words
    @return: a string which is correct order
    """
    def alien_order(self, words: List[str]) -> str:
        
        if not words:
            return ""

        graph = self.build_graph(words)
        if not graph:
            return ""
        
        return self.topological_sort(graph)
    
    def build_graph(self, words):
        # <char, subsequent chars>
        graph = {}

        # create the nodes
        for word in words:
            for c in word:
                if c not in graph:
                    graph[c] = set()

        # create the edges
        for i in range(len(words) - 1): # compare each word with the subsequent one
            for j in range(min(len(words[i]), len(words[i + 1]))):
                if words[i][j] != words[i+1][j]:
                    graph[words[i][j]].add(words[i + 1][j])
                    break
                # e.g. ["abc", "ab"] where "abc" is before "ab", illegal
                if j == min(len(words[i]), len(words[i + 1])) - 1:
                    if len(words[i]) > len(words[i + 1]):
                        return None
        
        return graph

    def topological_sort(self, graph):
        indegree = self.get_indegree(graph)
        queue = [node for node in graph if indegree[node] == 0] # put nodes with indegree 0 in queue

        heapify(queue)
        topo_order = ""

        while queue:
            node = heappop(queue)
            topo_order += node
            for neighbor in graph[node]:
                indegree[neighbor] -= 1
                if indegree[neighbor] == 0:
                    heappush(queue, neighbor)

        return topo_order if len(topo_order) == len(graph) else ""

    def get_indegree(self, graph):
        indegree = {node: 0 for node in graph}

        for node in graph:
            for neighbor in graph[node]: # it has node as an ancestor
                indegree[neighbor] += 1     
        
        return indegree
```

---

:green_book: [596 · Minimum Subtree](https://www.lintcode.com/problem/596/): Similar to [628 · Maximum Subtree](https://www.lintcode.com/problem/628/) but using global variables

```
class Solution:
    """
    @param root: the root of binary tree
    @return: the root of the minimum subtree
    """
    def find_subtree(self, root: TreeNode) -> TreeNode:
        
        if not root:
            return None
        if not root.left and not root.right:
            return root
        
        self.minimum_sum = float('inf') # global variable
        self.minimum_node = None # global variable
        self.get_tree_sum(root)

        return self.minimum_node
        
    def get_tree_sum(self, root): # get the sum of the current tree from [root]
        if not root:
            return 0
        
        left_sum = self.get_tree_sum(root.left)
        right_sum = self.get_tree_sum(root.right)
        current_sum = left_sum + right_sum + root.val

        if current_sum <= self.minimum_sum:
            self.minimum_sum = current_sum
            self.minimum_node = root

        return current_sum
```

---

:green_book: [474 · Lowest Common Ancestor II](https://www.lintcode.com/problem/474/): LCA but with parent pointer

```
class Solution:
    """
    @param: root: The root of the tree
    @param: A: node in the tree
    @param: B: node in the tree
    @return: The lowest common ancestor of A and B
    """
    def lowestCommonAncestorII(self, root, A, B):
        
        parent_set = set()

        curr = A
        while curr: # put all ancestors of A into the set
            parent_set.add(curr)
            curr = curr.parent

        curr = B
        while curr:
            if curr in parent_set: # in the order from down to up to ensure LEAST common ancestor
                return curr
            curr = curr.parent
        
        return None
```

---

:orange_book: [88 · Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/88/): LCA but regular without parent pointer

```
class Solution:
    """
    @param: root: The root of the binary tree.
    @param: A: A TreeNode in a Binary.
    @param: B: A TreeNode in a Binary.
    @return: Return the least common ancestor(LCA) of the two nodes.
    """
    def lowestCommonAncestor(self, root, A, B):
        
        if not root:
            return None

        # idea: find where A and B exists

        if root == A or root == B:
            return root
        
        left = self.lowestCommonAncestor(root.left, A, B)  
        right = self.lowestCommonAncestor(root.right, A, B)

        if left and right: # if A and B on either side, this is the LCA
            return root
        if left: # if A and B both on left, go left
            return left
        if right: # if A and B both on right, go right
            return right
        return None # A and B does not exist in this tree
```

---

:orange_book: [578 · Lowest Common Ancestor III](https://www.lintcode.com/problem/578/): LCA but does not guarantee that A, or B, or LCA exists

```
class Solution:
    """
    @param: root: The root of the binary tree.
    @param: A: A TreeNode
    @param: B: A TreeNode
    @return: Return the LCA of the two nodes.
    """
    def lowestCommonAncestor3(self, root, A, B):

        if not root:
            return None

        a_exists, b_exists, lca = self.helper(root, A, B)
        return lca if a_exists and b_exists else None
    
    def helper(self, root, A, B):
        # return: a exists, b exists, LCA node

        if not root:
            return False, False, None
        
        left_a_exists, left_b_exists, left_node = self.helper(root.left, A, B)
        right_a_exists, right_b_exists, right_node = self.helper(root.right, A, B)

        a_exists = left_a_exists or right_a_exists or root == A
        b_exists = left_b_exists or right_b_exists or root == B

        if root == A or root == B:
            return a_exists, b_exists, root
        
        if left_node and right_node:
            return a_exists, b_exists, root
        
        if left_node:
            return a_exists, b_exists, left_node
        
        if right_node:
            return a_exists, b_exists, right_node
        
        return a_exists, b_exists, None
```

---

:green_book: [453 · Flatten Binary Tree to Linked List](https://www.lintcode.com/problem/453/): Break up to left and right tree and flatten and connect using DFS

```
class Solution:
    """
    @param root: a TreeNode, the root of the binary tree
    @return: nothing
    """
    def flatten(self, root: TreeNode):
        # idea: DFS pre-order traversal, then put things together
        self.flatten_and_return_last_node(root)

    def flatten_and_return_last_node(self, root): # return last node after flattening
        if not root:
            return None
        
        left_last = self.flatten_and_return_last_node(root.left)
        right_last = self.flatten_and_return_last_node(root.right)

        if left_last: # we need to flatten
            left_last.right = root.right
            root.right = root.left
            root.left = None
        
        return right_last or left_last or root
```

---

:orange_book: [902 · Kth Smallest Element in a BST](https://www.lintcode.com/problem/902/description): In-order traversal on BST and find k-th traversed element

```
class Solution:
    """
    @param root: the given BST
    @param k: the given k
    @return: the kth smallest element in BST
    """
    def kth_smallest(self, root: TreeNode, k: int) -> int:
        # idea: use in-order traversal and find the k-th traversed element

        self.result = None
        self.count = 0
        self.inorder(root, k)
        return self.result
    
    def inorder(self, root, k):
        if not root:
            return

        self.inorder(root.left, k)

        self.count += 1
        if k == self.count:
            self.result = root.val

        self.inorder(root.right, k)
```

---

## Chapter 16: DFS for Combination

:orange_book: [17 · Subsets](https://www.lintcode.com/problem/17/): DFS for combination + iterating each possible subset length

```
class Solution:
    """
    @param nums: A set of numbers
    @return: A list of lists
             we will sort your return value in output
    """
    def subsets(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return [[]]
        
        result = []
        nums = sorted(nums) # since solution set must be non-descending

        for i in range(len(nums) + 1): # length of the subset
            self.dfs(nums, i, [], result)
        
        return result
        
    
    def dfs(self, candidates, target, path, result):
        if target == 0:
            result.append(path)
            return
        if target < 0:
            return
        
        # idea: combination
        for i in range(len(candidates)):
            self.dfs(candidates[i+1:], target - 1, path + [candidates[i]], result)
```

---

:orange_book: [18 · Subsets II](https://www.lintcode.com/problem/18): DFS for combination + iterating each possible subset length + duplicate removal

```
class Solution:
    """
    @param nums: A set of numbers.
    @return: A list of lists. All valid subsets.
             we will sort your return value in output
    """
    def subsets_with_dup(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return [[]]
        
        result = []
        nums = sorted(nums)

        for i in range(len(nums) + 1):
            self.dfs(nums, i, [], result)
        
        return result
    
    def dfs(self, candidates, target, path, result):
        if target == 0:
            result.append(path)
            return 
        if target < 0:
            return
        
        for i in range(len(candidates)):
            if i > 0 and candidates[i] == candidates[i - 1]: # avoid duplicates
                continue
            
            self.dfs(candidates[i + 1:], target - 1, path + [candidates[i]], result)
```

---

## Chapter 17: DFS for Permutation

:orange_book: [15 · Permutations](https://www.lintcode.com/problem/15/): DFS for permutation

```
class Solution:
    """
    @param nums: A list of integers.
    @return: A list of permutations.
             we will sort your return value in output
    """
    def permute(self, nums: List[int]) -> List[List[int]]:
        
        if not nums:
            return [[]]
        
        result = []
        nums = sorted(nums)
    
        self.dfs(nums, [], result)
        return result
    
    
    def dfs(self, candidates, path, result):
        if len(candidates) == 0:
            result.append(path)
            return 
        
        for i in range(len(candidates)): # assume no duplicate numbers in input
            self.dfs(candidates[:i] + candidates[i+1:], path + [candidates[i]], result)
```

---

:orange_book: [16 · Permutations II](https://www.lintcode.com/problem/16/): DFS for permutation + duplicate removal

```
class Solution:
    """
    @param nums: A list of integers
    @return: A list of unique permutations
             we will sort your return value in output
    """
    def permute_unique(self, nums: List[int]) -> List[List[int]]:
        
        if not nums:
            return [[]]
        
        result = []
        nums = sorted(nums)

        self.dfs(nums, [], result)
        return result

        

    def dfs(self, candidates, path, result):
        if len(candidates) == 0:
            result.append(path)
        
        for i in range(len(candidates)):
            if i > 0 and candidates[i] == candidates[i - 1]:
                continue
            self.dfs(candidates[:i] + candidates[i+1:], path + [candidates[i]], result)
```

---

:closed_book: [816 · Traveling Salesman Problem](https://www.lintcode.com/problem/816/): DFS for permutation + graph building

- **Teacher's Method**:

```
class Solution:
    """
    @param n: an integer,denote the number of cities
    @param roads: a list of three-tuples,denote the road between cities
    @return: return the minimum cost to travel all cities
    """

    def min_cost(self, n: int, roads: List[List[int]]) -> int:

        graph = self.build_graph(n, roads)
        self.result = float('inf')
        self.dfs(1, set([1]), 0, graph)

        return self.result

    
    def dfs(self, city, visited, cost, graph): # dfs from [start] city

        if len(visited) == len(graph):
            self.result = min(self.result, cost)
            return

        for next_city in graph[city]: # for each reachable neighboring city
            if next_city in visited:
                continue
            visited.add(next_city)

            self.dfs(next_city, visited, cost + graph[city][next_city], graph)

            visited.remove(next_city)


    def build_graph(self, n, roads):

        # graph[i][j] = cost between city i and city j
        graph = { i : {} for i in range(1, n + 1)} # cities: 1, ... , n

        for city_a, city_b, cost in roads:
            if city_b not in graph[city_a]:
                graph[city_a][city_b] = cost
            else: # since there can be two roads between two cities
                graph[city_a][city_b] = min(graph[city_a][city_b], cost)

            if city_a not in graph[city_b]:
                graph[city_b][city_a] = cost
            else: # since there can be two roads between two cities
                graph[city_b][city_a] = min(graph[city_b][city_a], cost)

        return graph
```

---

## Chapter 18: Hash Table

:green_book: [128 · Hash Function](https://www.lintcode.com/problem/128/): Basic hashing


```
class Solution:
    """
    @param key: A string you should hash
    @param h_a_s_h__s_i_z_e: An integer
    @return: An integer
    """
    def hash_code(self, key: str, h_a_s_h__s_i_z_e: int) -> int:
        answer = 0
        for i in range(len(key)):
            answer = ((answer * 33) + ord(key[i])) % h_a_s_h__s_i_z_e
        
        # e.g., 10 * 33^2 + 20 * 33^1 + 30 * 33^0
        #     = (10 * 33 + 20) * 33 + 30

        return answer
```

---

:orange_book: [129 · Rehashing](https://www.lintcode.com/problem/129/): Re-calculating hash index and re-inserting all elements


```
class Solution:
    """
    @param hashTable: A list of The first node of linked list
    @return: A list of The first node of linked list which have twice size
    """
    def rehashing(self, hashTable):
        
        # 算扩容之后的 size (origin_len * 2)
        # 遍历原来的 hashtable 和每个 listnode
        # 重算在 new_hashtable 的 index
        # 在新的位置已经有 node 就到 tails 里面找 tail 串，否则就初始化 heads[i] 和 tails[i]

        if not hashTable:
            return
        
        CAPACITY = len(hashTable) * 2

        heads = [None] * CAPACITY
        tails = [None] * CAPACITY

        for node in hashTable:
            cur = node

            while cur:
                i = cur.val % CAPACITY # recalculate hash index
                _node = ListNode(cur.val) # copy the node

                if heads[i]: # if this position is occupied
                    tails[i].next = _node # add node to the end of index i
                else:
                    heads[i] = _node # add node as first of index i
                
                tails[i] = _node # consider this node as the last node of index i

                cur = cur.next
            
        return heads
```

---

## Chapter 19: Heap

:orange_book: [130 · Heapify](https://www.lintcode.com/problem/130/): Using sift-down method

```
class Solution:
    """
    @param a: Given an integer array
    @return: nothing
    """
    def heapify(self, a: List[int]):
        
        # idea: sift the lower half down
        for i in reversed(range(len(a) // 2)):
            self.sift_down(a, i)
        
    def sift_down(self, nums, index):
       
        while index < len(nums):
            left_child = index * 2 + 1
            right_child = index * 2 + 2
            smallest = index # store the index of the smallest element

            # compare with left child
            if left_child < len(nums) and nums[left_child] < nums[smallest]:
                smallest = left_child
            # compare with right child
            if right_child < len(nums) and nums[right_child] < nums[smallest]:
                smallest = right_child

            if index == smallest: # if we did not move at all, stop
                break
            
            nums[smallest], nums[index] = nums[index], nums[smallest] # exchange elements
            
            index = smallest # go to the exchanged position
```

---

## Chapter 20 (LIVE): DFS

:orange_book: [425 · Letter Combinations of a Phone Number](https://www.lintcode.com/problem/425/)

```
KEYBOARD = {"2": "abc", "3": "def", "4":"ghi", "5":"jkl", "6":"mno", "7":"pqrs", "8":"tuv", "9":"wxyz"}

class Solution:
    """
    @param digits: A digital string
    @return: all possible letter combinations
             we will sort your return value in output
    """
    def letter_combinations(self, digits: str) -> List[str]:
        # DFS

        if not digits:
            return []

        result = []
        self.dfs(digits, 0, "", result) # start with all digits and from first digit

        return result

    def dfs(self, candidates, target, path, result):
        if target >= len(candidates): # we have gone through all digits
            result.append(path)
            return

        for letter in KEYBOARD[candidates[target]]: # all letters corresponding to this digit
            self.dfs(candidates, target + 1, path + letter, result)
```

---

:orange_book: [90 · k Sum II](https://www.lintcode.com/problem/90/)

```
class Solution:
    """
    @param a: an integer array
    @param k: a postive integer <= length(A)
    @param target: an integer
    @return: A list of lists of integer
             we will sort your return value in output
    """
    def k_sum_i_i(self, a: List[int], k: int, target: int) -> List[List[int]]:
        # DFS

        if not a:
            return []

        result = []
        self.dfs(a, k, target, [], result)

        return result

    def dfs(self, candidates, k, target, path, result):
        if k == 0 and target == 0: # we used k numbers and achieved the required sum
            result.append(path)
            return
        
        # scenarios in which we cannot find a valid answer
        if k == 0 and target != 0: # if we used k number but not reached target
            return
        if target <= 0 and k != 0: # we have already reached target but not using k numbers
            return 

        for i in range(len(candidates)):
            self.dfs(candidates[i+1:], k - 1, target - candidates[i], path + [candidates[i]], result)
```

---

:orange_book: [135 · Combination Sum](https://www.lintcode.com/problem/135/)

```
class Solution:
    """
    @param candidates: A list of integers
    @param target: An integer
    @return: A list of lists of integers
             we will sort your return value in output
    """
    def combination_sum(self, candidates: List[int], target: int) -> List[List[int]]:
        # DFS

        if not candidates:
            return []

        result = []
        self.dfs(candidates, target, [], result)
        return result
    
    def dfs(self, candidates, target, path, result):
        if target == 0:
            result.append(sorted(path)) # Note: we only want sorted results
            return

        if target < 0: # already invalid, early stop
            return
        
        for i in range(len(candidates)):
            if i > 0 and candidates[i] == candidates[i - 1]: # remove duplicates (input may contain duplciate numbers)
                continue
            self.dfs(candidates[i:], target - candidates[i], path + [candidates[i]], result) # Notice the [i:] here, since we allow reusing numbers
```

---

:orange_book: [10 · String Permutation II](https://www.lintcode.com/problem/10/)

```
class Solution:
    """
    @param str: A string
    @return: all permutations
             we will sort your return value in output
    """
    def string_permutation2(self, str: str) -> List[str]:
        # DFS

        if not str:
            return [""]
        
        str = sorted(str)

        result = []
        self.dfs(str, "", result)
        return result
    
    def dfs(self, candidates, path, result):
        if len(candidates) == 0:
            result.append(path)
            return
        
        for i in range(len(candidates)):
            if i > 0 and candidates[i] == candidates[i - 1]:
                continue
            self.dfs(candidates[:i] + candidates[i+1:], path + candidates[i], result)
```

---

:closed_book: [https://www.lintcode.com/problem/132/](https://www.lintcode.com/problem/132/)

```
TODO
```

---

---

## Chapter 22: Memoization Search

:orange_book: [109 · Triangle](https://www.lintcode.com/problem/109/): DFS but using memoization with HashMap to save intermediate results

```
class Solution:
    """
    @param triangle: a list of lists of integers
    @return: An integer, minimum path sum
    """
    def minimum_total(self, triangle: List[List[int]]) -> int:
        return self.divide_conquer(triangle, 0, 0, {})

    # memo: {(x, y): the minimum cost from (x, y) to bottom}
    def divide_conquer(self, triangle, x, y, memo):
        if x == len(triangle): # we have reached the bottom level
            return 0
        
        if (x, y) in memo: # we already searched here
            return memo[(x, y)]

        left = self.divide_conquer(triangle, x + 1, y, memo)
        right = self.divide_conquer(triangle, x + 1, y + 1, memo)

        memo[(x, y)] = min(left, right) + triangle[x][y]
        return memo[(x, y)]
```

---

:green_book: [1300 · Bash Game](https://www.lintcode.com/problem/1300/): DP will timeout, so using a clever method in O(1)

```
class Solution:
    """
    @param n: an integer
    @return: whether you can win the game given the number of stones in the heap
    """
    def can_win_bash(self, n: int) -> bool:
    #         1+3=4；只要最后对方拿时，剩余石头数是4，则我方必赢，因为无论对方拿几，我方都能一次拿完；
    # 题目变为：n能不能变为4，由此发现只要我们首次取n%4个石头，对方就会从4的倍数开始取（因为我们取走了余数，剩余一定被4整除），
    # 那么接下来，无论对方取几（1,2,3都不大于4），我们总能让对方一直处于4的倍数状态，直到获胜，
    # 因此题目最终变为：n能否被4整除；如不能则我方获胜，如果能则我方失败；

        return n % 4 != 0
```

---

## Chapter 23: Introduction to the concept of Dynamic Programming

:orange_book: [109 · Triangle](https://www.lintcode.com/problem/109/): Using DP -> dp[i][j] = cost to go from (0, 0) to this position (i, j)

```
class Solution:
    """
    @param triangle: a list of lists of integers
    @return: An integer, minimum path sum
    """
    def minimum_total(self, triangle: List[List[int]]) -> int:
        # Top-down DP

        # def: dp[i][j] = cost to go from (0, 0) to this position (i, j)
        dp = [[0 for _ in range(len(triangle))] for _ in range(len(triangle))]

        # initialize dp (the left and right edge of the triangle)
        n = len(triangle)
        dp[0][0] = triangle[0][0]
        for i in range(1, n):
            # leftmost element
            dp[i][0] = dp[i - 1][0] + triangle[i][0]
            # rightmost element
            dp[i][i] = dp[i - 1][i - 1] + triangle[i][i]
        
        # fill in the other positions
        for i in range(2, n):
            for j in range(1, i):
                dp[i][j] = min(dp[i - 1][j], dp[i - 1][j - 1]) + triangle[i][j]

        # each element in dp[n - 1] is a path from top to bottom -> choose the least cost one
        return min(dp[n - 1])
```

---

:green_book: [114 · Unique Paths](https://www.lintcode.com/problem/114/): Using DP -> dp[i][j] = # of unique paths to reach (i, j) from (0, 0)

```
class Solution:
    """
    @param m: positive integer (1 <= m <= 100)
    @param n: positive integer (1 <= n <= 100)
    @return: An integer
    """
    def unique_paths(self, m: int, n: int) -> int:  
        # Top-down DP

        # def: dp[i][j] = # of unique paths to reach (i, j) from (0, 0)
        dp = [[0 for _ in range(n)] for _ in range(m)]
        
        # initialize dp
        for i in range(n): # the first row
            dp[0][i] = 1
        for j in range(m): # the first column
            dp[j][0] = 1
        
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        return dp[m - 1][n - 1]
```

---

## Chapter 24: Dynamic Programming Typical Problems

:green_book: [115 · Unique Paths II](https://www.lintcode.com/problem/115): Problem 114 but with obstacles, similar DP

```
class Solution:
    """
    @param obstacle_grid: A list of lists of integers
    @return: An integer
    """
    def unique_paths_with_obstacles(self, obstacle_grid: List[List[int]]) -> int:
        # Top-down DP

        m, n = len(obstacle_grid), len(obstacle_grid[0])

        # def: dp[i][j] = # of unique paths to reach (i, j) from (0, 0)
        dp = [[0 for _ in range(n)] for _ in range(m)]
        
        # initialize dp
        for i in range(n): # the first row
            if obstacle_grid[0][i] == 1: # if obstacle, all subsequent ones cannot reach
                break
            dp[0][i] = 1
        for j in range(m): # the first column
            if obstacle_grid[j][0] == 1: # if obstacle, all subsequent ones cannot reach
                break
            dp[j][0] = 1
        
        for i in range(1, m):
            for j in range(1, n):
                if obstacle_grid[i][j] == 1:
                    continue
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        return dp[m - 1][n - 1]
```

---

:orange_book: [630 · Knight Shortest Path II](https://www.lintcode.com/problem/630/): Using DP -> dp[i][j] = minimum steps to reach (i, j) from (0, 0)

```
# REVERSED direction since we are backtracing the position that we CAME FROM
DIRECTIONS = [
    (-1, -2),
    (1, -2),
    (-2, -1),
    (2, -1)
]

class Solution:
    """
    @param grid: a chessboard included 0 and 1
    @return: the shortest path
    """
    def shortest_path2(self, grid: List[List[bool]]) -> int:
        # Dynamic Programming -> top-down

        if not grid or not grid[0]:
            return -1

        n, m = len(grid), len(grid[0])

        # dp[i][j] = minimum steps to reach (i, j) from (0, 0)
        dp = [[float('inf') for _ in range(m)] for _ in range(n)]

        # initialize dp
        dp[0][0] = 0

        for col in range(m): # NOTE: iterating by column first because column index always increment
            for row in range(n):
                if grid[row][col] == 1: # barrier
                    continue

                for delta_x, delta_y in DIRECTIONS:
                    x, y = row + delta_x, col + delta_y
                    if 0 <= x < n and 0 <= y < m: # don't forget to check validity
                        dp[row][col] = min(dp[row][col], dp[x][y] + 1)

        if dp[n - 1][m - 1] == float('inf'):
            return -1

        return dp[n - 1][m - 1]
```

---

:orange_book: [116 · Jump Game](https://www.lintcode.com/problem/116/): Using DP -> dp[i] = whether we can reach position [i]


```
class Solution:
    """
    @param a: A list of integers
    @return: A boolean
    """
    def can_jump(self, a: List[int]) -> bool:
        # Dynamic Programming

        if not a:
            return False
        
        # dp[i] = whether we can reach position [i]
        dp = [False for _ in range(len(a))]
        
        # initialize dp 
        dp[0] = True

        for i in range(1, len(a)):
            for j in range(i):
                # we can reach [j], AND when we add [j] with maximum jump distance >= [i]
                if dp[j] and a[j] + j >= i: 
                    dp[i] = True # we can reach [i]
                    break
        
        return dp[len(a) - 1] # whether we can reach [len(a) - 1] position
```

---

:orange_book: [92 · Backpack](https://www.lintcode.com/problem/92/): Two DP methods

- **DP #1: dp[i][j] = whether we can pick numbers from first [i] candidates to form a sum of j**

```
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param a: Given n items with size A[i]
    @return: The maximum size
    """
    def back_pack(self, m: int, a: List[int]) -> int:
        # DP Approach No.1

        if not a:
            return 0

        n = len(a)

        # dp[i][j] = whether we can pick numbers from first [i] candidates
        # to form a sum of j, NOTICE the range + 1
        dp = [[False for _ in range(m + 1)] for _ in range(n + 1)]

        # initialize 
        dp[0][0] = True

        for i in range(1, n + 1): # trying out first [i] numbers in list
            dp[i][0] = True
            for j in range(1, m + 1): # try to form a sum = [j]
                if j >= a[i - 1]: # consider adding the element No. [i - 1]
                    dp[i][j] = dp[i - 1][j] or dp[i - 1][j - a[i - 1]]
                else:
                    dp[i][j] = dp[i - 1][j]

        # acquire answer
        for i in range(m, -1 , -1): # see which maximum possible sum we can achieve
            if dp[n][i]: # we have considered all elements
                return i
            
        return -1
```

- **DP #2: dp[i][j] = maximum sum <= j that we can obtain by picking numbers from the first [i] candidates**

```
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param a: Given n items with size A[i]
    @return: The maximum size
    """
    def back_pack(self, m: int, a: List[int]) -> int:
        # DP Approach No.2

        if not a:
            return 0

        n = len(a)

        # dp[i][j] = maximum sum <= j that we can obtain by picking numbers from the first [i] candidates
        dp = [[0 for _ in range(m + 1)] for _ in range(len(a) + 1)]

        # initialize
        for i in range(1, n + 1):
            for j in range(0, m + 1):
                if j >= a[i - 1]: # consider adding the element No. [i - 1]
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - a[i - 1]] + a[i - 1])
                else:
                    dp[i][j] = dp[i - 1][j]
        
        return dp[len(a)][m] # the maximum sum <= m we can achieve by considering ALL candidates
```

---

:orange_book: [125 · Backpack II](https://www.lintcode.com/problem/125/): Problem 92 but with values for each item

```
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param a: Given n items with size A[i]
    @param v: Given n items with value V[i]
    @return: The maximum value
    """
    def back_pack_i_i(self, m: int, a: List[int], v: List[int]) -> int:

        if not a:
            return 0

        n = len(a)

        # dp[i][j] = maximum sum <= j that we can obtain by picking numbers from the first [i] candidates
        dp = [[0 for _ in range(m + 1)] for _ in range(len(a) + 1)]

        # initialize
        for i in range(1, n + 1):
            for j in range(0, m + 1):
                if j >= a[i - 1]: # consider adding the element No. [i - 1]
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - a[i - 1]] + v[i - 1]) # Notice we are adding v[i - 1] here
                else:
                    dp[i][j] = dp[i - 1][j]
        
        return dp[len(a)][m] # the maximum sum <= m we can achieve by considering ALL candidates
```

---

:orange_book: [440 · Backpack III](https://www.lintcode.com/problem/440): Problem 92 but we can pick any number of the same item

```
class Solution:
    """
    @param a: an integer array
    @param v: an integer array
    @param m: An integer
    @return: an array
    """
    def back_pack_i_i_i(self, a: List[int], v: List[int], m: int) -> int:

        if not a:
            return 0

        n = len(a)

        # dp[i][j] = maximum sum <= j that we can obtain by picking numbers from the first [i] candidates
        dp = [[0 for _ in range(m + 1)] for _ in range(len(a) + 1)]

        # initialize
        for i in range(1, n + 1):
            for j in range(0, m + 1):
                if j >= a[i - 1]: # consider adding the element No. [i - 1]
                    dp[i][j] = max(dp[i - 1][j], 
                                   # Notice we are adding v[i - 1] here & using dp[i] from previous level
                                   dp[i][j - a[i - 1]] + v[i - 1]) 
                else:
                    dp[i][j] = dp[i - 1][j]
        
        return dp[len(a)][m] # the maximum sum <= m we can achieve by considering ALL candidates
```

---




