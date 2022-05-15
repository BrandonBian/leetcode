# LeetCode - Binary Tree & Binary Search Tree | Problems
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---

## Binary Tree Problems

### 1. Basic Traversal Problems

:heavy_check_mark: :green_book: [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/): (**DFS & Recursion**)

```
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        
        # In-order: left, middle, right
        # Post-order: left, right, middle
        # Pre-order: middle, left, right
        
        # Special cases
        if root is None:
            return []
        
        return self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
```

---

:heavy_check_mark: :green_book: [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/): (**DFS & Recursion**)

```
class Solution(object):

    def preorder(self, root):
        return [root.val] + self.preorder(root.left) + self.preorder(root.right) if root else []

    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        
        # Pre-order: node -> left -> right
        
        if not root:
            return None
        if not root.left and not root.right:
            return [root.val]
        
        result = self.preorder(root)
        
        return result
```

---

:heavy_check_mark: :green_book: [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/): (**DFS & Recursion**)

```
class Solution(object):
    
    def postorder(self, root):
        if not root:
            return []
        else:
            return self.postorder(root.left) + self.postorder(root.right) + [root.val]
    
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        
        # Post-order: left -> right -> node
        
        if not root:
            return None
        if not root.left and not root.right:
            return [root.val]
        
        result = self.postorder(root)
        
        return result
```

---

:heavy_check_mark: :orange_book: [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/): (NOT using recursion, **BFS & Queue** instead)

```
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
            
        # Solution using BFS and Queues:
        # https://leetcode.com/problems/binary-tree-level-order-traversal/discuss/1219538/Python-Simple-bfs-explained
        
        if not root:
            return []
        
        if not root.left and not root.right:
            return [[root.val]]
        
        from collections import deque
        
        result = []
        queue = deque([root])
        
        while queue:
            
            # store the values of all nodes in this level
            result.append([node.val for node in queue])
            
            for _ in range(len(queue)): # for each node in queue
                
                node = queue.popleft()

                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            
        return result
```

---

:wavy_dash: :orange_book: [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/): a variation of level-order traversal (**BFS & Queue** with **alternating insertion directions**)

```
class Solution(object):
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        
        # Reference: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/discuss/749036/Python-Clean-BFS-solution-explained
        
        if not root:
            return None
        
        if not root.left and not root.right:
            return [[root.val]]
        
        from collections import deque
        
        queue, result = deque([root]), []
        direction = 1 
        
        while queue:
            
            level = [node.val for node in queue]
            result.append(level[::direction]) # appending in direction (1 is left->right; -1 is right->left)
            direction *= -1
            
            for i in range(len(queue)):
                node = queue.popleft()

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
        return result
```

---

:wavy_dash: :orange_book: [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/): a variation of level-order traversal of 102, just reversing the resulting list

```
class Solution(object):
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        
        if not root:
            return None
        
        if not root.left and not root.right:
            return [[root.val]]
        
        from collections import deque
        
        queue = deque([root])
        result = []
        
        while queue:
            
            result.append([node.val for node in queue])
            
            for _ in range(len(queue)):
                
                node = queue.popleft()
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

        return result[::-1]
```

---

### 2. Basic Binary Tree Operations

:heavy_check_mark: :green_book: [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/): (using **bottom-up & top-down & Recursion**)

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

#####################################################################

def maxDepth_topDown(root, current_level, max_level):
    # stop conditions    
    if not root:
        return 0
    
    max_level[0] = max(max_level[0], current_level)
        
    maxDepth_topDown(root.left, current_level + 1, max_level)
    maxDepth_topDown(root.right, current_level + 1, max_level)

#####################################################################

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        if root is None:
            return 0

        # return maxDepth_bottomUp(root)
        max_level = [0]
        maxDepth_topDown(root, 1, max_level)

        return max_level[0]
