# Dynamic Programming
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [Full List](https://leetcode.com/problemset/all/?topicSlugs=sorting&page=1)
- **Reference 1**: [Top 5 DP Patterns - NeetCode YouTube](https://www.youtube.com/watch?v=mBNrRy2_hVs)
- **Reference 2**: [DP Patterns - LeetCode](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns)
- **Reference 3**: [DP Patterns Problems - NeetCode](https://docs.google.com/spreadsheets/d/1pEzcVLdj7T4fv5mrNhsOvffBnsUH07GZk7c2jD-adE0/edit#gid=0)
---

## DP Patterns from [This LeetCode Post](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns)

### 1. Minimum (Maximum) Path / Cost to Reach a Target

- **Statement**

```
Given a target find minimum (maximum) cost / path / sum to reach the target.
```

- **Approach**

```
Choose minimum (maximum) path among all possible paths before the current state, then add value for the current state.

>> routes[i] = min(routes[i-1], routes[i-2], ... , routes[i-k]) + cost[i]
```


- **Template**

```
for (int i = 1; i <= target; ++i) {
   for (int j = 0; j < ways.size(); ++j) {
       if (ways[j] <= i) {
           dp[i] = min(dp[i], dp[i - ways[j]] + cost / path / sum) ;
       }
   }
}
 
return dp[target]
```

---
