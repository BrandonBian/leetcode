# Binary Trees & BSTs · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Basic Traversals

<a id="lc-94"></a>
:heavy_check_mark: :green_book: [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/): (**DFS & Recursion**)

```python
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

<a id="lc-144"></a>
:heavy_check_mark: :green_book: [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/): (**DFS & Recursion**)

```python
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
            return []
        if not root.left and not root.right:
            return [root.val]

        result = self.preorder(root)

        return result
```

---

<a id="lc-145"></a>
:heavy_check_mark: :green_book: [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/): (**DFS & Recursion**)

```python
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
            return []
        if not root.left and not root.right:
            return [root.val]

        result = self.postorder(root)

        return result
```

---

<a id="lc-102"></a>
:heavy_check_mark: :orange_book: [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/): (NOT using recursion, **BFS & Queue** instead)

```python
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

<a id="lc-103"></a>
:wavy_dash: :orange_book: [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/): a variation of level-order traversal (**BFS & Queue** with **alternating insertion directions**)

```python
class Solution(object):
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """

        # Reference: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/discuss/749036/Python-Clean-BFS-solution-explained

        if not root:
            return []

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

<a id="lc-107"></a>
:wavy_dash: :orange_book: [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/): a variation of level-order traversal of 102, just reversing the resulting list

```python
class Solution(object):
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """

        if not root:
            return []

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

## 2. Basic Binary Tree Operations

<a id="lc-104"></a>
:heavy_check_mark: :green_book: [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/): (using **bottom-up & top-down & Recursion**)

```python
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

<a id="lc-543"></a>
:heavy_check_mark: :green_book: [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

```python
class Solution(object):
    def depth(self, root, diameter): # max number of edges from this node to a leaf

        # stopping condition
        if not root:
            return 0

        # assuming depth working perfectly
        left_height = self.depth(root.left, diameter) # max number of edges from this node to a leaf in the left subtree
        right_height = self.depth(root.right, diameter) # max number of edges from this node to a leaf in the right subtree
        diameter[0] = max(diameter[0], left_height + right_height)

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

<a id="lc-100"></a>
:wavy_dash: :green_book: [100. Same Tree](https://leetcode.com/problems/same-tree/)

```python
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

<a id="lc-101"></a>
:wavy_dash: :green_book: [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

```python
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

<a id="lc-617"></a>
:wavy_dash: :green_book: [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

```python
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

<a id="lc-563"></a>
:wavy_dash: :green_book: [563. Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt/)

```python
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

<a id="lc-226"></a>
:wavy_dash: :green_book: [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

```python
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

<a id="lc-199"></a>
:wavy_dash: :orange_book: [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/): (**level-order traversal**)

```python
class Solution(object):
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """

        if not root:
            return []

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

## 3. Basic BST Operations

