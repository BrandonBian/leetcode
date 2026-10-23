# Dynamic Programming · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Minimum (Maximum) Path / Cost to Reach a Target

<a id="lc-64"></a>
:orange_book: [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/): dp[i,j] = minimum path sum from top left to position[i][j]

```python
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

<a id="lc-746"></a>
:green_book: [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/): dp[i] = min cost to reach step i

```python
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

<a id="lc-120"></a>
:orange_book: [120. Triangle](https://leetcode.com/problems/triangle/): dp[i][j] = minimum path sum from top to triangle[i][j]

```python
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

<a id="lc-221"></a>
:orange_book: [221. Maximal Square](https://leetcode.com/problems/maximal-square/): dp[i][j] = maximum square side length we can achieve when reaching matrix[i][j]

```python
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

<a id="lc-279"></a>
:orange_book: [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/): dp[i] = least number of perfect square numbers that sum to i

```python
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

<a id="lc-174"></a>
:closed_book: [174. Dungeon Game](https://leetcode.com/problems/dungeon-game/): dp[i][j] = minimum health we need to start from this point and reach bottom-right

```python
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

<a id="lc-322"></a>
:orange_book: [322. Coin Change](https://leetcode.com/problems/coin-change/): dp[i] = fewest number of coins needed to make up amount i

```python
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

- **Alternative: plain double loop over amounts and coins**

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """

        # YouTube: https://www.youtube.com/watch?v=Y0ZqKpToTic

        # dp[i] = fewest number of coins you need to make up amount i
        dp = [0] + [float('inf') for i in range(amount)]

        for i in range(1, amount + 1):
            for coin in coins:
                # considering how many coins
                if i >= coin:
                    dp[i] = min(dp[i], dp[i - coin] + 1)

        if dp[-1] == float('inf'):
            return -1
        else:
            return dp[-1]
```

---

<a id="lc-474"></a>
:orange_book: [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/): dp[i][j] = largest subset of strs such that there are at most [i] 0s and [j] 1s

```python
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

<a id="lc-650"></a>
:orange_book: [650. 2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard/): dp[i] = minimum number of operations to print A for [i] times

```python
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

<a id="lc-871"></a>
:closed_book: [871. Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/): dp[i] = furthest distance we can get after [i] number of refueling stops

```python
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

<a id="lc-931"></a>
:orange_book: [931. Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/): dp[i][j] = minimum sum of falling path up to matrix[i][j]

```python
class Solution(object):
    def minFallingPathSum(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: int
        """

        # dp[i][j] = minimum sum of falling path up to matrix[i][j]
        dp = matrix

        n = len(matrix[0])

        for row in range(1, len(matrix)):
            for col in range(n):

                # coming from top, top left (if not first column), or top right (if not last column)
                best = dp[row - 1][col]
                if col > 0:
                    best = min(best, dp[row - 1][col - 1])
                if col < n - 1:
                    best = min(best, dp[row - 1][col + 1])

                dp[row][col] += best

        return min(dp[-1])
```

---

<a id="lc-983"></a>
:orange_book: [983. Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/): dp[i] = minimum cost to fulfill travel plan up to day i

```python
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

<a id="lc-1049"></a>
:orange_book: [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

```python
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

## 2. Number of Distinct Ways to Do Something

<a id="lc-62"></a>
:orange_book: [62. Unique Paths](https://leetcode.com/problems/unique-paths/): dp[i][j] = number of unique paths from top-left to [i][j]

```python
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

<a id="lc-91"></a>
:orange_book: [91. Decode Ways](https://leetcode.com/problems/decode-ways/): dp[i] = number of ways to decode the first i characters in s

```python
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

<a id="lc-509"></a>
:green_book: [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/): dp[i] = F(i) = the i-th fibonacci number

```python
class Solution:
    def fib(self, n: int) -> int:

        # dp[i] = F(i) = the i-th fibonacci number
        dp = [0, 1, 1]

        for i in range(3, n + 1):
            dp.append(dp[i - 1] + dp[i - 2])

        return dp[n]
```

---

<a id="lc-63"></a>
:orange_book: [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/): dp[i][j] = number of unique paths from top-left to reach[i][j]

```python
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

<a id="lc-70"></a>
:green_book: [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/): dp[i] = number of distinct ways to reach step i

```python
class Solution:
    def climbStairs(self, n: int) -> int:

        # dp[i] = number of distinct ways to reach step i
        dp = [0, 1, 2]

        for i in range(3, n + 1):
            dp.append(dp[i - 2] + dp[i - 1])

        return dp[n]
```

---

<a id="lc-96"></a>
:heavy_check_mark: :orange_book: [96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/): dp[n] = sum(dp[i - 1] * dp[n - i]) over every root i (Catalan numbers), memoized

```python
class Solution(object):

    # dp[i] = number of unique BSTs for n nodes = numTrees(i)
    dp = [0] * 20

    def numTrees(self, n):
        """
        :type n: int
        :rtype: int
        """

        # Reference Solution:
        # https://leetcode.com/problems/unique-binary-search-trees/discuss/1565543/C%2B%2BPython-5-Easy-Solutions-w-Explanation-or-Optimization-from-Brute-Force-to-DP-to-Catalan-O(N)


        if n <= 1:
            return 1

        if self.dp[n] != 0: # already calculated
            return self.dp[n]

        for i in range(1, n + 1):
            self.dp[n] += self.numTrees(i - 1) * self.numTrees(n - i)

        return self.dp[n]
```

---

<a id="lc-377"></a>
:orange_book: [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/): dp[i] = number of possible combinations that add up to i

```python
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

<a id="lc-416"></a>
:orange_book: [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/): dp = set() # total values we can get

```python
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

<a id="lc-494"></a>
:orange_book: [494. Target Sum](https://leetcode.com/problems/target-sum/): curstops = <key, val> = <target sum, number of ways to reach this sum>

```python
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

<a id="lc-1155"></a>
:orange_book: [1155. Number of Dice Rolls With Target Sum](https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/): dp[i][j] = number of possible ways to reach target [j] with up to [i] dices

```python
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

<a id="lc-576"></a>
:orange_book: [576. Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths/): dp[i][j][M] = number of ways to reach [i][j] from outside of grid using [M] moves maximum

```python
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

<a id="lc-673"></a>
:orange_book: [673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/): two arrays for DP

```python
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

<a id="lc-688"></a>
:orange_book: [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/): dp[k][i][j] = probability that the knight is at [i][j] after k steps

```python
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

<a id="lc-790"></a>
:orange_book: [790. Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/)

```python
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

<a id="lc-935"></a>
:orange_book: [935. Knight Dialer](https://leetcode.com/problems/knight-dialer/): dp[i][j] = number of distinct numbers after [i] jumps, starting from digit [j]

```python
class Solution:
    def knightDialer(self, n: int) -> int:
        transitions = {
            0: [4, 6],
            1: [6, 8],
            2: [7, 9],
            3: [4, 8],
            4: [0, 3, 9],
            5: [],
            6: [0, 1, 7],
            7: [2, 6],
            8: [1, 3],
            9: [2, 4]
        }

        # dp[i][j] = number of distinct numbers after [i] jumps, starting from digit [j]
        dp = [[0] * 10 for _ in range(n)]

        # base case: jump = 0, only one
        for i in range(10):
            dp[0][i] = 1

        for i in range(1, n):
            for d in range(10):
                for from_pos in transitions[d]:
                    # all the digits that can reach our current digit, from previous jump
                    dp[i][d] += dp[i - 1][from_pos]

        result = 0

        for i in dp[-1]:
            # we can start from any numeric cell
            result += i

        return result % int(1e9 + 7)
```

---

<a id="lc-801"></a>
:closed_book: [801. Minimum Swaps To Make Sequences Increasing](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/)

```python
class Solution:
    def minSwap(self, nums1: List[int], nums2: List[int]) -> int:

        # Ref: https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/discuss/640752/Python-DP-solution-with-explanations

        # YouTube: https://youtu.be/__yxFFRQAl8

        # we need 2 arrays to record the two possible actions taken at index i
        # dp[0][i], min number of swaps up to i, if at i, we do nothing
        # dp[1][i], min number of swaps up to i, if at i, we swap A[i] and B[i]
        dp = [[float('inf')] * len(nums1) for _ in range(2)]

        # base case
        dp[0][0] = 0 # we do nothing
        dp[1][0] = 1 # we swap once

        for i in range(1, len(nums1)):
            if nums1[i] > nums1[i - 1] and nums2[i] > nums2[i - 1]: # both arrays are increasing
                # If at i, we choose to keep, i-1 will also have to keep to make it valid
                dp[0][i] = dp[0][i - 1]
                # If at i, we choose to swap, i-1 will also have to swap to make it valid
                # (we swap both i-1 and i to MAKE SURE this is 100% valid -> for next if statement)
                # and since we swap at i, we need to add one
                dp[1][i] = dp[1][i - 1] + 1

            if nums1[i] > nums2[i - 1] and nums2[i] > nums1[i - 1]: # cross comparing
                # If at i, we choose to keep
                dp[0][i] = min(dp[0][i], dp[1][i - 1])
                # If at i, we choose to swap
                dp[1][i] = min(dp[1][i], dp[0][i - 1] + 1)

        return min(dp[0][len(nums1) - 1], dp[1][len(nums1) - 1])
```

---

## 3. DP on Strings & Subsequences

<a id="lc-139"></a>
:heavy_check_mark: :orange_book: [139. Word Break](https://leetcode.com/problems/word-break/): dp[i] = True if some word ends at i and dp[i - len(word)] is True

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """

        # Sample Solution:
        # https://leetcode.com/problems/word-break/discuss/43808/Simple-DP-solution-in-Python-with-description

        # d[i] is True if there is a word in the dictionary that ends at ith index of s AND d is also True at the beginning of the word

        # e.g. Example 1 in given question
        # d[3] is True because there is "leet" in the dictionary that ends at 3rd index of "leetcode"
        # d[7] is True because there is "code" in the dictionary that ends at the 7th index of "leetcode" AND d[3] is True

        d = [False] * (len(s) + 1)
        d[0] = True

        for i in range(len(s) + 1):
            for word in wordDict:
                # if this word ends at index [i] and there is already a word ending at [i - len(word)]
                # we know that s[:i] can be made up of words in dictionary
                if word == s[i - len(word) : i] and d[i - len(word)] == True:
                    d[i] = True

        return d[-1]
```

---

<a id="lc-300"></a>
:heavy_check_mark: :orange_book: [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/): dp[i] = length of the longest increasing subsequence ending at nums[i], O(n^2)

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        # Reference Video: https://www.youtube.com/watch?v=CE2b_-XfVDk

        # dp[i] = longest increasing subsequence which ends at nums[i]
        dp = [1] * len(nums)

        for i in range(len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp)

        # Time: O(N^2), where N <= 2500 is the number of elements in array nums.
        # Space: O(N)

```

---

<a id="lc-1143"></a>
:heavy_check_mark: :orange_book: [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/): dp[i][j] = LCS of the two prefixes; match -> diagonal + 1, else max(top, left)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)

        # dp[i][j] = longest common subsequence of text2[:i], text1[:j]
        dp = [[0] * (m + 1) for _ in range(n + 1)]

        for i in range(n):
            for j in range(m):
                if text2[i] == text1[j]:
                    dp[i + 1][j + 1] = dp[i][j] + 1
                else:
                    # get max from top or left element
                    dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j])

        return dp[-1][-1]
