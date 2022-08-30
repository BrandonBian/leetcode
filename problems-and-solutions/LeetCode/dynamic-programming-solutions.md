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
class Solution:
    def calculateMinimumHP(self, dungeon: List[List[int]]) -> int:
        
        m, n = len(dungeon), len(dungeon[0])
        
        # dp[i][j] = minimum health we need to start from this point and reach bottom-right
        
        # additional row and column at bottom and right
        dp = [[float('inf')] * (n + 1) for _ in range(m + 1)]
        dp[m][n - 1] = 1;
        dp[m - 1][n] = 1;

        for row in range(m - 1, -1, -1):
            for col in range(n - 1, -1, -1):
                # get the minimum health from bottom or right, and minus current cell value
                need = min(dp[row + 1][col], dp[row][col + 1]) - dungeon[row][col]
                # if we do need health, record that; otherwise, 1 health is sufficient
                dp[row][col] = need if need > 0 else 1
                
        return dp[0][0]
```

---

:orange_book: [322. Coin Change](https://leetcode.com/problems/coin-change/)

```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        
        # dp[i] = fewest number of coins needed to make up amount i
        dp = [0] + [float('inf')] * amount
        
        for i in range(1, amount + 1):
            # for each coin choice, we remove it from i and find min
            # we add 1 for using this coin
            try:
                dp[i] = min(dp[i - coin] for coin in coins if coin <= i) + 1
            except:
                continue
        
        return dp[amount] if dp[amount] != float('inf') else -1
```

---


:orange_book: [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)

```
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        
        # dp[i][j] = largest subset of strs such that there are at most [i] 0s and [j] 1s
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        for str in strs:
            zeros = str.count('0')
            ones = str.count('1')
            
            # update cells that have at lest [zeros] 0s and [ones] 1s
            for i in range(m, zeros - 1, -1):
                for j in range(n, ones - 1, -1):
                    # either leave the string be, or take it by adding 1 to the optimal solution of remains
                    dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1)
        
        return dp[m][n]
```

---

:orange_book: [650. 2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard/)

```
class Solution:
    def minSteps(self, n: int) -> int:
        
        # dp[i] = minimum number of operations to print A for [i] times
        # we can always copy and continually paste (1 copy + (i - 1) paste operations)
        dp = [i for i in range(n + 1)]
        dp[0] = 0
        dp[1] = 0
        
        for i in range(2, n + 1):
            for j in range(i - 1, 1, -1):
                if i % j == 0:
                    # getting [j] number of As (i.e., dp[j]), and copy it (+ 1)
                    # paste [(i // j) - 1] times
                    dp[i] = dp[j] + 1 + i // j - 1
                    break
        
        return dp[n]
```

---