<a id="lc-700"></a>
:heavy_check_mark: :green_book: [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

```python
class Solution(object):

    def searchBST(self, root, val):
        """
        :type root: TreeNode
        :type val: int
        :rtype: TreeNode
        """
        if not root:
            return None

        if root.val == val:
            return root

        result = [None]
        self.inorder(root, val, result)

        return result[0]

    def inorder(self, node, val, result):

        if node:

            self.inorder(node.left, val, result)

            if node.val == val:
                result[0] = node

            self.inorder(node.right, val, result)
```

---

<a id="lc-653"></a>
:heavy_check_mark: :green_book: [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

```python
class Solution(object):

    # Reference Solution:
    # https://leetcode.com/problems/two-sum-iv-input-is-a-bst/discuss/106134/Simple-Python-O(n)-Solution-with-Explanation
    # This is basically tree traversal, storing node values of tree in a set and then simply finding 2 values in set that add upto target.

    def helper(self, node, elements, k):
        if not node:
            return False

        complement = k - node.val
        if complement in elements:
            return True

        elements.add(node.val)

        return self.helper(node.left, elements, k) or self.helper(node.right, elements, k)

    def findTarget(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: bool
        """

        if not root:
            return False
        if not root.left and not root.right: # only one element
            return False

        elements = set()

        return self.helper(root, elements, k)
```

---

<a id="lc-530"></a>
:heavy_check_mark: :green_book: [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/): (using **in-order traversal** to traverse in ascending order)

```python
class Solution(object):

    def getMinimumDifference(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        previous = [float('inf')]
        minimum = [float('inf')]

        self.inorder(root, minimum, previous)
        return minimum[0]


    def inorder(self, node, minimum, previous):
        if node:
            self.inorder(node.left, minimum, previous)

            minimum[0] = min(minimum[0], abs(node.val - previous[0]))
            previous[0] = node.val

            self.inorder(node.right, minimum, previous)
```

---

<a id="lc-450"></a>
:heavy_check_mark: :orange_book: [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/): (**deleting node** from BST)

```python
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

---

<a id="lc-669"></a>
:heavy_check_mark: :orange_book: [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

```python
class Solution(object):
    def trimBST(self, root, low, high):
        """
        :type root: TreeNode
        :type low: int
        :type high: int
        :rtype: TreeNode
        """

        # Reference Solution:
        # https://leetcode.com/problems/trim-a-binary-search-tree/discuss/107013/clear-python-solution

        if not root:
            return None

        # If the val of current node is smaller than [low], abandon the left sub-tree and trim its right sub-tree
        if low > root.val:
            return self.trimBST(root.right, low, high)

        # If the val of current node is greater than [high], abandon the right sub-tree and trim its left sub-tree
        elif high < root.val:
            return self.trimBST(root.left, low, high)

        # Else, recursively trim its left and right sub-tree and return the root
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)

        return root
```

---

<a id="lc-701"></a>
:heavy_check_mark: :orange_book: [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/): (**inserting node** to BST)

```python
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

---

<a id="lc-938"></a>
:wavy_dash: :green_book: [938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/): (using **in-order traversal** to traverse in ascending order, rather trivial)

```python
class Solution(object):
    def rangeSumBST(self, root, low, high):
        """
        :type root: TreeNode
        :type low: int
        :type high: int
        :rtype: int
        """

        result = []

        def inorder(node):
            if node:
                inorder(node.left)
                if node.val >= low and node.val <= high:
                    result.append(node.val)
                inorder(node.right)

        inorder(root)
        return sum(result)
```

---

<a id="lc-230"></a>
:wavy_dash: :orange_book: [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/): (using **in-order traversal** to traverse in ascending order)

```python
class Solution(object):

    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """

        def inorder(node):
            if node:
                inorder(node.left)
                self.result.append(node.val)
                inorder(node.right)

        self.result = []
        inorder(root)

        # in-order traversal of a BST is already sorted
        return self.result[k-1]
```

---

<a id="lc-1305"></a>
:wavy_dash: :orange_book: [1305. All Elements in Two Binary Search Trees](https://leetcode.com/problems/all-elements-in-two-binary-search-trees/): (using **in-order traversal** to traverse in ascending order, rather trivial)

```python
class Solution(object):
    def getAllElements(self, root1, root2):
        """
        :type root1: TreeNode
        :type root2: TreeNode
        :rtype: List[int]
        """

        self.result = []

        def inorder(node):
            if node:
                inorder(node.left)
                self.result.append(node.val)
                inorder(node.right)

        inorder(root1)
        inorder(root2)

        self.result.sort()

        return self.result
```

---

## 4. Path Problems

<a id="lc-257"></a>
:heavy_check_mark: :green_book: [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/): (**Standard Root-to-Leaf Paths Traversal**)

```python
class Solution(object):

    # Reference Solution:
    # https://leetcode.com/problems/binary-tree-paths/discuss/68272/Python-solutions-(dfs%2Bstack-bfs%2Bqueue-dfs-recursively).

    def dfs(self, node, path, result):
        if not node.left and not node.right:
            result.append(path + str(node.val))
            return

        if node.left:
            self.dfs(node.left, path + str(node.val) + '->', result)
        if node.right:
            self.dfs(node.right, path + str(node.val) + '->', result)

    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """

        if not root:
            return []

        result = []

        self.dfs(root, "", result)

        return result
```

---

<a id="lc-112"></a>
:heavy_check_mark: :green_book: [112. Path Sum](https://leetcode.com/problems/path-sum/): (**Standard Root-to-Leaf Paths Traversal**)

```python
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:

        if not root:
            return False

        self.found = False

        def dfs(node, path, result):
            if not node.left and not node.right:
                result = path + node.val
                if result == targetSum:
                    self.found = True
                    return

            if node.left:
                dfs(node.left, path + node.val, result)
            if node.right:
                dfs(node.right, path + node.val, result)

        dfs(root, 0, 0)
        return self.found
```

---

<a id="lc-437"></a>
:heavy_check_mark: :orange_book: [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/): (**Traversal: paths must go downwards, Using 2 DFS**)

```python
class Solution(object):

    def pathSum(self, root, targetSum):
        """
        :type root: TreeNode
        :type targetSum: int
        :rtype: int
        """

        # Reference Solution:
        # https://leetcode.com/problems/path-sum-iii/discuss/141424/Python-step-by-step-walk-through.-Easy-to-understand.-Two-solutions-comparison.-%3A-)
        # Method: Brute-Force (2 DFS)

        # Idea: first DFS to visit each node of the tree
        # for each visited node, run a DFS on its subtrees to find any paths that sum to target

        def search(node, target): # For a given node, search all its subtrees for paths that sum to target
            if not node:
                return
            if target == node.val:
                self.result += 1
                # return (note that there is no return here)

            if node.left:
                search(node.left, target - node.val)
            if node.right:
                search(node.right, target - node.val)

        def dfs(node): # For each node in tree, call search on it
            if not node:
                return

            search(node, targetSum)
            dfs(node.left)
            dfs(node.right)


        if not root:
            return 0
        if not root.left and not root.right:
            return 1 if root.val == targetSum else 0

        self.result = 0

        dfs(root)

        return self.result
```

- **Alternative: prefix sums + hash map in one DFS, O(n)**

```python
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:

        self.result = 0
        self.cache = {0 : 1} # all the path sum and their frequencies

        def dfs(node, currPathSum, cache):
            if not node:
                return
            currPathSum += node.val
            oldPathSum = currPathSum - targetSum

            self.result += self.cache.get(oldPathSum, 0) # how many different paths we can get
            cache[currPathSum] = self.cache.get(currPathSum, 0) + 1

            dfs(node.left, currPathSum, cache)
            dfs(node.right, currPathSum, cache)

            # when move to a different path, currPathSum no longer available, remove one
            cache[currPathSum] -= 1

        if not root:
            return 0
        if not root.left and not root.right:
            return 1 if root.val == targetSum else 0

        dfs(root, 0, self.cache)

        return self.result
```

---

<a id="lc-124"></a>
:heavy_check_mark: :closed_book: [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/): (**Traversal: paths do not need to pass root**)

```python
class Solution(object):

    def maxPathSum(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.max_path = float("-inf") # placeholder to be updated
        # Reference Solution:
        # https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/603423/Python-Recursion-stack-thinking-process-diagram

        if not root.left and not root.right:
            return root.val

        def get_max_gain(node):

            if node is None:
                return 0

            # What is the max and 0 doing:
            # In case the subtree is all negative, then we should return 0 instead of negative numbers, ignoring that subtree
            gain_on_left = max(get_max_gain(node.left), 0) # Read the part important observations
            gain_on_right = max(get_max_gain(node.right), 0)  # Read the part important observations

            current_max_path = node.val + gain_on_left + gain_on_right # Read first three images of going down the recursion stack
            self.max_path = max(self.max_path, current_max_path) # Read first three images of going down the recursion stack

            return node.val + max(gain_on_left, gain_on_right) # Read the last image of going down the recursion stack

        get_max_gain(root) # Starts the recursion chain
        return(self.max_path)
```

---

<a id="lc-113"></a>
:wavy_dash: :orange_book: [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/): a variation of 112 (**Standard Root-to-Leaf Paths Traversal**)

```python
class Solution(object):

    def dfs(self, node, target, path, result):

        if not node: # went too far
            return
        if target == node.val and not node.left and not node.right: # is a leaf node and path sums up to target
            path += [node.val]
            result.append(path)
            return

        if node.left:
            self.dfs(node.left, target - node.val, path + [node.val], result)
        if node.right:
            self.dfs(node.right, target - node.val, path + [node.val], result)



    def pathSum(self, root, targetSum):
        """
        :type root: TreeNode
        :type targetSum: int
        :rtype: List[List[int]]
        """

        if not root:
            return []

        result = []

        self.dfs(root, targetSum, [], result)

        return result
```

---

<a id="lc-129"></a>
:wavy_dash: :orange_book: [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/): a variation of 112 (**Standard Root-to-Leaf Paths Traversal**)

```python
class Solution(object):

    def dfs(self, node, path, result):

        if not node.left and not node.right: # arrived at a leaf
            path += str(node.val)
            result.append(path)
            return

        if node.left:
            self.dfs(node.left, path + str(node.val), result)
        if node.right:
            self.dfs(node.right, path + str(node.val), result)

    def sumNumbers(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        if not root.left and not root.right:
            return root.val

        result = []
        summation = 0

        self.dfs(root, "", result)

        for num in result:
            summation += int(num)

        return summation
```

---

<a id="lc-1457"></a>
:wavy_dash: :orange_book: [1457. Pseudo-Palindromic Paths in a Binary Tree](https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/): (**Standard Root-to-Leaf Paths Traversal + Palindrom checking**)

```python
class Solution(object):

    def pseudoPalindromicPaths(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        self.paths = []
        self.frequency = [0] * 10
        self.result = 0

        def dfs(node, path):
            if not node: # went too far
                return
            if not node.left and not node.right: # at a leaf node
                path += [node.val]
                # up to this point we obtained the root-to-leaf path, check for palindrome
                # palindrom if at most one odd occurrence
                for element in path:
                    self.frequency[element] += 1
                if sum(c % 2 == 1 for c in self.frequency) < 2:
                    self.result += 1
                self.frequency = [0] * 10
                return
            if node.left:
                dfs(node.left, path + [node.val])
            if node.right:
                dfs(node.right, path + [node.val])

        dfs(root, [])

        return self.result
```

---

## 5. Construct Binary Trees

<a id="lc-105"></a>
:heavy_check_mark: :orange_book: [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```python
if len(inorder) == 0:
    return None

if len(inorder) == 1: # only one element, construct a node for it
    return TreeNode(preorder[0])

rval = preorder[0] # Obtain nodes from preorder - from LEFT to RIGHT
root = TreeNode(rval)

# Looking at preorder traversal, the first value (node 1) must be the root.
# Then, we find the index of root within in-order traversal, and split into two sub problems.
inorder_rval_index = inorder.index(rval)

left_inorder = inorder[:inorder_rval_index]
right_inorder = inorder[inorder_rval_index+1:]
left_preorder = preorder[1: len(left_inorder) + 1]
right_preorder = preorder[1 + len(left_preorder):]

root.left = self.buildTree(left_preorder, left_inorder)
root.right = self.buildTree(right_preorder, right_inorder)

return root
```

---

<a id="lc-106"></a>
:heavy_check_mark: :orange_book: [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```python
if len(inorder) == 0:
        return None
if len(inorder) == 1:
    return TreeNode(inorder[0])

rval = postorder[-1] # Obtain nodes from preorder - from RIGHT to LEFT
root = TreeNode(rval)

inorder_rval_index = inorder.index(rval)

left_inorder = inorder[:inorder_rval_index]
right_inorder = inorder[inorder_rval_index+1:]
left_postorder = postorder[:len(left_inorder)]
right_postorder = postorder[len(left_postorder):-1]

root.left = self.buildTree(left_inorder, left_postorder)
root.right = self.buildTree(right_inorder, right_postorder)

return root
```

---

<a id="lc-889"></a>
:heavy_check_mark: :orange_book: [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

```python
if len(preorder) == 0:
    return None
if len(preorder) == 1:
    return TreeNode(preorder[0])

rval = preorder[0]
root = TreeNode(rval)

val = preorder[1]
postorder_idx = postorder.index(val)

left_preorder = preorder[1:postorder_idx + 2]
right_preorder = preorder[postorder_idx + 2:]

left_postorder = postorder[: postorder_idx + 1]
right_postorder = postorder[postorder_idx + 1:-1]

root.left = self.constructFromPrePost(left_preorder, left_postorder)
root.right = self.constructFromPrePost(right_preorder, right_postorder)

return root
```

---

<a id="lc-108"></a>
:heavy_check_mark: :green_book: [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

```python
class Solution(object):

    def sortedArrayToBST(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """

        # Reference Solution:
        # https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/discuss/35223/An-easy-Python-solution

        if not nums:
            return None

        mid_indx = len(nums) // 2
        root = TreeNode(nums[mid_indx])

        root.left = self.sortedArrayToBST(nums[:mid_indx])
        root.right = self.sortedArrayToBST(nums[mid_indx+1:])

        return root
```

---

<a id="lc-1008"></a>
:wavy_dash: :orange_book: [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

```python
class Solution(object):
    def bstFromPreorder(self, preorder):
        """
        :type preorder: List[int]
        :rtype: TreeNode
        """

        if len(preorder) == 0:
            return None
        if len(preorder) == 1:
            return TreeNode(preorder[0])

        root_val = preorder[0]
        root = TreeNode(root_val)

        left_preorder, right_preorder = [], []
        for i in range(1, len(preorder)):
            if preorder[i] < root_val:
                left_preorder.append(preorder[i])
            else:
                right_preorder.append(preorder[i])

        root.left = self.bstFromPreorder(left_preorder)
        root.right = self.bstFromPreorder(right_preorder)

        return root
```

---

<a id="lc-297"></a>
:wavy_dash: :closed_book: [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/): (**DFS - Preorder Traversal** to serialize the binary tree)

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    # Reference Solution:
    # https://leetcode.com/problems/serialize-and-deserialize-binary-tree/discuss/396124/Python-very-easy-to-understand-recursive-preorder-with-comments
    # Simplification by : https://leetcode.com/problems/serialize-and-deserialize-binary-tree/discuss/396124/Python-very-easy-to-understand-recursive-preorder-with-comments/1099960

    def serialize(self, root):
        """Encodes a tree to a single string.

        :type root: TreeNode
        :rtype: str
        """

        # take care of base cases
        # if a node is empty, add 'x' to string
        # you can set 'x' to any mark as you want

        if not root: return '#'

        # preoder(Root->left->right)
        # ex,
        #     1
        #    / \
        #   2   3
        #      / \
        #     4   5
        #
        # return (1, (2, 'x', 'x'), (3, (4, 'x', 'x'), (5, 'x', 'x')))
        # if you look at the return statement very closely, it is actually very intuitive
        # for value 1, you have 2 as left child and 3 as right child
        # for value 2, you have 'x'(None) as left child and 'x'(None) as right child which indicates it is a leaf node

        def dfs(root):
            if not root:
                return ["#"]
            return [str(root.val)] + dfs(root.left) + dfs(root.right)

        return ",".join(dfs(root))


    def deserialize(self, data):
        """Decodes your encoded data to tree.

        :type data: str
        :rtype: TreeNode
        """

        #######################INTUITION#########################
        # The initial data string will be something like below:
        # (1, (2, 'x', 'x'), (3, (4, 'x', 'x'), (5, 'x', 'x')))
        # if you loop through string:
        # 1                                 -> this is node value
        # (2, 'x', 'x')                     -> this is node left
        # (3, (4, 'x', 'x'), (5, 'x', 'x')) -> this is node right
        ########################################################

        def rdeserialize(queue):
            """ a recursive helper function for deserialization."""
            if queue[0] == '#':
                queue.pop(0)
                return None

            # reconstruct tree node (value, left, right)
            root = TreeNode(queue[0])
            queue.pop(0)
            root.left = rdeserialize(queue)
            root.right = rdeserialize(queue)
            return root

        queue = data.split(',')

        return rdeserialize(queue)


# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```

---

## 6. Validate Binary Trees

<a id="lc-98"></a>
:heavy_check_mark: :orange_book: [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

```python
class Solution(object):

    # Floor: all values must be larger than this
    # Ceiling: all values must be smaller than this
    def isValidBST(self, root, floor=float('-inf'), ceiling=float('inf')):
        """
        :type root: TreeNode
        :rtype: bool
        """

        if not root: # no node is a valid BST
            return True

        if root.val <= floor or root.val >= ceiling:
            return False

        # in the left branch, root is the new ceiling; contrarily root is the new floor in right branch
        return self.isValidBST(root.left, floor, root.val) and self.isValidBST(root.right, root.val, ceiling)
```

---

<a id="lc-1361"></a>
:wavy_dash: :orange_book: [1361. Validate Binary Tree Nodes](https://leetcode.com/problems/validate-binary-tree-nodes/)

```python
class Solution(object):
    def validateBinaryTreeNodes(self, n, leftChild, rightChild):
        """
        :type n: int
        :type leftChild: List[int]
        :type rightChild: List[int]
        :rtype: bool
        """

        # Reference Solution:
        # https://leetcode.com/problems/validate-binary-tree-nodes/discuss/939381/Python:-clean-BFS-96-faster-TimeComplexity:-O(n)-Space-Complexity:-O(n)/794077

        indegree = [0] * n

        # Count the indegrees and outdegrees of each node
        for l, r in zip(leftChild, rightChild):
            if l != -1:
                indegree[l] += 1
                # if there are nodes that has more than 2+ parents, return false.
                if indegree[l] > 1:
                    return False
            if r != -1:
                indegree[r] += 1
                if indegree[r] > 1:
                    return False

        # a valid tree only has 1 root.
        if indegree.count(0) != 1:
            return False

        # count nodes from root, if the total number is not n, it means there are islands, then return false.
        root = indegree.index(0)
        def count_nodes(root):
            if root == -1:
                return 0
            return 1 + count_nodes(leftChild[root]) + count_nodes(rightChild[root])

        return count_nodes(root) == n
```

---

## 7. Lowest Common Ancestor (LCA)

<a id="lc-236"></a>
:heavy_check_mark: :orange_book: [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """

        # Reference Solution:
        # YouTube: https://www.youtube.com/watch?v=py3R23aAPCA

        if not root: return None
        if root == p or root == q: # looking for myself
            return root

        left_LA = self.lowestCommonAncestor(root.left, p, q)
        right_LA = self.lowestCommonAncestor(root.right, p, q)

        if left_LA and right_LA:
            return root
        if left_LA and not right_LA:
            return left_LA
        if right_LA and not left_LA:
            return right_LA
```

---

<a id="lc-235"></a>
:wavy_dash: :green_book: [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/): basically the same as Problem 236

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """

        if not root:
            return None
        if root == p or root == q:
            return root

        left_LA = self.lowestCommonAncestor(root.left, p, q)
        right_LA = self.lowestCommonAncestor(root.right, p, q)

        if not left_LA: return right_LA
        if not right_LA: return left_LA

        return root
```

---

## 8. Miscellaneous

<a id="lc-114"></a>
:heavy_check_mark: :orange_book: [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/): here is a very smart solution (using **pre-order** traversal)

```python
class Solution(object):

    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: None Do not return anything, modify root in-place instead.
        """

        # Reference Solution:
        # https://leetcode.com/problems/flatten-binary-tree-to-linked-list/discuss/1208004/Extremely-Intuitive-O(1)-Space-solution-with-Simple-explanation-Python

        if not root:
            return None

        curr = root

        while curr:
            if curr.left:
                p = curr.left
                while p.right:
                    p = p.right # p is right-most point in left subtree

                p.right = curr.right
                curr.right = curr.left
                curr.left = None
            curr = curr.right

```

---

<a id="lc-222"></a>
:wavy_dash: :orange_book: [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

```python
class Solution(object):
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        if not root:
            return 0
        if not root.left and not root.right:
            return 1

        left_num = self.countNodes(root.left)
        right_num = self.countNodes(root.right)

        return 1 + left_num + right_num
```

---

<a id="lc-662"></a>
:wavy_dash: :orange_book: [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/): (using **BFS**)

```python
class Solution(object):

    def widthOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        # Reference Solution:
        # https://leetcode.com/problems/maximum-width-of-binary-tree/discuss/106707/Python-Straightforward-BFS-and-DFS-solutions

        queue = [(root, 0, 0)]
        cur_depth = left = ans = 0
        for node, depth, pos in queue:
            if node:
                queue.append((node.left, depth+1, pos*2))
                queue.append((node.right, depth+1, pos*2 + 1))
                if cur_depth != depth: # we have reached a new level
                    cur_depth = depth
                    left = pos
                ans = max(pos - left + 1, ans)

        return ans
```

---

<a id="lc-958"></a>
:wavy_dash: :orange_book: [958. Check Completeness of a Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree/): (using **BFS, Level-order Traversal, Queue**)

```python
class Solution:
    def isCompleteTree(self, root: Optional[TreeNode]) -> bool:

        null = False

        from collections import deque
        queue = deque([root])

        # BFS level-order traverse, when we first encounter a NULL node, we see if there is any non-NULL node afterwards
        while queue:
            level = [node.val for node in queue]
            if sum([val == -1 for val in level]) == len(level): # we have reached the last level
                break

            for _ in range(len(queue)):
                node = queue.popleft()

                if node.val == -1:
                    null = True
                else:
                    if null == True:
                        return False

                if node.left:
                    queue.append(node.left)
                else:
                    queue.append(TreeNode(-1))
                if node.right:
                    queue.append(node.right)
                else:
                    queue.append(TreeNode(-1))

        return True
```

---

<a id="lc-993"></a>
:wavy_dash: :green_book: [993. Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree/): (my solution using **BFS, Level-order Traversal, Queue**; reference solution using **DFS**)

---

<a id="lc-1026"></a>
:wavy_dash: :orange_book: [1026. Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/): (keeping track of the max and min value of each subtree)

---

<a id="lc-1530"></a>
:wavy_dash: :orange_book: [1530. Number of Good Leaf Nodes Pairs](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/): (using **DFS, Recursion**)

---

<a id="lc-865"></a>
:wavy_dash: :orange_book: [865. Smallest Subtree with all the Deepest Nodes](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/): (my solution using **finding positions of nodes & Lowest Common Ancestor**)

---

<a id="lc-863"></a>
:wavy_dash: :orange_book: [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/): (creating a **Node-Parent Map & running DFS**)

---

<a id="lc-1379"></a>
:wavy_dash: :orange_book: [1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree](https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/): (using **Paired DFS / BFS**, if you consider the harder version / follow-up of this problem)

---