```

---

<a id="lc-647"></a>
:heavy_check_mark: :orange_book: [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/): dp[i][j] = whether s[i..j] is a palindrome, filled by window size

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        n = len(s)
        dp = [[False] * n for _ in range(n)]
        # dp[i][j] = whether s[i:j] is a palindrome

        result = 0

        # check for a window of size 1 (single char)
        for i in range(n):
            dp[i][i] = True
            result += 1

        # check for a window of size 2
        for i in range(n - 1):
            if s[i] == s[i + 1]:
                dp[i][i + 1] = True
                result += 1

        # check for a window of size 3+
        for size in range(3, n + 1):
            for left in range(n - size + 1):
                right = left + size - 1
                if dp[left + 1][right - 1] == True and s[left] == s[right]:
                    dp[left][right] = True
                    result += 1

        return result
```

---

<a id="lc-72"></a>
:heavy_check_mark: :closed_book: [72. Edit Distance](https://leetcode.com/problems/edit-distance/): dp[i][j] = edit distance between word1[:i] and word2[:j]; 1 + min(insert, delete, replace)

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:

        # Ref: https://www.youtube.com/watch?v=We3YDTzNXEk

        if not word1 and not word2: # they are already the same empty string
            return 0
        if not word1:
            return len(word2)
        if not word2:
            return len(word1)

        # dp[i][j] = minimum number of operations required to convert s1[0..i-1] string to s2[0..j-1] string
        m, n = len(word1), len(word2)
        dp = [[float('inf') for _ in range(n + 1)] for _ in range(m + 1)]

        # initialization
        for i in range(m + 1): # the first column
            dp[i][0] = i
        for j in range(n + 1): # the first row
            dp[0][j] = j

        # dp
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    # if we are comparing the same character, look to top-left corner
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    # we find the minimum element from left / top / top-left corner
                    # and then we add 1 to account for the operation of changing the current character
                    dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])

        return dp[m][n]
