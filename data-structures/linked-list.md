# Linked Lists
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [List](https://leetcode.com/tag/linked-list)

---

## - Cycle Detection and Finding (with Slow / Fast Pointers)
- About **Slow / Fast Pointers for Cycle Finding**: (see LeetCode Problem 141, 142)
    - [A Proof](https://drive.google.com/file/d/1ypA196eeOnzWUTQGtOh5WpedPdM3FFDd/view)
    - [A Visual Explanation](https://leetcode.com/problems/linked-list-cycle-ii/discuss/1701128/C%2B%2BJavaPython-Slow-and-Fast-oror-Image-Explanation-oror-Beginner-Friendly)

![slow_fast_cycle](https://assets.leetcode.com/users/images/eb4e7e41-f0a8-4648-b145-23a9764fcd57_1642561451.2184958.png)

```
# Cycle Detection (return the begining of the cyle) - see LeetCode Problem 142

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

:heavy_check_mark: :green_book: [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/): (either using **slow/fast pointers** or a clever way of node deletion)

:heavy_check_mark: :orange_book: [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/): (using **slow/fast pointers**)

---

## - Miscellaneous Problems with Slow / Fast Pointers

- About **Slow / Fast Pointers for Finding Mid-node of Linked List**: (see LeetCode Problem 234)

```
# initially slow and fast are the same, starting from head
# note that if we set [fast = head.next] INSTEAD, then when while stops, slow will be on LAST node of FIRST half
slow = fast = head

while fast and fast.next:
    # fast traverses faster and moves to the end of the list if the length is odd
    fast = fast.next.next
    slow = slow.next

# when fast reaches the end or past the end, the slow will reach the middle or past the middle
if fast:
    # fast is exactly at the end, move slow one step further for comparison (cross middle one)
    slow = slow.next
    
# now slow should be at the STARTING node of the SECOND half of the list
```

- About **Slow / Fast Pointers for Fiding & Removing N-th Node from End of List**: (see LeetCode 19)

```
fast = slow = head
        
for _ in range(n):
    fast = fast.next # Let fast move n-nodes ahead of slow

# If removing first element of list (n = number of nodes)
if not fast:
    return head.next

# fast and slow will have a distance of n nodes
# so when fast reaches the end node, slow reaches the node before the [n-th node before the end node]
# which is very quite clever

while fast.next:
    fast = fast.next
    slow = slow.next

slow.next = slow.next.next # remove the node after slow

return head # return the modified list
```

:heavy_check_mark: :green_book: [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/): (**Slow/Fast Pointers to divide list & reversing linked list**)

:heavy_check_mark: :green_book: [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/): (**Slow/Fast Pointers for cycle detection & concatenating lists**)

:heavy_check_mark: :orange_book: [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/): (**Slow/Fast Pointers for finding N-th node from end of list**)

:heavy_check_mark: :orange_book: [148. Sort List](https://leetcode.com/problems/sort-list/): (using **Merge Sort & Slow/Fast Pointers for dividing the list**)

---
## - Miscellaneous Problems with Dummy Nodes & Double Pointers & Etc.

:heavy_check_mark: :orange_book: [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/): (**Dummy Nodes** with a super helpful **visual guide**)

:heavy_check_mark: :green_book: [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/): (**Dummy Nodes**)

:heavy_check_mark: :green_book: [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

:heavy_check_mark: :orange_book: [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/): (**Double Pointers**, one for odd elements and one for even elements)

:wavy_dash: :orange_book: [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/): (**Double Pointers**, one for each linked list)

:wavy_dash: :green_book: [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/): A very down-voted trick question, BUT it seems to be a popular interview question

:wavy_dash: :orange_book: [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/): (**Dictionary** to create the copy)

:wavy_dash: :closed_book: [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/): (**DFS - Preorder Traversal** to serialize the binary tree)
