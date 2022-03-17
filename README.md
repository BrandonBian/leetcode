# LeetCode-Notes
Here I will record all the useful information that I learned or gained from praticing LeetCode problems

## Functions and Methods
### Binary Tree Traversals (Pre/Post/In Order, BFS, DFS)

![tree](https://assets.leetcode.com/users/andvary/image_1556551007.png)
```
def preorder(root):
  return [root.val] + preorder(root.left) + preorder(root.right) if root else []
  
def inorder(root):
  return  inorder(root.left) + [root.val] + inorder(root.right) if root else []
  
def postorder(root):
  return  postorder(root.left) + postorder(root.right) + [root.val] if root else []
```
[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/): check if tree is symmetric (mirrored)

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)


### Dynamic Programming

[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/): dynamic programming to find number of ways to climb to a stair

[139. Word Break](https://leetcode.com/problems/word-break/): check if word can be broken down to elements in a word dictionary

### Binary Search

[35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): binary search on list index

### Kadane's Algorithm (Maximum Subarray)

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/): find subarray whose sum is maximized

## Data Structures
### Dictionary
[136. Single Number](https://leetcode.com/problems/single-number/): find the number in array that only appeared once (enumerating dict.items())


### Linked List

**Cycle finding using slow & faster pointers**: [Proof](https://drive.google.com/file/d/1ypA196eeOnzWUTQGtOh5WpedPdM3FFDd/view)

**Cycle Detection**: [Ref](https://leetcode.com/problems/linked-list-cycle-ii/discuss/1701128/C%2B%2BJavaPython-Slow-and-Fast-oror-Image-Explanation-oror-Beginner-Friendly) with proof.

```
class Solution(object):
    def detectCycle(self, head):
        slow = fast = head
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
            if slow == fast: break
        else: return None  # if not (fast and fast.next): return None
        while head != slow:
            head, slow = head.next, slow.next
        return head
```

[141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/): find if cycle present in linked list (using walker/slow and runner/fast pointers)

[142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/): find if cycle present, if so return the node at which the cyle begins

[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/): find the node at which two linked lists intersect (two pointers OR concatenate lists and find cycle)

### Stack
[155. Min Stack](https://leetcode.com/problems/min-stack/): design a stack that supports push, pop, top, and getMin