```

---

<a id="lc-10"></a>
:heavy_check_mark: :closed_book: [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/): dp[i][j] = whether s[:i] matches p[:j]; `*` means zero occurrences (j - 2) or one more (i - 1)

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:

        # Ref: https://leetcode.com/problems/regular-expression-matching/discuss/5723/My-DP-approach-in-Python-with-comments-and-unittest

        # YouTube: https://www.youtube.com/watch?v=l3hda49XcDE

        # The DP table and the string s and p use the same indexes i and j, but
        # table[i][j] means the match status between s[:i] and p[:j], i.e.
        # table[0][0] means the match status of two empty strings, and
        # table[1][1] means the match status of s[0] and p[0]. Therefore, when
        # refering to the i-th and the j-th characters of s and p for updating
        # table[i][j], we use s[i - 1] and p[j - 1].

        # Initialize the table with False.
        # The first column is automatically satisfied (if we have an empty pattern, there will never be a match)
        table = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]

        # Update the corner case of matching two empty strings.
        table[0][0] = True

        # Update the corner case of when s is an empty string but p is not.
        # Since each '*' can eliminate the character before it, the table is
        # horizontally updated by the one before previous (on first row).
        for i in range(1, len(p) + 1):
            if p[i - 1] == '*':
                table[0][i] = table[0][i - 2]

        # Populate DP table
        for i in range(1, len(s) + 1):
            for j in range(1, len(p) + 1):

                # since the table index starts at 1, we need to decrement by 1 to access the original arrays
                this_pttn, this_char = p[j - 1], s[i - 1]

                if this_pttn == '.' or this_pttn == this_char: # if pattern is '.' or the characters match
                    # we can just ignore both character in string and pattern (by looking to the top-left diagonal element)
                    table[i][j] = table[i - 1][j - 1]

                elif this_pttn == '*':

                    # 0 occurrence of the pattern
                    # we look two steps to the left in table
                    table[i][j] = table[i][j - 2]

                    # one or more occurrences of the pattern
                    # we need the pattern char before '*' to match the char in string or is '.' for arbitrary char
                    prev_pttn = p[j - 2]

                    if prev_pttn == this_char or prev_pttn == '.':
                        # look up one row on the table, i.e. try ignoring current char in string and re-do comparison
                        # the OR here is to account for the "0 occurrence of the pattern" case we just assigned before
                        table[i][j] = table[i][j] or table[i - 1][j]

                else:
                    table[i][j] = False

        return table[-1][-1]
```

