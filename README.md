# LeetCode-Notes
Here I will record all the useful information that I learned or gained from praticing LeetCode problems

Note: :heavy_check_mark: means **very important, typical, or good examples** that should definitely be familiar with

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
:heavy_check_mark: [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/): check if tree is symmetric (mirrored)

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

---
### Dynamic Programming

[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/): dynamic programming to find number of ways to climb to a stair

[139. Word Break](https://leetcode.com/problems/word-break/): check if word can be broken down to elements in a word dictionary

---
### Binary Search

[35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): binary search on list index

---
### Kadane's Algorithm (Maximum Subarray)
```
# The thought follows a simple rule:
# If the sum of a subarray is positive, it has possible to make the next value bigger, so we keep do it until it turn to negative.
# If the sum is negative, it has no use to the next element, so we break.
# it is a game of sum, not the elements.

Initialize:
    max_so_far = INT_MIN
    max_ending_here = 0

Loop for each element of the array
  (a) max_ending_here = max_ending_here + a[i]
  (b) if(max_so_far < max_ending_here)
            max_so_far = max_ending_here
  (c) if(max_ending_here < 0)
            max_ending_here = 0
return max_so_far
```

:heavy_check_mark: [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/): find subarray whose sum is maximized

---
### Bit Operations (Including Kernighan's Algorithm - which is a DP algorithm)
About Python **Bitwise Operators**: [link](https://wiki.python.org/moin/BitwiseOperators) (<<, >>, &, |, ~, and ^)

About **Kernighan's Algorithm**: [link](https://iq.opengenus.org/brian-kernighan-algorithm/)

```
def Kenighan_algo(n):
    cnt = 0
    while n != 0 :
        cnt += 1 # Each iteration will count one 1 digit in n
        n = n & n-1 # unsets the right most set bit
           
    return cnt

# Patterns:
# i= 1 ( 1 &  0 =  0)     1 &     0 =     0  n_ones =  1
# i= 2 ( 2 &  1 =  0)    10 &     1 =     0  n_ones =  1
# i= 3 ( 3 &  2 =  2)    11 &    10 =    10  n_ones =  2
# i= 4 ( 4 &  3 =  0)   100 &    11 =     0  n_ones =  1
# i= 5 ( 5 &  4 =  4)   101 &   100 =   100  n_ones =  2
# i= 6 ( 6 &  5 =  4)   110 &   101 =   100  n_ones =  2
# i= 7 ( 7 &  6 =  6)   111 &   110 =   110  n_ones =  3
# i= 8 ( 8 &  7 =  0)  1000 &   111 =     0  n_ones =  1
# i= 9 ( 9 &  8 =  8)  1001 &  1000 =  1000  n_ones =  2
# i=10 (10 &  9 =  8)  1010 &  1001 =  1000  n_ones =  2
```

[191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/): count number of 1 in a bit string

:heavy_check_mark: [338. Counting Bits](https://leetcode.com/problems/counting-bits/): count number of 1 for binary representation of integers up to a limit (dynamic programming)

---
## Data Structures
### Dictionary
[136. Single Number](https://leetcode.com/problems/single-number/): find the number in array that only appeared once (enumerating dict.items())

---
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

:heavy_check_mark: [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/): find if cycle present, if so return the node at which the cyle begins

[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/): find the node at which two linked lists intersect (two pointers OR concatenate lists and find cycle)

---
### Stack
[155. Min Stack](https://leetcode.com/problems/min-stack/): design a stack that supports push, pop, top, and getMin
