# Graphs: Grid DFS / BFS & Topological Sort · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. DFS / BFS on a Grid

<a id="lc-79"></a>
:heavy_check_mark: :orange_book: [79. Word Search](https://leetcode.com/problems/word-search/)

Solution: see [Recursion & Backtracking](../backtracking/solutions.md#lc-79).

---

<a id="lc-200"></a>
:heavy_check_mark: :orange_book: [200. Number of Islands](https://leetcode.com/problems/number-of-islands/): DFS from each island grid cell

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:

        def dfs(row, col):
            if row < 0 or row >= len(grid) or col < 0 or col >= len(grid[0]) or grid[row][col] != '1':
                return

            grid[row][col] = '#'

            for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                dfs(row + dx, col + dy)

        if not grid or not grid[0]:
            return 0

        result = 0

        for row in range(len(grid)):
            for col in range(len(grid[0])):
                if grid[row][col] == '1':
                    dfs(row, col)
                    result += 1

        return result
```

---

<a id="lc-994"></a>
:heavy_check_mark: :orange_book: [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/): BFS from each rotten orange grid cell

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        if not grid or not grid[0]:
            return 0

        fresh = 0
        rotten = deque() # for BFS on each rotten orange

        # first pass through the grid to get initialized info
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                if grid[row][col] == 2:
                    rotten.append((row, col))
                elif grid[row][col] == 1:
                    fresh += 1

        time = 0

        # while there is rotten in the queue and there is still fresh orange
        while rotten and fresh > 0:
            time += 1

            for _ in range(len(rotten)):
                row, col = rotten.popleft()
                for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                    new_row, new_col = row + dx, col + dy
                    if new_row < 0 or new_row >= len(grid) or new_col < 0 or new_col >= len(grid[0]):
                        continue
                    if grid[new_row][new_col] == 0 or grid[new_row][new_col] == 2:
                        continue

                    fresh -= 1
                    grid[new_row][new_col] = 2 # fresh orange becomes rotten
                    rotten.append((new_row, new_col))

        return time if fresh == 0 else -1
```

---

<a id="lc-417"></a>
:heavy_check_mark: :orange_book: [417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/): DFS from each peripheral grid cells (that are adjacent to ocean)

```python
DIRECTIONS = [(1, 0), (-1, 0), (0, 1), (0, -1)]

class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:

        # Explanation of idea:
        # https://leetcode.com/problems/pacific-atlantic-water-flow/discuss/1126938/Short-and-Easy-w-Explanation-and-diagrams-or-Simple-Graph-traversals-DFS-and-BFS

        # Ref solution:
        # https://leetcode.com/problems/pacific-atlantic-water-flow/discuss/90739/Python-DFS-bests-85.-Tips-for-all-DFS-in-matrix-question.

        if not heights or not heights[0]:
            return []

        m, n = len(heights), len(heights[0])

        pacific_visited = [[False for _ in range(n)] for _ in range(m)]
        atlantic_visited = [[False for _ in range(n)] for _ in range(m)]

        result = []

        for i in range(m):
            self.dfs(heights, i, 0, pacific_visited) # DFS from top edge
            self.dfs(heights, i, n - 1, atlantic_visited) # DFS from right edge

        for j in range(n):
            self.dfs(heights, 0, j, pacific_visited) # DFS from left edge
            self.dfs(heights, m - 1, j, atlantic_visited) # DFS from bottom edge

        # find union of pacific_visited and altantic_visited
        for i in range(m):
            for j in range(n):
                if pacific_visited[i][j] and atlantic_visited[i][j]:
                    result.append([i, j])

        return result



    def dfs(self, matrix, i, j, visited):
        visited[i][j] = True

        m, n = len(matrix), len(matrix[0])

        for dx, dy in DIRECTIONS:
            new_x, new_y = i + dx, j + dy

            if new_x < 0 or new_x >= m or new_y < 0 or new_y >= n or visited[new_x][new_y] or \
            matrix[new_x][new_y] < matrix[i][j]: # we want to move upwards since we are moving from ocean to mountain
                continue

            self.dfs(matrix, new_x, new_y, visited)
```

---

<a id="lc-329"></a>
:heavy_check_mark: :closed_book: [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/): DFS from each cell + memoization, DFS **with return value**

```python
DIRECTIONS = [(1, 0), (-1, 0), (0, 1), (0, -1)]

class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:

        if not matrix:
            return 0

        m, n = len(matrix), len(matrix[0])
        result = float('-inf')

        # a cache to record the length of longest increasing path
        visited = [[-1 for _ in range(n)] for _ in range(m)]

        for i in range(m):
            for j in range(n): # DFS from each cell to find maximum increasing path length
                curr_len = self.dfs(matrix, i, j, visited)
                result = max(result, curr_len)

        return result

    def dfs(self, matrix, i, j, visited):

        if visited[i][j] != -1:
            return visited[i][j]

        m, n = len(matrix), len(matrix[0])
        res = 1 # minimum length of increasing path is 1 (element by itself)

        for di, dj in DIRECTIONS:
            new_i, new_j = i + di, j + dj

            # we only want strictly increasing path
            if new_i < 0 or new_i >= m or new_j < 0 or new_j >= n or matrix[new_i][new_j] <= matrix[i][j]:
                continue

            length = 1 + self.dfs(matrix, new_i, new_j, visited)
            res = max(res, length)

        visited[i][j] = res

        return res
```

---

<a id="lc-130"></a>
:heavy_check_mark: :orange_book: [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/): DFS from each "O" cell in periphery

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """Do not return anything, modify board in-place instead."""

        def dfs(i, j):
            if 0 <= i < len(board) and 0 <= j < len(board[0]) and board[i][j] == 'O':
                board[i][j] = '.'
                for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                    dfs(i + dx, j + dy)

        if not board or not board[0]:
            return

        m, n = len(board), len(board[0])

        for i in range(m):
            dfs(i, 0) # left column
            dfs(i, n - 1) # right column

        for i in range(n):
            dfs(0, i) # top row
            dfs(m - 1, i) # bottom row

        for i in range(m):
            for j in range(n):
                board[i][j] = 'X' if board[i][j] != '.' else 'O'
```

---

<a id="lc-695"></a>
:heavy_check_mark: :orange_book: [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/): DFS from each '1' cell, DFS **with return value**

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:

        self.max_area = 0

        def dfs(i, j):

            if not (0 <= i < len(grid) and 0 <= j < len(grid[0]) and grid[i][j] == 1):
                return 0

            grid[i][j] = 2

            return 1 + dfs(i + 1, j) + dfs(i, j + 1) + dfs(i - 1, j) + dfs(i, j - 1)

        for i in range(len(grid)):
            for j in range(len(grid[0])):
                self.max_area = max(self.max_area, dfs(i, j))

        return self.max_area
```

---

<a id="lc-1254"></a>
:wavy_dash: :orange_book: [1254. Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/): DFS from each '0' land cell, DFS **with return value**

```python
class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:

        def dfs(i, j):
            if i < 0 or i >= len(grid) or j < 0 or j >= len(grid[0]) or grid[i][j] != 0:
                return

            grid[i][j] = 2

            for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                dfs(i + dx, j + dy)

        result = 0
        m, n = len(grid), len(grid[0])

        for i in range(m):
            dfs(i, 0) # left
            dfs(i, n - 1) # right

        for i in range(n):
            dfs(0, i) # top
            dfs(m - 1, i) # bottom

        # now all islands connected to the periphery are marked as 2
        # we now perform dfs to mark all connected islands, and count occurrence

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    dfs(i, j)
                    result += 1

        return result
```

---

<a id="lc-733"></a>
:wavy_dash: :green_book: [733. Flood Fill](https://leetcode.com/problems/flood-fill/): DFS from source cell

```python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:

        def dfs(i, j):

            image[i][j] = color

            for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                x, y = i + dx, j + dy

                if 0 <= x < len(image) and 0 <= y < len(image[0]) and image[x][y] == self.old:
                    dfs(x, y)

        self.old = image[sr][sc]

        if self.old != color:
            dfs(sr, sc)

        return image
```

---

<a id="lc-463"></a>
:wavy_dash: :green_book: [463. Island Perimeter](https://leetcode.com/problems/island-perimeter/): DFS on any one land grid is sufficient

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:

        def dfs(i, j):

            if not((0 <= i < len(grid)) and (0 <= j < len(grid[0])) and grid[i][j] == 1):
                return

            grid[i][j] = -1

            self.edges += (i + 1 >= len(grid) or grid[i + 1][j] == 0) + (j + 1 >= len(grid[0]) or grid[i][j + 1] == 0) + (i - 1 < 0 or grid[i - 1][j] == 0) + (j - 1 < 0 or grid[i][j - 1] == 0)

            for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                dfs(i + dx, j + dy)

        self.edges = 0

        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    dfs(i, j)
                    return self.edges
```

---

## 2. Topological Sort

<a id="lc-207"></a>
:heavy_check_mark: :orange_book: [207. Course Schedule](https://leetcode.com/problems/course-schedule/): Kahn's algorithm: BFS from in-degree-0 nodes; every course is finished iff there is no cycle

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # idea: find the in-degree of each node,
        # put each node with 0 in-degree into a queue as starting nodes
        # Repeat: take one node from queue and decrement in-degree for connected nodes
        # if we find a new node with 0 in-degree, add it to queue

        # create an adjacency matrix (store neighboring nodes)
        graph = [[] for i in range(numCourses)]

        # identify the in-degree of each node and construct adjacency matrix
        in_degree = [0] * numCourses
        for node_in, node_out in prerequisites:
            graph[node_out].append(node_in)
            in_degree[node_in] += 1

        # put each node with in-degree = 0 into a queue
        from collections import deque
        queue = deque()

        for i in range(numCourses):
            if in_degree[i] == 0: # we can learn this class
                queue.append(i)

        topo_order = []

        while queue:
            now_pos = queue.popleft()
            topo_order.append(now_pos)

            # for each neighboring node, in-degree decrement by 1
            for next_pos in graph[now_pos]:
                in_degree[next_pos] -= 1
                if in_degree[next_pos] == 0:
                    queue.append(next_pos)

        # return topological order if we have finished all required courses
        return True if len(topo_order) == numCourses else False
```

---