---

## 4. Decision Making

<a id="lc-53"></a>
:heavy_check_mark: :green_book: [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/): Kadane's algorithm: extend the current subarray or restart at this element

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:

        if not nums:
            return None

        # Kadane's Algorithm for Maximum Subarray problem

        curSum = maxSum = nums[0]

        for num in nums[1:]:
            curSum = max(num,            # either this num
                         curSum + num)   # OR this number plus the maximum subarray sum right before this element
            maxSum = max(maxSum, curSum) # update maxSum (i.e., global maximum sum)

        return maxSum
```

---

<a id="lc-152"></a>
:heavy_check_mark: :orange_book: [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/): track both the current max and the current min product (a negative number swaps them)

```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """

        # Ref: https://www.youtube.com/watch?v=lXVy6YWFcRM
        # observations:
        # 1. if all positive, the product can only be increasing as we include more numbers
        # 2. if there is negative or 0, the product will be more complicated
        # So, we need to keep track of currentMin and currentMax

        result = max(nums)
        curMin, curMax = 1, 1

        for num in nums:

            if num == 0: # we don't want to mess up the curMin and curMax, reset both to 1
                curMin, curMax = 1, 1
                continue

            # re-compute the current min and current max
            prev_curMax = curMax
            curMax = max(num * curMax, num * curMin, num)
            curMin = min(num * prev_curMax, num * curMin, num)

            result = max(result, curMax, curMin)

        return result
```

---

<a id="lc-198"></a>
:heavy_check_mark: :orange_book: [198. House Robber](https://leetcode.com/problems/house-robber/): dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]), rob this house or skip it

```python
class Solution:
    def rob(self, nums: List[int]) -> int:

        if not nums:
            return 0
        if len(nums) == 1: return nums[0]
        if len(nums) == 2: return max(nums)

        # dp[i] = maximum loot we can get by considering up to house [i]
        dp = [0 for _ in range(len(nums))]

        # initialization
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])

        for i in range(2, len(nums)):

            # At each sub problem
            # We either include the current house for robber or not
            # If we include it : then it means we rob the current house and leave the last house
            #     i.e rob the lastToLast house
            # If we exclude it : then it means we rob the last house

            # if we include, max loot = max loot up to the last-to-last house + current house loot
            include = nums[i] + dp[i - 2]
            # if we exclude, max loot = max loot up to the last house
            exclude = dp[i - 1]

            dp[i] = max(include, exclude)

        return dp[len(nums) - 1] # equivalent to dp[-1]
```

---

<a id="lc-55"></a>
:heavy_check_mark: :orange_book: [55. Jump Game](https://leetcode.com/problems/jump-game/): dp[i] = farthest index reachable using nums[0..i]

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:

        if not nums:
            return False

        # dp[i] = the farthest index we can reach given allowed steps nums[0:i]
        dp = [0 for _ in range(len(nums))]

        # initialization (the farthest position we can reach from nums[0] is the maximum jump length at this position = nums[0])
        dp[0] = nums[0]

        for i in range(1, len(nums) - 1):

            if dp[i - 1] < i: # index i cannot be reached starting from any index in range of [0: i - 1]
                return False  # we can never reach the end

            # the farthest position we can reach is either the same as before,
            # OR is the current position + the maximum jump length at this position
            dp[i] = max(dp[i - 1], i + nums[i])

            if dp[i] >= len(nums) - 1: # we can already jump to the end
                return True

        # see whether we can reach the end starting from any index in range of [0: len(nums) - 2]
        return dp[len(nums) - 2] >= len(nums) - 1
```

---

<a id="lc-45"></a>
:heavy_check_mark: :orange_book: [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/): greedy 1-D BFS: each jump expands the window [left, right] to the farthest reachable index

```python
class Solution:
    def jump(self, nums: List[int]) -> int:

        # Ref:
        # https://www.youtube.com/watch?v=dJ7sWiOoK7g

        n = len(nums)
        if n == 1:
            return 0

        result = 0

        # the window of search space
        left, right = 0, 0

        # stop when our window's right boundary meets the end
        # cause when right == len(nums) - 1, we still have result += 1 even though we already reached the end
        # so we are taking a redundant step
        while right < len(nums) - 1:
            farthest_idx = 0

            for i in range(left, right + 1): # we are considering the right index INCLUSIVE
                farthest_from_i = i + nums[i] # the farthest position we can reach by jumping from position [i]

                # we are finding the global farthest position ->
                # i.e., farthest position we can reach from any position in window [left, right] (right inclusive)
                farthest_idx = max(farthest_idx, farthest_from_i)

            # update the window boundary (left moves to the right of right, right moves to the farthest index)
            left = right + 1
            right = farthest_idx

            result += 1 # we made a jump

        return result
```

---
