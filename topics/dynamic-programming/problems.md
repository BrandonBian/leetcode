# Dynamic Programming · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. Minimum (Maximum) Path / Cost to Reach a Target

:orange_book: [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/): dp[i,j] = minimum path sum from top left to position[i][j] &nbsp;→ [solution](solutions.md#lc-64)

:green_book: [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/): dp[i] = min cost to reach step i &nbsp;→ [solution](solutions.md#lc-746)

:orange_book: [120. Triangle](https://leetcode.com/problems/triangle/): dp[i][j] = minimum path sum from top to triangle[i][j] &nbsp;→ [solution](solutions.md#lc-120)

:orange_book: [221. Maximal Square](https://leetcode.com/problems/maximal-square/): dp[i][j] = maximum square side length we can achieve when reaching matrix[i][j] &nbsp;→ [solution](solutions.md#lc-221)

:orange_book: [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/): dp[i] = least number of perfect square numbers that sum to i &nbsp;→ [solution](solutions.md#lc-279)

:closed_book: [174. Dungeon Game](https://leetcode.com/problems/dungeon-game/): dp[i][j] = minimum health we need to start from this point and reach bottom-right &nbsp;→ [solution](solutions.md#lc-174)

:orange_book: [322. Coin Change](https://leetcode.com/problems/coin-change/): dp[i] = fewest number of coins needed to make up amount i &nbsp;→ [solution](solutions.md#lc-322)

:orange_book: [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/): dp[i][j] = largest subset of strs such that there are at most [i] 0s and [j] 1s &nbsp;→ [solution](solutions.md#lc-474)

:orange_book: [650. 2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard/): dp[i] = minimum number of operations to print A for [i] times &nbsp;→ [solution](solutions.md#lc-650)

:closed_book: [871. Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/): dp[i] = furthest distance we can get after [i] number of refueling stops &nbsp;→ [solution](solutions.md#lc-871)

:orange_book: [931. Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/): dp[i][j] = minimum sum of falling path up to matrix[i][j] &nbsp;→ [solution](solutions.md#lc-931)

:orange_book: [983. Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/): dp[i] = minimum cost to fulfill travel plan up to day i &nbsp;→ [solution](solutions.md#lc-983)

:orange_book: [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/) &nbsp;→ [solution](solutions.md#lc-1049)

## 2. Number of Distinct Ways to Do Something

:orange_book: [62. Unique Paths](https://leetcode.com/problems/unique-paths/): dp[i][j] = number of unique paths from top-left to [i][j] &nbsp;→ [solution](solutions.md#lc-62)

:orange_book: [91. Decode Ways](https://leetcode.com/problems/decode-ways/): dp[i] = number of ways to decode the first i characters in s &nbsp;→ [solution](solutions.md#lc-91)

:green_book: [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/): dp[i] = F(i) = the i-th fibonacci number &nbsp;→ [solution](solutions.md#lc-509)

:orange_book: [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/): dp[i][j] = number of unique paths from top-left to reach[i][j] &nbsp;→ [solution](solutions.md#lc-63)

:green_book: [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/): dp[i] = number of distinct ways to reach step i &nbsp;→ [solution](solutions.md#lc-70)

:heavy_check_mark: :orange_book: [96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/): dp[n] = sum(dp[i - 1] * dp[n - i]) over every root i (Catalan numbers), memoized &nbsp;→ [solution](solutions.md#lc-96)

:orange_book: [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/): dp[i] = number of possible combinations that add up to i &nbsp;→ [solution](solutions.md#lc-377)

:orange_book: [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/): dp = set() # total values we can get &nbsp;→ [solution](solutions.md#lc-416)

:orange_book: [494. Target Sum](https://leetcode.com/problems/target-sum/): curstops = <key, val> = <target sum, number of ways to reach this sum> &nbsp;→ [solution](solutions.md#lc-494)

:orange_book: [1155. Number of Dice Rolls With Target Sum](https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/): dp[i][j] = number of possible ways to reach target [j] with up to [i] dices &nbsp;→ [solution](solutions.md#lc-1155)

:orange_book: [576. Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths/): dp[i][j][M] = number of ways to reach [i][j] from outside of grid using [M] moves maximum &nbsp;→ [solution](solutions.md#lc-576)

:orange_book: [673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/): two arrays for DP &nbsp;→ [solution](solutions.md#lc-673)

:orange_book: [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/): dp[k][i][j] = probability that the knight is at [i][j] after k steps &nbsp;→ [solution](solutions.md#lc-688)

:orange_book: [790. Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/) &nbsp;→ [solution](solutions.md#lc-790)

:orange_book: [935. Knight Dialer](https://leetcode.com/problems/knight-dialer/): dp[i][j] = number of distinct numbers after [i] jumps, starting from digit [j] &nbsp;→ [solution](solutions.md#lc-935)

:closed_book: [801. Minimum Swaps To Make Sequences Increasing](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/) &nbsp;→ [solution](solutions.md#lc-801)

## 3. DP on Strings & Subsequences

:heavy_check_mark: :orange_book: [139. Word Break](https://leetcode.com/problems/word-break/): dp[i] = True if some word ends at i and dp[i - len(word)] is True &nbsp;→ [solution](solutions.md#lc-139)

:heavy_check_mark: :orange_book: [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/): dp[i] = length of the longest increasing subsequence ending at nums[i], O(n^2) &nbsp;→ [solution](solutions.md#lc-300)

:heavy_check_mark: :orange_book: [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/): dp[i][j] = LCS of the two prefixes; match -> diagonal + 1, else max(top, left) &nbsp;→ [solution](solutions.md#lc-1143)

:heavy_check_mark: :orange_book: [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/): dp[i][j] = whether s[i..j] is a palindrome, filled by window size &nbsp;→ [solution](solutions.md#lc-647)

:heavy_check_mark: :closed_book: [72. Edit Distance](https://leetcode.com/problems/edit-distance/): dp[i][j] = edit distance between word1[:i] and word2[:j]; 1 + min(insert, delete, replace) &nbsp;→ [solution](solutions.md#lc-72)

:heavy_check_mark: :closed_book: [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/): dp[i][j] = whether s[:i] matches p[:j]; `*` means zero occurrences (j - 2) or one more (i - 1) &nbsp;→ [solution](solutions.md#lc-10)

## 4. Decision Making

:heavy_check_mark: :green_book: [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/): Kadane's algorithm: extend the current subarray or restart at this element &nbsp;→ [solution](solutions.md#lc-53)

:heavy_check_mark: :orange_book: [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/): track both the current max and the current min product (a negative number swaps them) &nbsp;→ [solution](solutions.md#lc-152)

:heavy_check_mark: :orange_book: [198. House Robber](https://leetcode.com/problems/house-robber/): dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]), rob this house or skip it &nbsp;→ [solution](solutions.md#lc-198)

:heavy_check_mark: :orange_book: [55. Jump Game](https://leetcode.com/problems/jump-game/): dp[i] = farthest index reachable using nums[0..i] &nbsp;→ [solution](solutions.md#lc-55)

:heavy_check_mark: :orange_book: [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/): greedy 1-D BFS: each jump expands the window [left, right] to the farthest reachable index &nbsp;→ [solution](solutions.md#lc-45)
