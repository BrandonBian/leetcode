# Linked Lists | [[Selected Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/linked-list.md)]
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


---
## - Miscellaneous Problems with Dummy Nodes & Double Pointers & Etc.

See selected problem list.