```

---

:heavy_check_mark: :green_book: [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

```
class Solution(object):
    def depth(self, root, diameter): # max number of edges from this node to a leaf
        
        # stopping condition
        if not root:
            return 0
        
        # assuming depth working perfectly
        left_height = self.depth(root.left, diameter) # max number of edges from this node to a leaf in the left subtree
        right_height = self.depth(root.right, diameter) # max number of edges from this node to a leaf in the right subtree
        diameter[0] = max(diameter[0], left_height + right_height)
        
#         print("Node Value: ", root.val)
#         print("Left Height: ", left_height)
#         print("Right Height: ", right_height)
#         print('')
        
        # assuming left_height, right_height calculated as requested - go up one layer
        return 1 + max(left_height, right_height) # The +1 to account for the edge between the root and the subtree

    def diameterOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        # Reference solution:
        # https://leetcode.com/problems/diameter-of-binary-tree/discuss/101145/Simple-Python/175847
        
        diameter = [0]
        
        self.depth(root, diameter)
        
        return diameter[0]
```

---

:wavy_dash: :green_book: [100. Same Tree](https://leetcode.com/problems/same-tree/)

```
class Solution(object):

    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        
        # stopping conditions
        if not p and not q:
            return True
        if not p or not q:
            return False
                
        left_same = self.isSameTree(p.left, q.left)
        right_same = self.isSameTree(p.right, q.right)
        
        return p.val == q.val and left_same and right_same
```

---

:wavy_dash: :green_book: [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

```
def isMirror(left, right):
    # stop conditions
    if left is None and right is None:
        return True
    if left is None or right is None:
        return False
    
    if left.val == right.val:
        # assume isMirror is perfect
        outPair = isMirror(left.left, right.right)
        inPair = isMirror(left.right, right.left)
        return outPair and inPair
    else:
        return False
    
    
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        
        # According to solution
        # https://leetcode.com/problems/symmetric-tree/discuss/33050/Recursively-and-iteratively-solution-in-Python
        if root is None:
            return True
        else:
            return isMirror(root.left, root.right)
```

---

:wavy_dash: :green_book: [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

```
class Solution(object):
    def mergeTrees(self, root1, root2):
        """
        :type root1: TreeNode
        :type root2: TreeNode
        :rtype: TreeNode
        """
        
        # stop conditions
        if root1 is None and root2 is None:
            return None
        if root1 is None:
            return root2
        if root2 is None:
            return root1
        
        # assuming left and right trees are merged trees - go up one layer
        root = TreeNode(root1.val + root2.val)
        root.left = self.mergeTrees(root1.left, root2.left)
        root.right = self.mergeTrees(root1.right, root2.right)
        
        return root
