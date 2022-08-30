# LeetCode - Dynamic Programming | Problems
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---
# Problem List According to [LeetCode Post](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns)
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

:orange_book: [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/): dp[i,j] = minimum sum for path up to this location on grid

---
