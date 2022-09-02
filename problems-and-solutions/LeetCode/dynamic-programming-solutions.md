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

:closed_book: [871. Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

```
class Solution(object):
    def minRefuelStops(self, target, startFuel, stations):
        """
        :type target: int
        :type startFuel: int
        :type stations: List[List[int]]
        :rtype: int
        """
        
        # dp[i] = furthest distance we can get after [i] number of refueling stops
        
        dp = [startFuel] + [0] * len(stations)
        
        for i in range(len(stations)): 
            # for every station, find out maximum distance we can reaching using this as a refuel point
            for t in range(i, -1, -1):
                # if current distance >= position of station (i.e., we can reach this station)
                if dp[t] >= stations[i][0]:
                    # then we can refuel
                    dp[t + 1] = max(dp[t + 1], dp[t] + stations[i][1])
        
        for t, d in enumerate(dp):
            if d >= target: return t
        
        return -1
```

---

:orange_book: [931. Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/)

```
class Solution(object):
    def minFallingPathSum(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: int
        """
        
        # dp[i][j] = minimum sum of falling path up to matrix[i][j]
        dp = matrix
        
        for row in range(1, len(matrix)):
            for col in range(len(matrix[0])):
                
                if col == 0:
                    # first column: coming from top or top right
                    dp[row][col] += min(dp[row - 1][col], dp[row - 1][col + 1])
                    
                elif col == len(matrix[0]) - 1:
                    # last column: coming from top or top left
                    dp[row][col] += min(dp[row - 1][col], dp[row - 1][col - 1])
                
                else:
                    # coming from top left, top, or top right
                    dp[row][col] += min(dp[row - 1][col - 1], dp[row - 1][col], dp[row - 1][col + 1])
                    
        return min(dp[-1])
```

---

:orange_book: [983. Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/)

```
class Solution(object):
    def mincostTickets(self, days, costs):
        """
        :type days: List[int]
        :type costs: List[int]
        :rtype: int
        """
        
        # dp[i] = minimum cost to fulfill travel plan up to day i
        dp = [0 for _ in range(days[-1] + 1)] # for all days from 0 ... last day on plan
        
        travel_days = set(days)
        
        for i in range(days[-1] + 1):
            if i not in travel_days: # if we are not traveling on this day, same cost as day before
                dp[i] = dp[i - 1]
            else: # if we are traveling on this day
                
                # a minimum of yesterday's cost plus single-day ticket, 
                # or cost for 8 days ago plus 7-day pass, 
                # or cost 31 days ago plus 30-day pass
                dp[i] = min(dp[max(0, i - 1)] + costs[0],
                            dp[max(0, i - 7)] + costs[1],
                            dp[max(0, i - 30)] + costs[2])
                
        return dp[-1]
```

---

:orange_book: [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

```
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        # partition the stones into two subsets, which have least difference in summation
        # ref 1: https://leetcode.com/problems/last-stone-weight-ii/discuss/402213/Python-solution-based-on-0-1-Knapsack
        # ref 2: https://leetcode.com/problems/last-stone-weight-ii/discuss/859641/Python3-DP-with-extensive-though-process-explanation-and-clean-code
        
        total_weight = sum(stones)
        max_weight = total_weight // 2
        
        dp = [0] * (max_weight + 1)
        
        for stone in stones:
            for weight in range(max_weight, -1, -1):
                if weight >= stone:
                    dp[weight] = max(dp[weight], dp[weight - stone] + stone)
                    
        return total_weight - 2 * dp[-1]
```

---

:orange_book: [62. Unique Paths](https://leetcode.com/problems/unique-paths/)

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        
        # dp[i][j] = number of unique paths from top-left to [i][j]
        dp = [[0] * n for _ in range(m)]
        
        # base case
        for i in range(m):
            # left column
            dp[i][0] = 1
        for i in range(n):
            # top row
            dp[0][i] = 1
        
        for row in range(1, m):
            for col in range(1, n):
                # you can come from either top or left
                dp[row][col] = dp[row - 1][col] + dp[row][col - 1]
        
        return dp[m - 1][n - 1]
```

---

:orange_book: [91. Decode Ways](https://leetcode.com/problems/decode-ways/)

```
class Solution:
    def numDecodings(self, s: str) -> int:
        
        # dp[i] = number of ways to decode the first i characters in s
        dp = [0] * (len(s) + 1)
        
        # base case
        dp[0] = 1
        dp[1] = 0 if s[0] == '0' else 1
        
        for i in range(2, len(s) + 1):
            if 0 < int(s[i - 1:i]) <= 9:
                # one step jump
                dp[i] += dp[i - 1]
            if 10 <= int(s[i - 2:i]) <= 26:
                # two steps jump
                dp[i] += dp[i - 2]

        return dp[len(s)]       
```

---

:green_book: [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

```
class Solution:
    def fib(self, n: int) -> int:
        
        # dp[i] = F(i) = the i-th fibonacci number
        dp = [0, 1, 1]
        
        for i in range(3, n + 1):
            dp.append(dp[i - 1] + dp[i - 2])
        
        return dp[n]
```

---

:orange_book: [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

```
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        
        # dp[i][j] = number of unique paths from top-left to reach[i][j]
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        dp = [[0] * n for _ in range(m)]
        
        # base cases
        blocked = False
        for i in range(m):
            # left column
            if obstacleGrid[i][0] == 0:
                if blocked:
                    dp[i][0] = 0
                else:
                    dp[i][0] = 1
            else:
                blocked = True
                dp[i][0] = 0

        blocked = False
        for i in range(n):
            # top row
            if obstacleGrid[0][i] == 0:
                if blocked:
                    dp[0][i] = 0
                else:
                    dp[0][i] = 1
            else:
                blocked = True
                dp[0][i] = 0
        
        for row in range(1, m):
            for col in range(1, n):
                if obstacleGrid[row][col] == 0:
                    dp[row][col] = dp[row - 1][col] + dp[row][col - 1]
        
        return dp[m - 1][n - 1]
```

---


:green_book: [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

```
class Solution:
    def climbStairs(self, n: int) -> int:
        
        # dp[i] = number of distinct ways to reach step i
        dp = [0, 1, 2]
        
        for i in range(3, n + 1):
            dp.append(dp[i - 2] + dp[i - 1])
        
        return dp[n]
```

---

:orange_book: [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

```
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        
        nums.sort()
        
        # dp[i] = number of possible combinations that add up to i
        dp = [0] * (target + 1)
        dp[0] = 1
        
        for i in range(1, target + 1):
            for num in nums:
                if num > i:
                    break # we cannot add any valid combination
                elif num == i:
                    dp[i] += 1 # we can add one more valid combination (i.e., num itself)
                else:
                    dp[i] += dp[i - num]
        
        return dp[target]
```

---

:orange_book: [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        
        # Ref: https://youtu.be/IsvocB5BJhw
        
        if sum(nums) % 2 == 1:
            # sum must be even
            return False
        
        dp = set() # total values we can get
        dp.add(0) # guaranteed to have a sum of 0 (i.e., picking no element)
        target = sum(nums) // 2
        
        for i in range(len(nums)):
            nextDP = set() # note that you cannot modify dp during the following for loop
            for t in dp:
                if (t + nums[i]) == target: # we have already found a valid partition
                    return True
                nextDP.add(t + nums[i]) # adding new value to nextDP
                nextDP.add(t) # add original value in dp
            dp = nextDP
            
        return True if target in dp else False
```

---

:orange_book: [494. Target Sum](https://leetcode.com/problems/target-sum/)

```
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        
        # curstops = <key, val> = <target sum, number of ways to reach this sum>
        curstops = defaultdict(int)
        curstops[0] = 1
        
        for distance in nums:
            # parepare a new map for the next iteration
            nextstops = defaultdict(int)
            
            # for each iteration, update each possible sum and it's count
            for stop in curstops:

                # current to be positive (if we choose to + distance)
                nextstops[stop + distance] += curstops[stop] # number of ways to reach this step/position
                # current to be negative (if we choose to - distance)
                nextstops[stop - distance] += curstops[stop] # number of ways to reach this step/position
                
            # assign the new map to be the current map
            curstops = nextstops

        return curstops[target]
```

---

:orange_book: [1155. Number of Dice Rolls With Target Sum](https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/)

```
class Solution:
    def numRollsToTarget(self, n: int, k: int, target: int) -> int:
        
        # Ref: https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/discuss/770166/Evolve-from-brute-force-to-dp
        
        # dp[i][j] = number of possible ways to reach target [j] with up to [i] dices
        dp = [[0] * (target + 1) for _ in range(n + 1)]
        
        dp[0][0] = 1
        
        for dice in range(1, n + 1):
            for tar in range(1, target + 1):
                for face in range(1, k + 1):
                    if face <= tar:
                        dp[dice][tar] += dp[dice - 1][tar - face]
                        
        return dp[n][target] % (int(1e9) + 7)
```

---

:orange_book: [576. Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths/)

```
class Solution:
    def findPaths(self, m: int, n: int, maxMove: int, startRow: int, startColumn: int) -> int:
        
        # Dynamic programming using tabulation
        # Ref: https://leetcode.com/problems/out-of-boundary-paths/discuss/1293835/Short-and-Easy-Solution-w-Explanation-or-Optimization-from-Brute-Force
        
        # dp[i][j][M] = number of ways to move outside of grid from [i][j] using [M] moves maximum
        dp = [[[0] * (maxMove + 1) for _ in range(n + 1)] for _ in range(m + 1)]
        
        def out_of_bound(i, j):
            return not (0 <= i < m and 0 <= j < n)
        
        # iterate for all available moves
        for move in range(1, maxMove + 1):
            for i in range(m):
                for j in range(n):
                    # for each cell, try all 4 possible moves
                    for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                        if out_of_bound(i + dx, j + dy): 
                            dp[i][j][move] += 1
                        else:
                            # Note: reversed version is faster
                            # dp[i][j][move] += dp[i + dx][j + dy][move - 1]
                            dp[i + dx][j + dy][move] += dp[i][j][move - 1]
                            
        return dp[startRow][startColumn][maxMove] % int(1e9 + 7)
```

---

:orange_book: [673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

```
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        
        # length[i] = longest increasing subsequence length ending at nums[i]
        # count[i] = number of paths with longest increasing subsequence length length[i]
        
        length = [1] * len(nums)
        count = [1] * len(nums)
        
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    # length[i] = max(length[j] + 1, length[i]) 
                    # but we need to compute count also
                    if length[i] == length[j]:
                        length[i] = length[j] + 1
                        count[i] = count[j]
                    elif length[i] == length[j] + 1:
                        count[i] += count[j]
                        
        max_length = max(length)
        return sum([count[i] for i in range(len(nums)) if length[i] == max_length])
```

---

:orange_book: [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

```
class Solution:
    def knightProbability(self, n: int, k: int, row: int, column: int) -> float:
        
        # dp[k][i][j] = probability that the knight is at [i][j] after k steps
        dp = [[[0] * n for _ in range(n)] for _ in range(k + 1)]
        dp[0][row][column] = 1
        
        dirs = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]
        
        def out_of_range(i, j):
            return not(0 <= i < n and 0 <= j < n)
        
        for k in range(1, k + 1):
            for i in range(n):
                for j in range(n):
                    for dx, dy in dirs:
                        x, y = i + dx, j + dy
                        if out_of_range(x, y): continue
                        # Note: reversed version is valid too
                        # dp[k][i][j] += dp[k - 1][x][y] * 0.125
                        dp[k][x][y] += dp[k - 1][i][j] * 0.125
        
        # return the summation of pobability at the last step of all cells
        return sum(i for row in dp[-1] for i in row)
```

---

:orange_book: [790. Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/)

```
class Solution:
    def numTilings(self, n: int) -> int:
        
        # Ref: https://leetcode.com/problems/domino-and-tromino-tiling/discuss/1620975/C%2B%2BPython-Simple-Solution-w-Images-and-Explanation-or-Optimization-from-Brute-Force-to-DP
        
        # dp[i][0]: number of ways to tile the grid till ith column (including ith column) and keeping no gap
        # dp[i][1]: number of ways to completely tile the grid till ith column keeping a gap in i+1th column (a square protruding out)
        
        dp = [[0, 0] for _ in range(n + 2)]
        
        # dp[1][0] = 1, using T1
        # dp[1][1] = 1, using T3 or T4
        # dp[2][0] = 2, adding T1 to dp[1][0] or using T2 in pairs
        # dp[2][1] = 2, adding T2 to dp[1][1] or adding T3/T4 to dp[1][0]    
        dp[1], dp[2] = [1, 1], [2, 2]
        
        for i in range(3, n + 1):
            dp[i][0] = dp[i - 1][0] + dp[i - 2][0] + 2 * dp[i - 2][1]
            dp[i][1] = dp[i - 1][0] + dp[i - 1][1]
        
        return dp[n][0] % int(1e9 + 7)
```

---
