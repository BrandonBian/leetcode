# Stacks & Queues · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. Stack

:heavy_check_mark: :green_book: [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/): push opening brackets; each closing bracket must match the top of the stack &nbsp;→ [solution](solutions.md#lc-20)

:heavy_check_mark: :green_book: [155. Min Stack](https://leetcode.com/problems/min-stack/): a second stack keeps the running minimum &nbsp;→ [solution](solutions.md#lc-155)

:heavy_check_mark: :orange_book: [394. Decode String](https://leetcode.com/problems/decode-string/): stack of (previous string, repeat count); build the current string until `]` &nbsp;→ [solution](solutions.md#lc-394)

:heavy_check_mark: :closed_book: [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/): stack of indices seeded with -1 as the base of the current valid run &nbsp;→ [solution](solutions.md#lc-32)

## 2. Monotonic Stack / Queue

:heavy_check_mark: :orange_book: [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/): monotonic decreasing stack of indices; pop when a warmer day arrives &nbsp;→ [solution](solutions.md#lc-739)

:heavy_check_mark: :closed_book: [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/): for each bar, count consecutive bars >= it on both sides (jumping by earlier counts) &nbsp;→ [solution](solutions.md#lc-84)

:heavy_check_mark: :closed_book: [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/): build histogram heights row by row, then apply 84 to each row &nbsp;→ [solution](solutions.md#lc-85)

:heavy_check_mark: :closed_book: [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/): monotonic decreasing deque of indices; the front is the window maximum &nbsp;→ [solution](solutions.md#lc-239)
