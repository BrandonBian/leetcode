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

:green_book: [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

```
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        
        # dp[i] = min cost to reach index i
        dp = [0] * len(cost)
        
        # base cases
        dp[0] = cost[0]
        dp[1] = cost[1]
        
        for i in range(2, len(cost)):
            # you can come from either 1 step below or 2 steps below, whichever is less
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i]
            
        return min(dp[-1], dp[-2])
```

---

:orange_book: [120. Triangle](https://leetcode.com/problems/triangle/)

```
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        
        # dp[i][j] = minimum path sum from top to triangle[i][j]
        dp = triangle
        
        # base case
        for row in range(1, len(triangle)):
            dp[row][0] += dp[row - 1][0] # left edge
            dp[row][-1] += dp[row - 1][-1] # right edge
        
        for i in range(2, len(triangle)):
            for j in range(1, len(triangle[i]) - 1):
                # either come from top left or from top right
                dp[i][j] += min(dp[i - 1][j], dp[i - 1][j - 1])
        
        return min(dp[len(triangle) - 1])
```

---

:orange_book: [221. Maximal Square](https://leetcode.com/problems/maximal-square/)

```
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        
        m, n = len(matrix), len(matrix[0])
        result = 0
        
        # dp[i][j] = maximum square side length we can achieve when reaching matrix[i][j]
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        for row in range(len(matrix)):
            for col in range(len(matrix[0])):
                if matrix[row][col] == '1':
                    # check top, left, and top-left
                    dp[row + 1][col + 1] = min(dp[row][col], dp[row][col + 1], dp[row + 1][col]) + 1
                    result = max(result, dp[row + 1][col + 1])
                    
        # we want area
        return result * result
```

---

:orange_book: [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/)

```
class Solution:
    def numSquares(self, n: int) -> int:
        
        # dp[i] = least number of perfect square numbers that sum to i
        dp = [0] * (n + 1)

        for i in range(1, n + 1):
            # we iterate through perfect square numbers j ** 2 such that j ** 2 <= i
            # then we remove j ** 2 from i and find that corresponding least number of perfect squares
            # then we add 1 to it indicating our addition of the single perfect square number j ** 2
            dp[i] = min(dp[i - j ** 2] for j in range(1, int(i ** 0.5) + 1)) + 1

        return dp[n]    
```

---

:closed_book: [174. Dungeon Game](https://leetcode.com/problems/dungeon-game/)

```
https://leetcode.com/problems/dungeon-game/
```

---

