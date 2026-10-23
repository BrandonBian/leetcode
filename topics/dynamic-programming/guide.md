# Dynamic Programming · Guide

[← All topics](../../README.md) &nbsp;·&nbsp; **Guide** &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; [Solutions](solutions.md)

- **Reference 1**: [DP Patterns - LeetCode](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns) (the problem list follows this post's patterns)
- **Reference 2**: [DP Patterns - LeetCode - V2](https://leetcode.com/problems/target-sum/discuss/455024/DP-IS-EASY!-5-Steps-to-Think-Through-DP-Questions.)
- **Reference 3**: [Top 5 DP Patterns - NeetCode YouTube](https://www.youtube.com/watch?v=mBNrRy2_hVs)
- **Reference 4**: [DP Patterns Problems - NeetCode](https://docs.google.com/spreadsheets/d/1pEzcVLdj7T4fv5mrNhsOvffBnsUH07GZk7c2jD-adE0/edit#gid=0)

---

## 1. Minimum (Maximum) Path / Cost to Reach a Target

- **Statement**

```text
Given a target find minimum (maximum) cost / path / sum to reach the target.
```

- **Approach**

```text
Choose minimum (maximum) path among all possible paths before the current state, then add value for the current state.

>> routes[i] = min(routes[i-1], routes[i-2], ... , routes[i-k]) + cost[i]
```

- **Template**

```python
dp = [0] + [float('inf')] * target

for i in range(1, target + 1):
    for way in ways:
        if way <= i:
            dp[i] = min(dp[i], dp[i - way] + cost)  # cost / path / sum of taking this way

return dp[target]
```

---

## 2. Number of Distinct / Unique Ways to Do Something

- **Statement**

```text
Given a target find a number of distinct ways to reach the target.
```

- **Approach**

```text
Sum all possible ways to reach the current state.
Generate sum for all values in the target and return the value for the target.

>> routes[i] = routes[i-1] + routes[i-2], ... , + routes[i-k]
```

- **Template**

```python
# Some questions point out the number of repetitions, in that case, add one more loop to simulate every repetition.

dp = [1] + [0] * target  # one way to reach 0: take nothing

for i in range(1, target + 1):
    for way in ways:
        if way <= i:
            dp[i] += dp[i - way]

return dp[target]
```

---

## 3. DP on Strings & Subsequences

- **Statement**

```text
Given one or two strings (or arrays), find something about their prefixes / substrings / subsequences.
```

- **Approach**

```text
Two strings: dp[i][j] describes the prefixes s1[:i] and s2[:j].
    If the last characters match, build on dp[i-1][j-1]; otherwise combine dp[i-1][j] and dp[i][j-1].
One string: dp[i][j] describes the substring s[i..j], filled by increasing length.
```

---

## 4. Decision Making

- **Statement**

```text
At each element, decide whether to use it or not (rob / skip, extend / restart), and optimize the result.
```

- **Approach**

```text
Keep the best answer for each choice at the current position, built from the best answers at the previous positions.

>> dp[i] = max(dp[i-1], dp[i-2] + nums[i])        (House Robber)
>> cur = max(nums[i], cur + nums[i])               (Kadane, Maximum Subarray)
```
