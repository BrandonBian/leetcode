# Two Pointers & Sliding Window · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. "Face-to-Face" Two Pointers

<a id="lc-15"></a>
:heavy_check_mark: :orange_book: [15. 3Sum](https://leetcode.com/problems/3sum/): sort, fixing least element and running **2 Sum** with target = -(that element), **remove duplicates** (**2 Sum**)

```python
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
            return []

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

<a id="lc-611"></a>
:heavy_check_mark: :orange_book: [611. Valid Triangle Number](https://leetcode.com/problems/valid-triangle-number/): sort, fix the longest edge and run **2 Sum** on the shorter edges (count pairs with sum > longest edge)

```python
class Solution:
    def triangleNumber(self, nums: List[int]) -> int:

        def get_triangle_count(nums, max_edge_idx):
            count = 0
            # two pointers, each for one short edge
            left, right = 0, max_edge_idx - 1
            target = nums[max_edge_idx]

            while left < right:
                two_sum = nums[left] + nums[right]
                if two_sum > target: # we can make a triangle
                    # if nums[left] sufficient, then nums[left] ~ nums[right - 1] all valid, so a total of (right - 1 - left + 1) selections
                    count += right - left
                    right -= 1 # we go smaller
                else:
                    left += 1 # we need to go larger

            return count

        # for a triangle to work, we need: sum(two short edges) > longest edge
        if not nums or len(nums) < 3:
            return 0

        nums.sort()
        result = 0

        for i in range(2, len(nums)): # fix longest edge and 2 Sum on shorter ones
            result += get_triangle_count(nums, i)

        return result
```

---

<a id="lc-11"></a>
:heavy_check_mark: :orange_book: [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/): start from both ends and always move the shorter bar inwards

```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """

        # Reference Solution:
        # https://leetcode.com/problems/container-with-most-water/discuss/6110/Very-simple-O(n)-solution

        left = 0
        right = len(height) - 1
        maxArea = 0

        while left < right:

            area = (right - left) * min(height[left], height[right])
            maxArea = max(maxArea, area)

            # limited by the lowest bar, so change the lowest bar to maximize it
            if height[left] > height[right]:
                right -= 1
            else:
                left += 1

        return maxArea
```

---

<a id="lc-42"></a>
:heavy_check_mark: :closed_book: [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/): two pointers with running left_max / right_max; the smaller side decides the water level

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """

        # Ref:
        # https://leetcode.com/problems/trapping-rain-water/discuss/17391/Share-my-short-solution./185869

        if not height:
            return 0

        left, right = 0, len(height) - 1
        left_max, right_max = height[left], height[right]
        result = 0

        while left <= right:
            left_max = max(left_max, height[left])
            right_max = max(right_max, height[right])

            if left_max < right_max: # we are limited by the max left height
                result += left_max - height[left]
                left += 1
            else: # we are limited by the max right height
                result += right_max - height[right]
                right -= 1

        return result
```

---

<a id="lc-75"></a>
:heavy_check_mark: :orange_book: [75. Sort Colors](https://leetcode.com/problems/sort-colors/): two partition passes (< 1, then < 2), the quick-sort partition step

```python
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """

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


#         if len(nums) == 1:
#             return nums

#         nums_zero, nums_one, nums_two = 0, 0, 0

#         # count the number of each colors
#         for i in range(len(nums)):
#             if nums[i] == 0:
#                 nums_zero += 1
#             if nums[i] == 1:
#                 nums_one += 1
#             if nums[i] == 2:
#                 nums_two += 1

#         # replace
#         for i in range(len(nums)):
#             if nums_zero != 0:
#                 nums[i] = 0
#                 nums_zero -= 1
#             elif nums_one != 0:
#                 nums[i] = 1
#                 nums_one -= 1
#             else:
#                 nums[i] = 2
#                 nums_two -= 1

#         return nums
```

---

## 2. "Back-to-Back" Two Pointers (Expand from the Middle)

<a id="lc-5"></a>
:heavy_check_mark: :orange_book: [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/): expand around every center (odd and even length) with two pointers moving outwards

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:

        # Method: Two Pointers
        res = ""

        for i in range(len(s)):
            odd  = self.palindromeAt(s, i, i)
            even = self.palindromeAt(s, i, i+1)

            res = max(res, odd, even, key=len)

        return res

    # starting at l,r expand outwards to find the biggest palindrome
    def palindromeAt(self, s, l, r):

        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1

        return s[l + 1:r]
```

---

## 3. "Same-Direction" Two Pointers & Sliding Window

<a id="lc-283"></a>
:heavy_check_mark: :green_book: [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/): one pointing to the first non-replaced element, one pointing to non-zero elements, replace / swap

```python
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

<a id="lc-121"></a>
:heavy_check_mark: :green_book: [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/): left pointer keeps the lowest price so far, right pointer scans for the best profit

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """

        # See Solution
        # https://www.youtube.com/watch?v=1pkOgXD63yU

        if len(prices) < 2:
            return 0

        l, r = 0, 1
        max_profit = 0

        while r < len(prices):

            if prices[l] < prices[r]: # We are making profit (we can leave our left pointer here)
                current_profit = prices[r] - prices[l]
                max_profit = max(current_profit, max_profit)

            else: # We are losing money
                l = r # Always want l to be minimum

            r += 1

        return max_profit
```

---

<a id="lc-76"></a>
:heavy_check_mark: :closed_book: [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/): sliding window with a Counter of needed chars; shrink from the left while the window is valid

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:

        # Ref:
        # https://leetcode.com/problems/minimum-window-substring/discuss/226911/Python-two-pointer-sliding-window-with-explanation

        # we need to make sure all letters in [t] appeared their corresponding number of times
        target_letter_counts = Counter(t)
        target_length = len(t)

        # start and end of the sliding window
        start, end = 0, 0

        min_window = ""

        # we first fix the [start] and move the [end] to the right
        for end in range(len(s)):
            # If we see a target letter, decrease the total target letter count
            # We might have more than needed number for a specific character
            # what we do is we dont count down target_length from then on when we see that element

            if target_letter_counts[s[end]] > 0:
                target_length -= 1 # we used one letter from the target pool/string

            # Decrease the letter count for the current letter
            # If the letter is not a target letter, the count just becomes -ve
            # Yes, we can decrement an element not yet present in the Counter
            target_letter_counts[s[end]] -= 1 # what this does makes all non-target characters negative as well

            # If all letters in the target are found
            # try to contract the window by moving the [start] pointer forward
            while target_length == 0:
                window_length = end - start + 1

                if not min_window or window_length < len(min_window):
                    # Note the new minimum window
                    min_window = s[start : end + 1]

                # Increase the letter count of the current letter
                # we will increase this for all letters. but only the target elements have a chance to have positive counters
                target_letter_counts[s[start]] += 1

                # If all target letters have been seen and now, a target letter is seen with count > 0
                # what does this mean? one of the target characters that we use to validate the target is now out of the window
                if target_letter_counts[s[start]] > 0:
                    target_length += 1

                start += 1 # move the [start] pointer forward to further contract the window

        return min_window
```

---

<a id="lc-763"></a>
:heavy_check_mark: :orange_book: [763. Partition Labels](https://leetcode.com/problems/partition-labels/): record each letter's last index; cut when the current index reaches the farthest last index

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        rightmost = {val:idx for idx, val in enumerate(s)}

        result = []
        left, right = 0, 0

        # 2 pointers
        for idx, letter in enumerate(s):
            right = max(right, rightmost[letter]) # find rightmost possible end for all iterated letter

            if idx == right:
                result += [right - left + 1]
                left = idx + 1 # reset left pointer to start of new section

        return result
```

---

## 4. Other Pointer Tricks

<a id="lc-31"></a>
:heavy_check_mark: :orange_book: [31. Next Permutation](https://leetcode.com/problems/next-permutation/): find the longest non-increasing suffix, swap the pivot with the rightmost larger element, reverse the suffix

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """

        # Ref: https://www.nayuki.io/page/next-lexicographical-permutation-algorithm
        # Follow the steps in Ref

        if len(nums) == 1:
            return nums

        i = j = len(nums) - 1

        while i > 0 and nums[i - 1] >= nums[i]:
            i -= 1

        # now [i] is at the start of the longest non-increasing suffix

        if i == 0: # in this case, the suffix is the entire string, reverse it (no larger possible permutation)
            nums[:] = nums[::-1]
            return

        pivot = i - 1

        while nums[j] <= nums[pivot]:
            j -= 1

        # now nums[j] is the rightmost element that is larger than nums[pivot]
        # swap with pivot
        nums[pivot], nums[j] = nums[j], nums[pivot]

        # reverse the suffix portion
        nums[i:] = nums[len(nums) - 1: i - 1: -1]

        return
```

---
