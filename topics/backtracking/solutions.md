# Recursion & Backtracking · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Base Problems (must do)

<a id="lc-77"></a>
:heavy_check_mark: :orange_book: [77. Combinations](https://leetcode.com/problems/combinations/): (**Backtracking for Combinations**)

```python
class Solution(object):
    def combine(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: List[List[int]]
        """

        # See the combinations template in guide.md

        def dfs(candidates, target, path, res):
            if target < 0:
                return
            if target == 0:
                res.append(path)
                return
            for i in range(len(candidates)):
                dfs(candidates[i+1: ], target - 1, path + [candidates[i]], res)

        res = []
        candidates = range(1, n+1)
        dfs(candidates = candidates, target = k, path = [], res = res)
        return res
```

---

<a id="lc-39"></a>
:heavy_check_mark: :orange_book: [39. Combination Sum](https://leetcode.com/problems/combination-sum/): (**Backtracking for Combination Sum**)

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """

        # Reference Solution: (DFS)
        # https://leetcode.com/problems/combination-sum/discuss/16510/Python-dfs-solution.

        # See template:
        # https://leetcode.com/problems/combination-sum/discuss/429538/General-Backtracking-questions-solutions-in-Python-for-reference-%3A


        #################################################

        # Alternative (using nums[i:] instead of index, which is cleaner):
        def dfs(nums, target, path, ret):

            if target < 0:
                return
            if target == 0:
                ret.append(path)
                return
            for i in range(len(nums)):
                # not nums[i+1:] because we can reuse the same number
                dfs(nums[i:], target-nums[i], path+[nums[i]], ret)

        ret = []
        dfs(candidates, target, [], ret)
        return ret
```

---

<a id="lc-46"></a>
:heavy_check_mark: :orange_book: [46. Permutations](https://leetcode.com/problems/permutations/): (**Backtracking for Permutations**)

```python
class Solution(object):

    def dfs(self, nums, path, res):

        if not nums:
            res.append(path)
            return # backtracking
        for i in range(len(nums)):
            # nums[:i] + nums[i+1:] -> nums but leaving out i
            self.dfs(nums[:i]+nums[i+1:], path+[nums[i]], res)

    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """

        res = []
        self.dfs(nums, [], res)
        return res
```

---

<a id="lc-78"></a>
:heavy_check_mark: :orange_book: [78. Subsets](https://leetcode.com/problems/subsets/): running combinations for lengths from [0, len(nums)] (**Backtracking for Subsets / Power Set**)

```python
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """

        def dfs(candidates, target, path, res):
            if target == 0:
                res.append(path)
                return
            if target < 0:
                return

            for i in range(len(candidates)):
                dfs(candidates[i+1:], target - 1, path + [candidates[i]], res)

        candidates = nums
        res = []

        for i in range(len(candidates) + 1):
            dfs(candidates, i, [], res) # combinations for each length from [0, len(candidates)]

        return res
```

---

## 2. Variations of the Base Problems

<a id="lc-47"></a>
:heavy_check_mark: :orange_book: [47. Permutations II](https://leetcode.com/problems/permutations-ii/): elements in given number array may not be unique, need to **remove duplicates**

```python
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """

        # Note: numbers in nums may not be unique
        # Reference solution:
        # https://leetcode.com/problems/permutations-ii/discuss/18649/Python-easy-to-understand-backtracking-solution.

        def dfs(candidates, path, result):
            if not candidates:
                # This is too slow, since its only checking duplicates at the end of each depth
                # if path not in result:
                #     result.append(path)
                result.append(path)
                return
            for i in range(len(candidates)):
                if i > 0 and candidates[i] == candidates[i-1]: # remove duplicated subtree paths
                    continue
                dfs(candidates[:i]+candidates[i+1:], path+[candidates[i]], result)

        nums.sort()
        result = []
        dfs(nums, [], result)

        return result

```

---

<a id="lc-90"></a>
:heavy_check_mark: :orange_book: [90. Subsets II](https://leetcode.com/problems/subsets-ii/): elements in given number array may not be unique, need to **remove duplicates**

```python
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """

        def dfs(candidates, target, path, result):
            if target == 0:
                result.append(path)
                return
            for i in range(len(candidates)):
                if i > 0 and candidates[i-1] == candidates[i]:
                    continue
                dfs(candidates[i+1:], target-1, path+[candidates[i]], result)

        result = []
        candidates = nums
        candidates.sort()
        for i in range(len(nums)+1):
            dfs(candidates, i, [], result)

        return result
```

---

<a id="lc-40"></a>
:heavy_check_mark: :orange_book: [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/): need to **remove duplicates**, each number can only be used once

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """

        def dfs(candidates, target, path, result):
            if target == 0:
                result.append(path)
                return
            if target < 0:
                return
            for i in range(len(candidates)):
                if i > 0 and candidates[i-1] == candidates[i]:
                    continue
                dfs(candidates[i+1:], target-candidates[i], path+[candidates[i]], result)

        candidates.sort()
        result = []

        dfs(candidates, target, [], result)
        return result
```

---

<a id="lc-216"></a>
:heavy_check_mark: :orange_book: [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/): fixed length, each number can only be used once

```python
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """

        # numbers from 1 to 9

        def dfs(candidates, length, target, path, result):
            if target < 0 or length < 0:
                return

            if target == 0 and length == 0:
                result.append(path)
                return

            for i in range(len(candidates)):
                dfs(candidates[i+1:], length-1, target-candidates[i], path+[candidates[i]], result)

        candidates = [x for x in range(1,10)]
        result = []

        dfs(candidates, k, n, [], result)
        return result
```

---

## 3. Harder Core Problems

<a id="lc-17"></a>
:heavy_check_mark: :orange_book: [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```python
class Solution(object):
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """

        maps = {"2": "abc", "3": "def", "4":"ghi", "5":"jkl", "6":"mno", "7":"pqrs", "8":"tuv", "9":"wxyz"}

        def dfs(candidates, target, path, res):
            if target >= len(digits): # we have gone through all digits
                res.append(path)
                return

            string1 = maps[candidates[target]] # all letters corresponding to this digit

            for i in string1:
                dfs(candidates, target + 1, path + i, res)


        if len(digits) == 0:
            return []

        res = []

        dfs(digits, 0, "", res)
        return res
```

---

<a id="lc-22"></a>
:heavy_check_mark: :orange_book: [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """

        # Backtracking solution

        def backtracking(nOpen, nClose, path):
            if n == nClose:  # Found a valid n pairs of parentheses
                ans.append(path)
                return

            if nOpen < n:  # Number of opening bracket up to `n`
                backtracking(nOpen + 1, nClose, path + "(")
            if nClose < nOpen:  # Number of closing bracket up to opening bracket
                backtracking(nOpen, nClose + 1, path + ")")

        ans = []
        backtracking(0, 0, "")
        return ans
```

---

<a id="lc-79"></a>
:heavy_check_mark: :orange_book: [79. Word Search](https://leetcode.com/problems/word-search/)

```python
class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """

        # Reference Solution:
        # https://leetcode.com/problems/word-search/discuss/747144/Python-dfs-backtracking-solution-explained

        # ind = index of the word we are at; i,j = position on board we are at

        def dfs(ind, i, j):
            if self.Found: return        # early stop if word is found

            if ind == k:
                self.Found = True                # for early stopping
                return

            if i < 0 or i >= m or j < 0 or j >= n: return  # we went outside of the board

            # check if current symbol on board is what we want in word
            tmp = board[i][j]
            if tmp != word[ind]: return

            # indicate this position is visited

            board[i][j] = "#"

            # dfs on all neighbors
            for x, y in [[0,-1], [0,1], [1,0], [-1,0]]:
                dfs(ind + 1, i+x, j+y)

            # change the position back from "#"
            board[i][j] = tmp

        self.Found = False

        m, n, k = len(board), len(board[0]), len(word)

        for row in range(m):
            for col in range(n):
                if self.Found: return True # early stop
                dfs(0, row, col)

        return self.Found
```

---

<a id="lc-131"></a>
:heavy_check_mark: :orange_book: [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/): a similar method as the DFS template above

```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """

        # Reference solution
        #

        def isPal(string):
            return string == string[::-1] # string == its inverse

        def dfs(candidates, path, result):
            if len(candidates) == 0:
                result.append(path[:])
                return
            for i in range(1, len(candidates)+1):
                if isPal(candidates[:i]): # check first part before current index is palindrome
                    dfs(candidates[i:], path+[candidates[:i]], result) # then we move on to check the rest part

        result = []
        dfs(s, [], result)

        return result
```

---

<a id="lc-51"></a>
:heavy_check_mark: :closed_book: [51. N-Queens](https://leetcode.com/problems/n-queens/)

```python
class Solution(object):
    def solveNQueens(self, n):
        """
        :type n: int
        :rtype: List[List[str]]
        """

        if n == 1:
            return [["Q"]]

        # Reference solution
        # https://leetcode.com/problems/n-queens/discuss/19810/Fast-short-and-easy-to-understand-python-solution-11-lines-76ms

        # whenever a location (x, y) is occupied,
        # any other locations (p, q ) where p + q == x + y or p - q == x - y would be invalid

        def DFS(queens, xy_dif, xy_sum):
            p = len(queens) # p is the index of row
            if p==n: # we have successfully placed all n queens (each queen in different index of row)
                result.append(queens)
                return None
            for q in range(n): # q is the index of col
                # queens stores those used cols, for example, [0,2,4,1] means these cols have been used
                # xy_dif is the diagonal top-left -> bottom-right
                # xy_sum is the diagonal top-right -> bottom-left
                if q not in queens and p-q not in xy_dif and p+q not in xy_sum:
                    DFS(queens+[q], xy_dif+[p-q], xy_sum+[p+q])
        result = []
        DFS([],[],[])
        return [ ["."*i + "Q" + "."*(n-i-1) for i in sol] for sol in result]
```

---

<a id="lc-37"></a>
:heavy_check_mark: :closed_book: [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

```python
class Solution(object):
    def solveSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """

        # Reference solution
        # https://leetcode.com/problems/sudoku-solver/discuss/294215/Simple-concise-clear-Python-solution

        self.backtrack(board, 0, 0) # start from top left position

    def backtrack(self, board, r, c):

        # Stopping condition: when board is filled
        while board[r][c] != '.':
            c += 1
            if c == 9: c, r = 0, r+1
            if r == 9: return True # all board filled

        # Now the spot at board[r][c] is empty
        # Try all options, backtracking if not work
        for k in range(1, 10):
            if self.isValidSudokuMove(board, r, c, str(k)):
                board[r][c] = str(k) # if valid option, insert it
                if self.backtrack(board, r, c): # test if the puzzle is solved (i.e., all slots filled)
                    return True

        # If all options failed, backtrack and return slot to empty
        board[r][c] = '.'
        return False

    def isValidSudokuMove(self, board, r, c, cand):
        # Check row
        if any(board[r][j] == cand for j in range(9)): return False
        # Check col
        if any(board[i][c] == cand for i in range(9)): return False
        # Check block
        br, bc = 3*(r//3), 3*(c//3)
        if any(board[i][j] == cand for i in range(br, br+3) for j in range(bc, bc+3)): return False

        return True
```

---