```

---

:wavy_dash: :green_book: [563. Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt/)

```
class Solution(object):

    def sum(self, node, answer):
        # calculate the sum of all nodes below this node
        
        if not node:
            return 0
        
        left_sum = self.sum(node.left, answer)
        right_sum = self.sum(node.right, answer)
        
        answer[0] += abs(left_sum - right_sum)
        
        return node.val + left_sum + right_sum

    def findTilt(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        answer = [0]
        self.sum(root, answer)
        
        return answer[0]
```

---

:wavy_dash: :green_book: [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

```
class Solution(object):
    def invertTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        
        # stop condition
        if not root:
            return None
        
        if not root.left and not root.right:
            return root
        
        # assuming invertTree works perfectly
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        
        # assuming left and right both inverted as required - go up one layer
        root.left = right
        root.right = left
        
        return root
```

---

:wavy_dash: :orange_book: [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/): (**level-order traversal**)

```
class Solution(object):
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        
        if not root:
            return None
        
        if not root.left and not root.right:
            return [root.val]
        
        from collections import deque
        
        result = []
        queue = deque([root])
        
        while queue:
            
            level = [node.val for node in queue]
            result.append(level[-1]) # only want the rightmost element of each level
            
            for _ in range(len(queue)): # for each node in queue
                
                node = queue.popleft()

                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            
        return result
```

---

## Binary Search Tree Problems

### 1. Basic BST Operations

:heavy_check_mark: :green_book: [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

:heavy_check_mark: :green_book: [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

:heavy_check_mark: :green_book: [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/): (using **in-order traversal** to traverse in ascending order)

:heavy_check_mark: :orange_book: [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/): (**deleting node** from BST)

:heavy_check_mark: :orange_book: [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

:heavy_check_mark: :orange_book: [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/): (**inserting node** to BST)

:wavy_dash: :green_book: [938. Range Sum of BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/): (using **in-order traversal** to traverse in ascending order, rather trivial)

:wavy_dash: :orange_book: [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/): (using **in-order traversal** to traverse in ascending order)

:wavy_dash: :orange_book: [1305. All Elements in Two Binary Search Trees](https://leetcode.com/problems/all-elements-in-two-binary-search-trees/): (using **in-order traversal** to traverse in ascending order, rather trivial)

---

### 2. BST Path Problems

:heavy_check_mark: :green_book: [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/): (**Standard Root-to-Leaf Paths Traversal**)

:heavy_check_mark: :green_book: [112. Path Sum](https://leetcode.com/problems/path-sum/): (**Standard Root-to-Leaf Paths Traversal**)

:heavy_check_mark: :orange_book: [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/): (**Traversal: paths must go downwards, Using 2 DFS**)

:heavy_check_mark: :closed_book: [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/): (**Traversal: paths do not need to pass root**)

:wavy_dash: :orange_book: [113. Path Sum II](https://leetcode.com/problems/path-sum/): a variation of 112 (**Standard Root-to-Leaf Paths Traversal**)

:wavy_dash: :orange_book: [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/): a variation of 112 (**Standard Root-to-Leaf Paths Traversal**)

:wavy_dash: :orange_book: [1457. Pseudo-Palindromic Paths in a Binary Tree](https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/): (**Standard Root-to-Leaf Paths Traversal + Palindrom checking**)

---

### 3. Construct Binary Trees

:heavy_check_mark: :orange_book: [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

:heavy_check_mark: :orange_book: [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

:heavy_check_mark: :orange_book: [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

:heavy_check_mark: :green_book: [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

:wavy_dash: :orange_book: [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

---

### 4. Validate Binary Trees

:heavy_check_mark: :orange_book: [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

:wavy_dash: :orange_book: [1361. Validate Binary Tree Nodes](https://leetcode.com/problems/validate-binary-tree-nodes/)

---

### 5. Lowest Common Ancestor (LCA) Problems

:heavy_check_mark: :orange_book: [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

:wavy_dash: :green_book: [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/submissions/): basically the same as Problem 236

---

### 6. Miscellaneous Problems


:heavy_check_mark: :orange_book: [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/): here is a very smart solution (using **pre-order** traversal)

:wavy_dash: :orange_book: [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

:wavy_dash: :orange_book: [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/): (using **BFS**)

:wavy_dash: :orange_book: [958. Check Completeness of a Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree/): (using **BFS, Level-order Traversal, Queue**)

:wavy_dash: :green_book: [993. Cousins in Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree/): (my solution using **BFS, Level-order Traversal, Queue**; reference solution using **DFS**)

:wavy_dash: :orange_book: [1026. Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/): (keeping track of the max and min value of each subtree)

:wavy_dash: :orange_book: [1530. Number of Good Leaf Nodes Pairs](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/): (using **DFS, Recursion**)

:wavy_dash: :orange_book: [865. Smallest Subtree with all the Deepest Nodes](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/): (my solution using **finding positions of nodes & Lowest Common Ancestor**)

:wavy_dash: :orange_book: [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/): (creating a **Node-Parent Map & running DFS**)

:wavy_dash: :orange_book: [1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree](https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/): (using **Paired DFS / BFS**, if you consider the harder version / follow-up of this problem)

---
