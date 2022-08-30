# LeetCode - Dynamic Programming | Solutions
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---
:orange_book: [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

```
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        
        # dp[i][j] = minimum path from top left to position[i][j]
        dp = grid
        m, n = len(grid), len(grid[0])
        
        for i in range(1, m):
            # going down the left column
            dp[i][0] += dp[i - 1][0]
        
        for i in range(1, n):
            # going right the top row
            dp[0][i] += dp[0][i - 1]
        
        for row in range(1, m):
            for col in range(1, n):
                # all possible paths before this position: coming from top or left
                # then add the value for the current position
                dp[row][col] += min(dp[row - 1][col], dp[row][col - 1])
        
        return dp[m - 1][n - 1]
```

---
