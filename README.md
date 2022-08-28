# Table of Contents

- **Section 0: Patterns**
- **Section 1: Algorithms**
- **Section 2: Data Structures**
- **Section 3: Time & Space Complexities**
- **Section 4: Auxiliary Collection of Problems & Solutions**

---
## Section 0: Patterns
- Ref 1: [LeetCode Patterns](https://seanprashad.com/leetcode-patterns/)
- Ref 2: [14 Patterns](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)

| Condition | Algorithms to / can Use | Data Structures to / can Use |
| -- | -- | -- |
| Input array is sorted | Binary Search + Two Pointers | -- |
| Asking for all permutations / subsets / combinations | DFS Backtracking | -- |
| Finding the minimum / maximum / k-th position | Binary Search + Heap | -- |
| Binary tree / binary search tree | BFS using queues + DFS using recursion | BT + BST |
| Path finding on a grid / matrix | BFS using queues + DFS using recursion + Step towards directions | -- |
| Linked list | Two Pointers + Dummy Node | Linked List |
| K-Sum / partitioning | Two Pointers | -- |
| Substring / subarray on linear structure | Sliding Window (Two Pointers) | -- |
| Dealing with brackets | -- | Stack |
| Finding occurrence of a certain pattern | -- | Hash Table (Dict, Counter) |
| Maximum / minimum subarray / subset / options | Dynamic Programming | -- |
---

## Section 1: Algorithms

| ID | Algorithm | When To Use | Study Guide | Problems | Solutions | Important Key Points
| -- | -- | -- | -- | -- | -- | -- |
| 1 | **Binary Search** | "minimizing / maximizing / find K-th position" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/binary-search.md) | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/binary-search.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/binary-search-solutions.md) | :white_check_mark: [A Template](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/binary-search.md#white_check_mark-general-template---very-good-please-memorize)
| 2 | **Greatest Common Divisor (GCD) & Least Common Multiple (LCM)** | "total number of positive integers <= N which are divisible by a or b or c" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/gcd-lcm.md) | - | - | :white_check_mark: [Calculation Formulae in Python](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/gcd-lcm.md)
| 3 | **DFS Recursion & Backtracking** | "combination / combination sum / permutations / subsets" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/recursion-backtracking.md) | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/recursion-backtracking.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/recursion-backtracking-solutions.md) | :white_check_mark: [A Template](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/recursion-backtracking.md#--backtracking-problem-solving-templates)
| 4 | **Two Pointers** | "K-Sum / partitioning / sorting" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/two-pointers.md) | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/two-pointers.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/two-pointers-solutions.md) | -
| 5 | **Topological DFS & BFS** | "path finding in a grid or matrix" | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/topological-dfs-bfs.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/tree/main/problems-and-solutions/LeetCode)
| 6 | **Sorting Methods** | 
| 7 | **Dynamic Programming** | 
| 8 | **Prefix & Sufix Sum** | 
---

## Section 2: Data Structures

| ID | Data Structure | Common Topics / Applications | Study Guide | Problems | Solutions | Important Key Points
| -- | -- | -- | -- | -- | -- | -- |
| 1 | **Binary Tree & Binary Search Trees** | "BT and BST traversal / deleting & inserting node / root-to-leaf DFS / construction / validation" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/data-structures/binary-tree.md) | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/binary-tree.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/binary-tree-solutions.md) | :white_check_mark: [Traversal Templates](https://github.com/BrandonBian/LeetCode-Notes/blob/main/data-structures/binary-tree.md#--binary-tree-traversals-and-operations-usually-with-recursion)
| 2 | **Hash Table** | "using Counter / Set / Dictionary / Pattern Hashing" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/data-structures/hash-table.md) | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/hash-table.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/hash-table-solutions.md) | :white_check_mark: [Using and Sorting Python Dictionary & Counter](https://github.com/BrandonBian/LeetCode-Notes/blob/main/data-structures/hash-table.md#--python-dictionary-and-counters-and-how-to-sort-them)
| 3 | **Linked List** | "Linked List cycle detection / cycle locating / slow-fast pointers / dummy nodes / double pointers" | [Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/data-structures/linked-list.md) | [Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/linked-list.md) | [Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/linked-list-solutions.md) | -
| 4 | **Binary Indexed Tree / Fenwick Tree** |
| 5 | **Heaps** |
| 6 | **Trie / Prefix Tree** |

---

## Section 3: Time & Space Complexities

---

## Section 4: Auxiliary Problems & Solutions

### 4.1 LeetCode Top 100 - [[Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/top100.md)] | [[Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/top100-solutions.md)]


### 4.2 LintCode Top 100 - [[Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LintCode/top100.md)] | [Solutions]


### 4.3 LintCode Chapter-by-Chapter Practice - [[Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LintCode/chapter-by-chapter.md)] | [[Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LintCode/chapter-by-chapter-solutions.md)]

