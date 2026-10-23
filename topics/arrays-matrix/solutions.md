# Arrays & Matrices · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Matrix Manipulation

<a id="lc-48"></a>
:heavy_check_mark: :orange_book: [48. Rotate Image](https://leetcode.com/problems/rotate-image/): reverse the rows top-to-bottom, then transpose

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """Do not return anything, modify matrix in-place instead."""

        # reverse up and down
        l = 0
        r = len(matrix) -1
        while l < r:
            matrix[l], matrix[r] = matrix[r], matrix[l]
            l += 1
            r -= 1

        # transpose
        for i in range(len(matrix)):
            for j in range(i):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        return matrix
```

---

<a id="lc-54"></a>
:heavy_check_mark: :orange_book: [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/): shrink the top / right / bottom / left boundaries layer by layer

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        height = len(matrix)
        width = len(matrix[0])

        top = 0
        bottom = height - 1
        left = 0
        right = width - 1

        result = []

        while top <= bottom and left <= right:
            for col in range(left, right + 1):
                result.append(matrix[top][col])
            top += 1

            for row in range(top, bottom + 1):
                result.append(matrix[row][right])
            right -= 1

            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1

            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1

        return result[:height*width] # there may be redundant elements for in the central of the matrix
```

---

<a id="lc-73"></a>
:heavy_check_mark: :orange_book: [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/): use the first row and column as O(1)-space markers (track them separately)

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """

        # Ref: https://www.youtube.com/watch?v=T41rL0L3Pnw

        # we need an Space = O(1) solution, since we need to do in-place
        # we use the first row and first col of matrix to record whether a 0 appeared
        m, n = len(matrix), len(matrix[0])

        # still, we would need extra variables to deal with the first row and column
        first_row_has_zero = False
        first_col_has_zero = False

        for row in range(m):
            for col in range(n):
                if matrix[row][col] == 0:
                    if row == 0:
                        first_row_has_zero = True
                    if col == 0:
                        first_col_has_zero = True

                    matrix[0][col] = 0 # record in first row
                    matrix[row][0] = 0 # record in first col

        # iterate through matrix to update the cell to be zero if it's in a zero row or col
        # BUT leaving out the first row and first col, since we will be dealing with them last
        for row in range(1, m):
            for col in range(1, n):
                if matrix[0][col] == 0 or matrix[row][0] == 0:
                    matrix[row][col] = 0

        # update the first row and col if they're zero
        if first_row_has_zero:
            for col in range(n):
                matrix[0][col] = 0

        if first_col_has_zero:
            for row in range(m):
                matrix[row][0] = 0
```

---

<a id="lc-240"></a>
:heavy_check_mark: :orange_book: [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/): start at the top-right corner; move left if too big, down if too small

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:

        m, n = len(matrix), len(matrix[0])

        if m == 1 and n == 1:
            return matrix[0][0] == target

        # we start our search from the top-right corner: matrix[0][n - 1]
        row, col = 0, n - 1

        while row < m and col >= 0:

            # if matrix[row][col] < target, matrix[row][:col] < target, move downwards
            if matrix[row][col] < target:
                row += 1

            # if matrix[row][col] > target, matrix[row:][col] > target, move leftwards
            elif matrix[row][col] > target:
                col -= 1

            else:
                return True

        return False
```

---

## 2. Array Manipulation & Prefix Products

<a id="lc-189"></a>
:heavy_check_mark: :orange_book: [189. Rotate Array](https://leetcode.com/problems/rotate-array/): nums[:] = nums[n - k:] + nums[:n - k] after k %= n

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """

        n = len(nums)
        k = k % n

        if k == 0:
            return

        #           | (k = 3)
        # [1, 2, 3, 4, 5, 6, 7]
        # [5, 6, 7, 1, 2, 3, 4]
        # [5, 6, 7] + [1, 2, 3, 4]

        # [rest of the elements] + [first (n - k) elements]
        nums[:] = nums[n - k:] + nums[:n - k]
```

---

<a id="lc-238"></a>
:heavy_check_mark: :orange_book: [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/): prefix products from the left times suffix products from the right

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        # product of array except nums[i] = product of numbers to the left of nums[i]
        # * product of numbers to the right of nums[i]

        # left_product[i] = product of all elements to the left of index i
        left_product = []
        # right_product[i] = product of all elements to the right of index i
        right_product = []

        # there is no element to the left of index 0
        left_product.append(1)
        # there is no element to the right of index len(nums) - 1
        right_product.append(1)

        for i in range(1, len(nums)):
            left_product.append(left_product[-1] * nums[i - 1])

        for i in range(len(nums) - 2, -1, -1):
            right_product.append(right_product[-1] * nums[i + 1])

        right_product.reverse()

        answer = []
        for i in range(len(nums)):
            answer.append(left_product[i] * right_product[i])

        return answer
```

---
