# Graphs: Grid DFS / BFS & Topological Sort · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. DFS / BFS on a Grid

:heavy_check_mark: :orange_book: [79. Word Search](https://leetcode.com/problems/word-search/) &nbsp;→ [solution in Recursion & Backtracking](../backtracking/solutions.md#lc-79)

:heavy_check_mark: :orange_book: [200. Number of Islands](https://leetcode.com/problems/number-of-islands/): DFS from each island grid cell &nbsp;→ [solution](solutions.md#lc-200)

:heavy_check_mark: :orange_book: [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/): BFS from each rotten orange grid cell &nbsp;→ [solution](solutions.md#lc-994)

:heavy_check_mark: :orange_book: [417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/): DFS from each peripheral grid cells (that are adjacent to ocean) &nbsp;→ [solution](solutions.md#lc-417)

:heavy_check_mark: :closed_book: [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/): DFS from each cell + memorization, DFS **with return value** &nbsp;→ [solution](solutions.md#lc-329)

:heavy_check_mark: :orange_book: [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/): DFS from each "O" cell in periphery &nbsp;→ [solution](solutions.md#lc-130)

:heavy_check_mark: :orange_book: [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/): DFS from each '1' cell, DFS **with return value** &nbsp;→ [solution](solutions.md#lc-695)

:wavy_dash: :orange_book: [1254. Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/): DFS from each '0' land cell, DFS **with return value** &nbsp;→ [solution](solutions.md#lc-1254)

:wavy_dash: :green_book: [733. Flood Fill](https://leetcode.com/problems/flood-fill/): DFS from source cell &nbsp;→ [solution](solutions.md#lc-733)

:wavy_dash: :green_book: [463. Island Perimeter](https://leetcode.com/problems/island-perimeter/): DFS on any one land grid is sufficient &nbsp;→ [solution](solutions.md#lc-463)

## 2. Topological Sort

:heavy_check_mark: :orange_book: [207. Course Schedule](https://leetcode.com/problems/course-schedule/): Kahn's algorithm: BFS from in-degree-0 nodes; every course is finished iff there is no cycle &nbsp;→ [solution](solutions.md#lc-207)
