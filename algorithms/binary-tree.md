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
def maxDepth_bottomUp(root):
    
    # stop conditions    
    if not root:
        return 0
    
    left_depth, right_depth = 0, 0
    
    # assuming maxDepth works perfectly
    if root.left is not None:
        left_depth = maxDepth_bottomUp(root.left)
        
    if root.right is not None:
        right_depth = maxDepth_bottomUp(root.right)
    
    # go up one layer (+ 1 account for going up one layer)
    return 1 + max(left_depth, right_depth)

###############################################################

def maxDepth_topDown(root, current_level, max_level):
    # stop conditions    
    if not root:
        return 0
    
    if current_level > max_level[0]:
        max_level[0] = current_level
        
    maxDepth_topDown(root.left, current_level + 1, max_level)
    maxDepth_topDown(root.right, current_level + 1, max_level)
```

- About **General Ideas of Recursion**:
```
TODO
```

## - LeetCode Problems

### - Basic Traversal Problems

:heavy_check_mark: :green_book: [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/): (DFS, Recursion)

:heavy_check_mark: :green_book: [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/): (DFS, Recursion)

:heavy_check_mark: :green_book: [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/): (DFS, Recursion)

:heavy_check_mark: :orange_book: [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/): (NOT using recursion, BFS & Queue instead)

:wavy_dash: :orange_book: [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/): a variation of level-order traversal (BFS & Queue with altenating insertion directions)

:wavy_dash: :orange_book: [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/): a variation of level-order traversal (just reversing the resulting list)

---

### - Basic Binary Tree Operations

:heavy_check_mark: :green_book: [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/): (using **bottom-up & top-down** recursion)

:heavy_check_mark: :green_book: [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

:wavy_dash: :green_book: [100. Same Tree](https://leetcode.com/problems/same-tree/)

:wavy_dash: :green_book: [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

:wavy_dash: :green_book: [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

:wavy_dash: :green_book: [563. Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt/)

:wavy_dash: :green_book: [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

---

### - Binary Search Trees (BSTs)
- **In-order Traversal**
```
result = []

def inorder(node):
    if node:
        inorder(node.left)
        # do something to your node here
        if node.val >= low and node.val <= high:
            result.append(node.val)
        inorder(node.right)
    
# result will have node values ordered in ascending order
```
- **Deleting a Node from BST**
```
def deleteNode(self, root, key):
    
    # Reference Solution:
    # https://leetcode.com/problems/delete-node-in-a-bst/discuss/821420/Python-O(h)-solution-explained
    
    if not root:
        return None

    if root.val == key:
        if not root.right: return root.left
        if not root.left: return root.right
            
        if root.left and root.right:
                
            # go right one step and go to the left most node, replace current node value with that
            temp = root.right
            while temp.left: temp = temp.left
                    
            root.val = temp.val
            root.right = self.deleteNode(root.right, root.val) # delete the replaced value from right tree
                
    elif root.val > key:
        root.left = self.deleteNode(root.left, key)
    else:
        root.right = self.deleteNode(root.right, key)
            
    return root

```
- **Inserting a Node to BST**

```
def insertIntoBST(self, root, val):
        """
        :type root: TreeNode
        :type val: int
        :rtype: TreeNode
        """
        
        # Reference Solution:
        # https://leetcode.com/problems/insert-into-a-binary-search-tree/discuss/180244/Python-4-line-clean-recursive-solution
        
        if not root:
            return TreeNode(val)
        
        if root.val < val:
            root.right = self.insertIntoBST(root.right, val)
        else:
            root.left = self.insertIntoBST(root.left, val)
            
        return root
```

:heavy_check_mark: :green_book: [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

:heavy_check_mark: :green_book: [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

:heavy_check_mark: :green_book: [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/): (using **in-order traversal** to traverse in ascending order)

:heavy_check_mark: :orange_book: [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

:heavy_check_mark: :orange_book: [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

:heavy_check_mark: :orange_book: [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

:wavy_dash: :green_book: [938. Range Sum of BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/): (using **in-order traversal** to traverse in ascending order, rather trivial)

:wavy_dash: :orange_book: [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/): (using **in-order traversal** to traverse in ascending order)

:wavy_dash: :orange_book: [1305. All Elements in Two Binary Search Trees](https://leetcode.com/problems/all-elements-in-two-binary-search-trees/): (using **in-order traversal** to traverse in ascending order, rather trivial)

<!-- 
[95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/): (Advanced version of 102, quite complicated)
 -->
