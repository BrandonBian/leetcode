# LintCode - Chapter by Chapter Practice Problems | [[Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LintCode/chapter-by-chapter-solutions.md)]
- **All problems are LintCode problems, solved in Python.**
- **Relevant keypoints are also noted.**
- **Please pay special attention to the chapters that are marked as LIVE**

---

## Chapter 1 (LIVE): Introduction (Part I)
**No practice Problems**

---

## Chapter 2: Introduction (Part II)

:orange_book: [200 · Longest Palindromic Substring](https://www.lintcode.com/problem/200/): Enumeration from middle with two pointers / Dynamic Programming

:orange_book: [667 · Longest Palindromic Subsequence](https://www.lintcode.com/problem/667/): Dynamic Programming

---

## Chapter 3: Introduction (Part III)

:green_book: [13 · Implement strStr()](https://www.lintcode.com/problem/13/): Naive for loop

:orange_book: [187 · Gas Station](https://www.lintcode.com/problem/187/): Greedy algorithm

---

## Chapter 4: Algorithm Complexity & Two Pointers

:orange_book: [415 · Valid Palindrome](https://www.lintcode.com/problem/415/): Remove special characters, face-to-face two pointers

:orange_book: [891 · Valid Palindrome II](https://www.lintcode.com/problem/891/): Face-to-face two pointers

---

## Chapter 5: Two Sorting Algorithms (Quick Sort + Merge Sort + Quick Select)

:green_book: [463 · Sort Integers](https://www.lintcode.com/problem/463/): Quick Sort / Merge Sort

:orange_book: [5 · Kth Largest Element](https://www.lintcode.com/problem/5/description): Quick Select

---

## Chapter 6: Binary Search (using Recursion)

:green_book: [366 · Fibonacci](https://www.lintcode.com/problem/366/): General Recursion is too slow, use Dynamic Programming

:green_book: [457 · Classical Binary Search](https://www.lintcode.com/problem/457/): Binary Search but using Recursion

---

## Chapter 7: Binary Search (introducing a template)

:green_book: [457 · Classical Binary Search](https://www.lintcode.com/problem/457/): Binary Search using the template

:green_book: [458 · Last Position of Target](https://www.lintcode.com/problem/458/): Binary Search using the template

---

## Auxiliary Problems: Ch. 1 ~ 7

:green_book: [1790 · Rotate String II](https://www.lintcode.com/problem/1790/description?_from=collection&fromId=161): String manipulation and modulus operation

:closed_book: [594 · strStr II](https://www.lintcode.com/problem/594/?_from=collection&fromId=161): Rabin-Karp (Hashing)

:closed_book: [841 · String Replace](https://www.lintcode.com/problem/841/?_from=collection&fromId=161): A enumerating method using Set and Dictionary

---

## Chapter 8 (LIVE): Face-to-face Two Pointers & Partition Problems

:green_book: [56 · Two Sum](https://www.lintcode.com/problem/56/): Sort, then face-to-face two pointers

:green_book: [607 · Two Sum III - Data structure design](https://www.lintcode.com/problem/607/): Data structure design, add and find functionalities

:orange_book: [57 · 3Sum](https://www.lintcode.com/problem/57/?_from=collection&fromId=161): Reduce to 2Sum, removing duplicates

:orange_book: [382 · Triangle Count](https://www.lintcode.com/problem/382/): Fix longest edge and 2Sum on shorter ones

:orange_book: [31 · Partition Array](https://www.lintcode.com/problem/31/): Two pointers from either end of array going to middle, exchange misplaced elements

:orange_book: [148 · Sort Colors](https://www.lintcode.com/problem/148/): Quick sort partition (also two pointers)

:orange_book: [143 · Sort Colors II](https://www.lintcode.com/problem/143/): Arbitrary number of colors, two pointers partitioning with Recursion

:green_book: [539 · Move Zeroes](https://www.lintcode.com/problem/539/): Two pointers, one for filling, one for moving

---

## Chapter 9 (LIVE): Binary Search

:orange_book: [447 · Search in a Big Sorted Array](https://www.lintcode.com/problem/447/): Exponential Backoff & Binary Search

:orange_book: [460 · Find K Closest Elements](https://www.lintcode.com/problem/460/): Binary Search find upper closest index then compare bottom and left interleavingly

:orange_book: [585 · Maximum Number in Mountain Sequence](https://www.lintcode.com/problem/585/): Identify whether [mid] is on increasing or decreasing side

:orange_book: [159 · Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/159/): Identify which segment is [mid] located at

:orange_book: [62 · Search in Rotated Sorted Array](https://www.lintcode.com/problem/62/): Identify which segment is [mid] located at

:orange_book: [75 · Find Peak Element](https://www.lintcode.com/problem/75/): Identify which segment is [mid] located at

:closed_book: [183 · Wood Cut](https://www.lintcode.com/problem/183/): Maximize length of small pieces [mid] s.t. [mid] satisfies the given conditions

---

## Chapter 10: Queues

:green_book: [492 · Implement Queue by Linked List](https://www.lintcode.com/problem/492/): Implementing enqueue and dequeue (using Linked Lists)

---

## Chapter 11: BFS and Graphs

:green_book: [69 · Binary Tree Level Order Traversal](https://www.lintcode.com/problem/69/): BFS using single queue / double queue / dummy nodes

:orange_book: [1179 · Friend Circles](https://www.lintcode.com/problem/1179/): BFS with a dictionary to track visited nodes

---

## Chapter 12: DFS Traversal on BST

:green_book: [480 · Binary Tree Paths](https://www.lintcode.com/problem/480/): DFS on BST

:green_book: [93 · Balanced Binary Tree](https://www.lintcode.com/problem/93/): DFS on BST with multiple return values

:green_book: [97 · Maximum Depth of Binary Tree](https://www.lintcode.com/problem/97/): DFS on BST

:green_book: [628 · Maximum Subtree](https://www.lintcode.com/problem/628/): DFS on BST with multiple return values

---

## Chapter 13: Non-recursion Traversal on BST

:closed_book: [86 · Binary Search Tree Iterator](https://www.lintcode.com/problem/86/): Using stack

:green_book: [900 · Closest Binary Search Tree Value](https://www.lintcode.com/problem/900/): Basic binary search tree traversal

---

## Chapter 14 (LIVE): BFS

:orange_book: [137 · Clone Graph](https://www.lintcode.com/problem/137/): BFS to find all nodes, then create mappings for new nodes and populate edge connections

:closed_book: [120 · Word Ladder](https://www.lintcode.com/problem/120/): Change problem to a graph and run BFS with set

:green_book: [433 · Number of Islands](https://www.lintcode.com/problem/433/): BFS on 2D matrix, need to identify whether step is valid

:orange_book: [611 · Knight Shortest Path](https://www.lintcode.com/problem/611/): BFS on 2D matrix, need to identify whether step is valid

:orange_book: [616 · Course Schedule II](https://www.lintcode.com/problem/616/): Topological sorting with adjacency, in degree, and BFS manipulation

:closed_book: [892 · Alien Dictionary](https://www.lintcode.com/problem/892/): Topological sorting with adjacency, in degree, and BFS with heapify manipulation

---

## Chapter 15 (LIVE): Divide-and-Conquer for Binary Trees

:green_book: [596 · Minimum Subtree](https://www.lintcode.com/problem/596/): Similar to [628 · Maximum Subtree](https://www.lintcode.com/problem/628/) but using global variables

:green_book: [474 · Lowest Common Ancestor II](https://www.lintcode.com/problem/474/): LCA but with parent pointer

:orange_book: [88 · Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/88/): LCA but regular without parent pointer

:orange_book: [578 · Lowest Common Ancestor III](https://www.lintcode.com/problem/578/): LCA but does not guarantee that A, or B, or LCA exists

:green_book: [453 · Flatten Binary Tree to Linked List](https://www.lintcode.com/problem/453/): Break up to left and right tree and flatten and connect using DFS

:orange_book: [902 · Kth Smallest Element in a BST](https://www.lintcode.com/problem/902/description): In-order traversal on BST and find k-th traversed element

---

## Chapter 16: DFS for Combination

:orange_book: [17 · Subsets](https://www.lintcode.com/problem/17/): DFS for combination + iterating each possible subset length

:orange_book: [18 · Subsets II](https://www.lintcode.com/problem/18): DFS for combination + iterating each possible subset length + duplicate removal

---

## Chapter 17: DFS for Permutation

:orange_book: [15 · Permutations](https://www.lintcode.com/problem/15/): DFS for permutation

:orange_book: [16 · Permutations II](https://www.lintcode.com/problem/16/): DFS for permutation + duplicate removal

:closed_book: [816 · Traveling Salesman Problem](https://www.lintcode.com/problem/816/): DFS for permutation + graph building


---

## Chapter 18: Hash Table

:green_book: [128 · Hash Function](https://www.lintcode.com/problem/128/): Basic hashing

:orange_book: [129 · Rehashing](https://www.lintcode.com/problem/129/): Re-calculating hash index and re-inserting all elements

---

## Chapter 19: Heap

:orange_book: [130 · Heapify](https://www.lintcode.com/problem/130/): Using sift-down method

---

## Chapter 20: TODO

---

## Chapter 21: TODO

---

## Chapter 22: Memoization Search

:orange_book: [109 · Triangle](https://www.lintcode.com/problem/109/): DFS but using memoization with HashMap to save intermediate results

:green_book: [1300 · Bash Game](https://www.lintcode.com/problem/1300/): DP will timeout, so using a clever method in O(1)

---

## Chapter 23: Introduction to the concept of Dynamic Programming

:orange_book: [109 · Triangle](https://www.lintcode.com/problem/109/): Using DP -> dp[i][j] = cost to go from (0, 0) to this position (i, j)

:green_book: [114 · Unique Paths](https://www.lintcode.com/problem/114/): Using DP -> dp[i][j] = # of unique paths to reach (i, j) from (0, 0)

---

## Chapter 24: Dynamic Programming Typical Problems

:green_book: [115 · Unique Paths II](https://www.lintcode.com/problem/115): Problem 114 but with obstacles, similar DP

:orange_book: [630 · Knight Shortest Path II](https://www.lintcode.com/problem/630/): Using DP -> dp[i][j] = minimum steps to reach (i, j) from (0, 0)

:orange_book: [116 · Jump Game](https://www.lintcode.com/problem/116/): Using DP -> dp[i] = whether we can reach position [i]

---

## Chapter 25: Dynamic Programming Typical Problems

:orange_book: [92 · Backpack](https://www.lintcode.com/problem/92/): Two DP methods

:orange_book: [125 · Backpack II](https://www.lintcode.com/problem/125/): Problem 92 but with values for each item

:orange_book: [440 · Backpack III](https://www.lintcode.com/problem/440): Problem 92 but we can pick any number of the same item
