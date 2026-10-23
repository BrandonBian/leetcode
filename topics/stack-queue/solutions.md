# Stacks & Queues · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Stack

<a id="lc-20"></a>
:heavy_check_mark: :green_book: [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/): push opening brackets; each closing bracket must match the top of the stack

```python
class Solution:
    def isValid(self, s: str) -> bool:

        brackets = {'(':')', '{':'}', '[':']'}
        stack = []

        for idx, value in enumerate(s):
            if value == '(' or value == '[' or value == '{':
                stack.append(value)
            else:
                if len(stack) == 0: # If nothing in stack, definitely no match for this value
                    return False

                val = stack.pop() # Obtain the top-most element in stack

                if brackets[val] != value:
                    return False

        return len(stack) == 0
```

---

<a id="lc-155"></a>
:heavy_check_mark: :green_book: [155. Min Stack](https://leetcode.com/problems/min-stack/): a second stack keeps the running minimum

```python
class MinStack:

    def __init__(self):
        # Idea: using a list to mimic a stack
        self.stack = []
        self.min = [float('inf')]


    def push(self, val: int) -> None:
        self.stack.append(val)

        if val <= self.min[-1]:
            self.min.append(val)


    def pop(self) -> None:

        if self.stack[-1] == self.min[-1]:
            self.min.pop()

        self.stack.pop()


    def top(self) -> int:
        return self.stack[-1]


    def getMin(self) -> int:
        return self.min[-1]
```

---

<a id="lc-394"></a>
:heavy_check_mark: :orange_book: [394. Decode String](https://leetcode.com/problems/decode-string/): stack of (previous string, repeat count); build the current string until `]`

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []
        k = 0
        current_string = ""

        for char in s:
            if char == "[":
                # finished parsing the k, save current string and k into stack
                stack.append((current_string, k))
                current_string = ""
                k = 0
            elif char == "]":
                # multiply the current string and append to previous string
                last_string, last_k = stack.pop()
                current_string = last_string + last_k * current_string
            elif char.isdigit():
                # keep reading and finding the k that it represents
                k = k * 10 + int(char)
            else:
                # this is a regular char, just append to current_string
                current_string += char

        return current_string
```

---

<a id="lc-32"></a>
:heavy_check_mark: :closed_book: [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/): stack of indices seeded with -1 as the base of the current valid run

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:

        if len(s) <= 1:
            return 0

        # stack will hold [indices] instead of the [parentheses]
        # Ref: https://www.youtube.com/watch?v=VdQuwtEd10M
        stack = [-1] # -1 here to account for the edge case in which given string is already valid
        result = 0

        for i in range(len(s)):
            char = s[i]

            if char == '(':
                stack.append(i)
            else:
                stack.pop()
                if not stack:
                    stack.append(i)
                else:
                    length = i - stack[-1]
                    result = max(result, length)

        return result
```

---

## 2. Monotonic Stack / Queue

<a id="lc-739"></a>
:heavy_check_mark: :orange_book: [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/): monotonic decreasing stack of indices; pop when a warmer day arrives

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        # Ref: https://leetcode.com/problems/daily-temperatures/discuss/1574808/C%2B%2BPython-3-Simple-Solutions-w-Explanation-Examples-and-Images-or-2-Monotonic-Stack-Approaches

        answer = [0] * len(temperatures)
        stack = [] # storing indices of temperatures

        for idx, temp in enumerate(temperatures):
            while len(stack) > 0 and temp > temperatures[stack[-1]]:
                answer[stack[-1]] = idx - stack[-1]
                stack.pop()

            stack.append(idx)

        return answer
```

---

<a id="lc-84"></a>
:heavy_check_mark: :closed_book: [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/): for each bar, count consecutive bars >= it on both sides (jumping by earlier counts)

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:

        # Plain iterative solution (less efficient but easier to think of)
        # Ref: https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28963/Python-solution-without-using-stack.-(with-explanation)

        n = len(heights)
        if n == 1:
            return heights[0]

        # left[i]: how many CONSECUTIVE bars to the left (including the bar at [i]) that is >= heights[i]
        # right[i]: how many CONSECUTIVE bars to the right (including the bar at [i]) that is >= heights[i]
        # then we have max rectangle containing bar[i] = height[i] * (left[i] + right[i] - 1)

        left = [1] * n
        right = [1] * n

        # calculate left
        for i in range(0, n): # [j] to the left of [i]
            j = i - 1
            while j >= 0 and heights[j] >= heights[i]:
                j -= left[j]
            left[i] = i - j

        # calculate right
        for i in range(n-2, -1, -1): # [i] to the left of [j]
            j = i + 1
            while j < len(heights) and heights[i] <= heights[j]:
                j += right[j]
            right[i] = j - i

        res = 0

        for i in range(n):
            res = max(res, heights[i] * (left[i] + right[i] - 1))

        return res
```

---

<a id="lc-85"></a>
:heavy_check_mark: :closed_book: [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/): build histogram heights row by row, then apply 84 to each row

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        # Ref: https://www.youtube.com/watch?v=g8bSdXCG-lA

        if not matrix or not matrix[0]:
            return 0
        if len(matrix) == len(matrix[0]) == 1: # only one element
            return 0 if matrix[0][0] == '0' else 1

        m, n = len(matrix), len(matrix[0])
        result = 0

        height = [0] * n

        for row in matrix:
            for col in range(n):
                height[col] = height[col] + 1 if row[col] == '1' else 0

            result = max(result, self.largestRectangleInHistogram(height))

        return result

    def largestRectangleInHistogram(self, heights):
        # from LeetCode 84

        n = len(heights)
        if n == 1:
            return heights[0]

        left = [1] * n
        right = [1] * n

        # calculate left
        for i in range(0, n): # [j] to the left of [i]
            j = i - 1
            while j >= 0 and heights[j] >= heights[i]:
                j -= left[j]
            left[i] = i - j

        # calculate right
        for i in range(n-2, -1, -1): # [i] to the left of [j]
            j = i + 1
            while j < len(heights) and heights[i] <= heights[j]:
                j += right[j]
            right[i] = j - i

        res = 0

        for i in range(n):
            res = max(res, heights[i] * (left[i] + right[i] - 1))

        return res
```

---

<a id="lc-239"></a>
:heavy_check_mark: :closed_book: [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/): monotonic decreasing deque of indices; the front is the window maximum

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

        # YouTube: https://www.youtube.com/watch?v=DfljaUwZsOk

        # monotonically decreasing queue: contains indices for nums
        from collections import deque
        queue = deque()

        result = []

        for idx, num in enumerate(nums):

            # while the queue is not empty
            # and that the right-most element in the queue < value that we are inserting
            # remove the smaller elements from the queue -> since our goal is to keep a monotonically DECREASING queue
            while queue and nums[queue[-1]] < num:
                queue.pop() # pop from the right

            queue.append(idx)

            # remove first element if it's outside the window
            if queue[0] == idx - k:
                queue.popleft()

            # if window has k elements, we add to result
            # first k-1 windows have < k elements because we start from empty window and add 1 element each iteration
            if idx >= k - 1:
                result.append(nums[queue[0]])

        return result
```

---
