# LeetCode - Two Pointers | Solutions
- **Language**: all solutions in **Python 3**
- **Implementation**: ideas taken from various sources, organized and coded by me
- **Problem Search**: use "Ctrl + F" to search for problem ID
---

## 1. "Face-to-Face" Two Pointers

:heavy_check_mark: :orange_book: [15. 3Sum](https://leetcode.com/problems/3sum/): sort, fixing least element and running **2 Sum** with target = -(that element), **remove duplicates** (**2 Sum**)

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        
        # idea: fix one of the nums, then use 2 Sum method so that the sum of the other 2 elements = -(first element)
        
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
        
        for i in range(0, length - 2): # fix the first element and run 2 Sum on rest 
            
            # skip duplicate (nums[i] already visited in prior loop)
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            # two pointers from the element next to this first element to the last
            left, right = i + 1, length - 1
                        
            find_two_sum(left, right, -nums[i], result)
        
        return result
```

---

:heavy_check_mark: :orange_book: [LintCode 382 · Triangle Count](https://www.lintcode.com/problem/382/): sort, fixing the longest edge and running **2 Sum** on short edges with target > longest edge (**2 Sum**)

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

:heavy_check_mark: :orange_book: [LintCode 144 · Interleaving Positive and Negative Numbers](https://www.lintcode.com/problem/144/): **partition** numbers of different signs, then interleave (**Partition**)

```
class Solution:
    """
    @param a: An integer array.
    @return: nothing
    """
    def rerange(self, a: List[int]):
        # idea: first partition into negative numbers | positive numbers

        if not a or len(a) == 1:
            return a

        neg_cnt = self.partition(a)
        pos_cnt = len(a) - neg_cnt

        left = 1 if neg_cnt > pos_cnt else 0
        right = len(a) - (2 if neg_cnt < pos_cnt else 1)
        
        self.interleave(a, left, right)

    def partition(self, a):
        # partition array into negative numbers | positive numbers
        left, right = 0, len(a) - 1
        while left <= right:
            # find the numbers that are misplaced on either side
            while left <= right and a[left] < 0:
                left += 1
            while left <= right and a[right] > 0:
                right -= 1
            
            # exchange
            if left <= right:
                a[left], a[right] = a[right], a[left]
                left += 1
                right -= 1

        return left # left is the index of the first positive number

    def interleave(self, a, left, right):
        while left < right:
            a[left], a[right] = a[right], a[left]
            left += 2
            right -= 2
```

---

:wavy_dash: :orange_book: [LintCode 148 · Sort Colors](https://www.lintcode.com/problem/148/): using **quick sort - partition** (**Partition**)

```
class Solution:
    """
    @param nums: A list of integer which is 0, 1 or 2
    @return: nothing
    """
    def sort_colors(self, nums: List[int]):
        # write your code here

        def partition(nums, k):
            # point to the last element in the (< k) region
            last_small_ptr = -1

            for i in range(len(nums)):
                if nums[i] < k: # we need to exchange their value
                    last_small_ptr += 1
                    nums[last_small_ptr], nums[i] = \
                    nums[i], nums[last_small_ptr]

        partition(nums, 1)
        partition(nums, 2)

        return nums
```

---

:wavy_dash: :orange_book: [LintCode 143 · Sort Colors II](https://www.lintcode.com/problem/143/): sorting arbitrary number of colors, using **quick sort - partition & recursion** (**Partition & Recursion**)

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

        # sort left portion
        self.sort_colors(colors, color_from, mid_color, idx_from, right)
        self.sort_colors(colors, mid_color + 1, color_to, left, idx_to)
```

---

## 2. "Same-Direction" Two Pointers

:heavy_check_mark: :green_book: [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/): one pointing to the first non-replaced element, one pointing to non-zero elements, replace / swap

```
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        # Reference Solution:
        # https://leetcode.com/problems/move-zeroes/discuss/72012/Python-short-in-place-solution-with-comments.
        # Visualization: https://leetcode.com/problems/move-zeroes/discuss/72012/Python-short-in-place-solution-with-comments./1150489
        
        zero = 0  # pointer to the left most 0
        for i in range(len(nums)):
            
            if nums[i] != 0:
                nums[zero] = nums[i]
                zero += 1
                
        while zero < len(nums):
            nums[zero] = 0
            zero += 1
        
        return nums
```

---


