# LeetCode - Dynamic Programming | Problems
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---
# Problem List According to [This LeetCode Post](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns)
## Minimum (Maximum) Path to Reach a Target

- **Statement**

```
Given a target find minimum (maximum) cost / path / sum to reach the target.
```

- **Approach**

```
Choose minimum (maximum) path among all possible paths before the current state, then add value for the current state.

>> routes[i] = min(routes[i-1], routes[i-2], ... , routes[i-k]) + cost[i]
```

:orange_book: [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/): dp[i,j] = minimum path sum from top left to position[i][j]

:green_book: [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/): dp[i] = min cost to reach step i

:orange_book: [120. Triangle](https://leetcode.com/problems/triangle/): dp[i][j] = minimum path sum from top to triangle[i][j]

:orange_book: [221. Maximal Square](https://leetcode.com/problems/maximal-square/): dp[i][j] = maximum square side length we can achieve when reaching matrix[i][j]

:orange_book: [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/): dp[i] = least number of perfect square numbers that sum to i

:closed_book: [174. Dungeon Game](https://leetcode.com/problems/dungeon-game/):dp[i][j] = minimum health we need to start from this point and reach bottom-right

:orange_book: [322. Coin Change](https://leetcode.com/problems/coin-change/): dp[i] = fewest number of coins needed to make up amount i

:orange_book: [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/): dp[i][j] = largest subset of strs such that there are at most [i] 0s and [j] 1s

:orange_book: [650. 2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard/): dp[i] = minimum number of operations to print A for [i] times

:closed_book: [871. Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/): dp[i] = furthest distance we can get after [i] number of refueling stops

:orange_book: [931. Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/): dp[i][j] = minimum sum of falling path up to matrix[i][j]

:orange_book: [983. Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/): dp[i] = minimum cost to fulfill travel plan up to day i

:orange_book: [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/): 

---
