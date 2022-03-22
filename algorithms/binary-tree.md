# Binary Trees & Binary Search Trees
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

- **LeetCode Problems**: [List](https://leetcode.com/tag/binary-tree/)

- **Reference 1**: [Study Guide - LeetCode Problems](https://leetcode.com/tag/binary-tree/discuss/1212004/Binary-Trees-study-guide)

- **Reference 2**: [Study Guide - Concepts](https://leetcode.com/tag/binary-tree/discuss/1820334/Become-Master-in-Tree)

## - Binary Tree Traversals and Operations (Usually with Recursion)
- About **Binary Tree Traversals Algorithms**:

![tree](https://assets.leetcode.com/users/andvary/image_1556551007.png)
```
def preorder(root):
  return [root.val] + preorder(root.left) + preorder(root.right) if root else []
  
def inorder(root):
  return  inorder(root.left) + [root.val] + inorder(root.right) if root else []
  
def postorder(root):
  return  postorder(root.left) + postorder(root.right) + [root.val] if root else []
```
- About **Top-down** and **Bottom-up** recursions: [link](https://leetcode.com/explore/learn/card/data-structure-tree/17/solve-problems-recursively/534/)

```
# Top-down maximum_depth(root, depth)

1. return if root is null
2. if root is a leaf node:
3.     answer = max(answer, depth)         // update the answer if needed
4. maximum_depth(root.left, depth + 1)     // call the function recursively for left child
5. maximum_depth(root.right, depth + 1)    // call the function recursively for right child

# Bottom-up maximum_depth(root, depth)

1. return 0 if root is null                 // return 0 for null node
2. left_depth = maximum_depth(root.left)
3. right_depth = maximum_depth(root.right)
4. return max(left_depth, right_depth) + 1  // return depth of the subtree rooted at root
```

- About **General Ideas of Recursion**:
```
1. Define stopping conditions (and return)
2. Assuming the recursion function works perfectly, get results of current stage
3. Going up one layer by considering the current stage's result and updating it accordingly (assuming results from Step-2 perfect)
```

## - LeetCode Problems

### - Basic Traversal Problems

:heavy_check_mark: :green_book: [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/): (DFS, Recursion)

:heavy_check_mark: :green_book: [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/): (DFS, Recursion)

:heavy_check_mark: :green_book: [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/): (DFS, Recursion)

:heavy_check_mark: :orange_book: [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/): (NOT using recursion, BFS & Queue instead)

:wavy_dash: :orange_book: [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/): a variation of level-order traversal (BFS & Queue with altenating insertion directions)

:wavy_dash: :orange_book: [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/): a variation of level-order traversal 

### - Basic Binary Tree Operations
 
:wavy_dash: :green_book:[100. Same Tree](https://leetcode.com/problems/same-tree/)

[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

[617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

:heavy_check_mark: [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)



---

### - Medium



[95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/): (Advanced version of 102, quite complicated)
