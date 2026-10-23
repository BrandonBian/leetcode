# Linked Lists · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Cycle Detection & Finding (Slow / Fast Pointers)

<a id="lc-141"></a>
:heavy_check_mark: :green_book: [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/): (either using **slow/fast pointers** or a clever way of node deletion)

- **Clever way of changing element to None after traversal**

```python
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """

        # Sample Solution: clever way of changing element to None after traversal
        # https://leetcode.com/problems/linked-list-cycle/discuss/44494/Except-ionally-fast-Python/240672

        while head:
            if head.val == None:
                return True
            head.val = None
            head = head.next
        return False
```

- **Using slow/fast pointers (as in Study Guide)**

```python
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """

        if not head:
            return False

        slow, fast = head, head # both starting from head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True

        return False
```

---

<a id="lc-142"></a>
:heavy_check_mark: :orange_book: [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/): (using **slow/fast pointers**)

```python
class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """

        slow = fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                break

        else: # if not (fast and fast.next): return None
            return None

        while head != slow:
            head, slow = head.next, slow.next

        return head
```

---

## 2. More Slow / Fast Pointer Problems

<a id="lc-234"></a>
:heavy_check_mark: :green_book: [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/): (**Slow/Fast Pointers to divide list & reversing linked list**)

```python
class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """

        # Reference Solution:
        # https://leetcode.com/problems/palindrome-linked-list/discuss/64500/11-lines-12-with-restore-O(n)-time-O(1)-space

        # rev records the first half, need to set the same structure as fast, slow, hence later we have rev.next
        rev = None

        # initially slow and fast are the same, starting from head
        slow = fast = head

        while fast and fast.next:
            # fast traverses faster and moves to the end of the list if the length is odd
            fast = fast.next.next

            # take it as a tuple being assigned (rev, rev.next, slow) = (slow, rev, slow.next),
            # hence the re-assignment of slow would not affect rev (rev = slow)

            # "rev, rev.next, slow = slow, rev, slow.next" is equivalent to:
            temp1 = slow
            temp2 = rev
            temp3 = slow.next
            rev = temp1 # current rev is the same as slow
            rev.next = temp2 # next of rev is the previous rev
            slow = temp3 # slow moves one step further

        # When fast reaches the end or past the end, the slow will reach the middle or past the middle
        if fast:
            # fast is exactly at the end, move slow one step further for comparison (cross middle one)
            slow = slow.next

        # compare the reversed first half with the second half
        while rev and rev.val == slow.val:
            slow = slow.next
            rev = rev.next

        # if equivalent then rev become None, return True; otherwise return False
        return not rev
```

---

<a id="lc-160"></a>
:heavy_check_mark: :green_book: [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/): (**Slow/Fast Pointers for cycle detection & concatenating lists**)

```python
class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """

        # Reference Solution:
        # https://leetcode.com/problems/intersection-of-two-linked-lists/discuss/49798/Concise-python-code-with-comments/182352

        # Concatenate list A and list B, if there's an intersection, there's a loop, so we need to find the start of the loop if there's any:

        if not headA or not headB: return None

        # Concatenate A and B
        last = headA
        while last.next: last = last.next
        last.next = headB

        # Find the start of the loop
        fast = slow = headA
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
            if slow == fast: # If there is a loop
                break
        else: # there is no loop -> there is no intersection
            last.next = None # Retain lists' original structures
            return None

        while headA != slow:
            slow, headA = slow.next, headA.next
        last.next = None # Retain lists' original structures

        return headA
```

---

<a id="lc-19"></a>
:heavy_check_mark: :orange_book: [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/): (**Slow/Fast Pointers for finding N-th node from end of list**)

```python
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """

        # Reference Solution:
        # https://leetcode.com/problems/remove-nth-node-from-end-of-list/discuss/8802/3-short-Python-solutions

        fast = slow = head

        for _ in range(n):
            fast = fast.next

        # Removing first element of list (n = number of nodes)
        if not fast:
            return head.next

        # fast and slow will have a distance of n nodes
        # so when fast reaches the end node, slow reaches the n-th node before the end node
        # which is very quite clever

        while fast.next:
            fast = fast.next
            slow = slow.next

        slow.next = slow.next.next # remove slow.next node

        return head
```

---

<a id="lc-148"></a>
:heavy_check_mark: :orange_book: [148. Sort List](https://leetcode.com/problems/sort-list/): Merge sort on linked list

```python
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:

        if not head:
            return None
        if not head.next:
            return head

        slow, fast = head, head.next

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next


        second = slow.next
        slow.next = None

        l = self.sortList(head)
        r = self.sortList(second)

        return self.merge(l, r)

    def merge(self, l, r):
        if not l:
            return r
        if not r:
            return l

        dummy = ListNode(0)
        head = dummy

        while l and r:
            if l.val <= r.val:
                head.next = l
                l = l.next
            else:
                head.next = r
                r = r.next
            head = head.next

        if l:
            head.next = l
        else:
            head.next = r

        return dummy.next
```

---

## 3. Dummy Nodes, Double Pointers & Reversal

<a id="lc-21"></a>
:heavy_check_mark: :green_book: [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/): dummy node + two pointers, attach the smaller node each time

```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        cur = dummy = ListNode()

        while list1 and list2:
            if list1.val < list2.val:
                cur.next = list1
                list1 = list1.next
            else:
                cur.next = list2
                list2 = list2.next

            cur = cur.next

        if list1 or list2: # if there is anything left
            cur.next = list1 if list1 else list2

        return dummy.next

# [Note]:
# For simplicity, we create a dummy node to which we attach nodes from lists.
# We iterate over lists using two-pointers and build up a resulting list so that values are monotonically increased.
```

---

<a id="lc-24"></a>
:heavy_check_mark: :orange_book: [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/): (**Dummy Nodes** with a super helpful **visual guide**)

```python
class Solution(object):
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """

        # Reference Solution:

        # https://leetcode.com/problems/swap-nodes-in-pairs/discuss/1774318/Python3-I-HATE-LINKED-LISTS-shDsh-Not-Explained/1268978
        # For visual guide: https://leetcode.com/problems/swap-nodes-in-pairs/discuss/1775033/swapping-nodes-not-just-the-values-visual-explanation-well-explained-c

        dummy = ListNode(None, head)
        prev, cur = dummy, head

        while cur and cur.next:
            prev.next = cur.next
            cur.next = prev.next.next
            prev.next.next = cur
            # prev, cur = cur, cur.next
            prev = cur
            cur = cur.next

        return dummy.next
```

---

<a id="lc-206"></a>
:heavy_check_mark: :green_book: [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

```python
class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """

        # YouTube Tutorial:
        # https://youtu.be/XDO6I8jxHtA

        if not head:
            return None

        if not head.next:
            return head

        prev = None
        while head:
            curr = head
            head = head.next
            curr.next = prev
            prev = curr

        return prev
```

---

<a id="lc-25"></a>
:heavy_check_mark: :closed_book: [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/): find k nodes ahead, reverse that group in place, reconnect with a `jump` pointer

```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:

        # Ref:
        # https://leetcode.com/problems/reverse-nodes-in-k-group/discuss/11491/Succinct-iterative-Python-O(n)-time-O(1)-space

        # jump: used to connect last node in previous k-group to first node in following k-group
        dummy = jump = ListNode(0)

        # l, r: define reversing range
        l = r = head
        dummy.next = head

        while True:
            count = 0

            while r and count < k:   # use r to locate the range
                r = r.next
                count += 1

            if count == k:  # if size k satisfied, reverse the inner linked list
                # Ref: https://leetcode.com/problems/reverse-nodes-in-k-group/discuss/11491/Succinct-iterative-Python-O(n)-time-O(1)-space/217945
                pre, cur = r, l

                for _ in range(k):
                    cur.next, cur, pre = pre, cur.next, cur

                jump.next, jump, l = pre, l, r  # connect two k-groups

            else:
                return dummy.next
```

---

<a id="lc-203"></a>
:heavy_check_mark: :green_book: [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

```python
class Solution(object):
    def removeElements(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """

        if not head:
            return None
        if not head.next:
            return head if head.val != val else None

        prev, curr = head, head.next

        while curr:
            if curr.val == val: # delete curr node
                prev.next = curr.next
                curr = curr.next
            else:
                curr = curr.next
                prev = prev.next

        # Check the first element (cause we skipped it with the curr = head.next)
        if head.val == val:
            head = head.next

        return head

        # We can also use a dummy node:
        # https://leetcode.com/problems/remove-linked-list-elements/discuss/158651/Simple-Python-solution-with-explanation-(single-pointer-dummy-head).
```

---

<a id="lc-328"></a>
:heavy_check_mark: :orange_book: [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/): (**Double Pointers**, one for odd elements and one for even elements)

```python
class Solution(object):
    def oddEvenList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """

        # idea: filter out list of odd nodes and list of even nodes, then merge

        if not head:
            return None
        if not head.next:
            return head

        odd = head # Both of them point at the first node of the target linked list
        even = head.next # doesn't matter even there's only one node in the linked list (even will become None)
        eHead = even # We have to keep where the even-node list starts

        while even and even.next: # won't get in the loop at first if there's only one node in the linked list
            # both even and even.next are necessary condition because even might point to None, which has no attribute 'next'
            # AND, why these two, small discussion by myself as below
            odd.next = odd.next.next
            even.next = even.next.next
            # After these two ops, odd/even still points at its original place
            # Therefore, we move them to the next node repectively
            odd = odd.next
            even = even.next

        odd.next = eHead # the odd pointer currently points at the last node of the odd-node list

        return head # We keep the start of the odd-node list in the first of our code
```

---

<a id="lc-2"></a>
:wavy_dash: :orange_book: [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/): (**Double Pointers**, one for each linked list)

```python
class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """


        first_ptr, second_ptr = l1, l2
        carry_over = 0

        first_len, second_len = 0, 0
        while first_ptr:
            first_len += 1
            first_ptr = first_ptr.next
        while second_ptr:
            second_len += 1
            second_ptr = second_ptr.next

        if first_len < second_len:
            l1, l2 = l2, l1

        # Now l1 is always the longer linked list
        first_ptr, second_ptr = l1, l2

        while first_ptr or second_ptr:

            if second_ptr:
                value = (first_ptr.val + second_ptr.val + carry_over) % 10
                carry_over = (first_ptr.val + second_ptr.val + carry_over) // 10
                first_ptr.val = value

                if not first_ptr.next and carry_over != 0:
                    # We need an extra node for extra digit
                    new = ListNode(carry_over)
                    first_ptr.next = new
                    break
                else:
                    first_ptr = first_ptr.next
                    second_ptr = second_ptr.next

            else:
                value = (first_ptr.val + carry_over) % 10
                carry_over = (first_ptr.val + carry_over) // 10
                first_ptr.val = value

                if not first_ptr.next and carry_over != 0:
                    # We need an extra node for extra digit
                    new = ListNode(carry_over)
                    first_ptr.next = new
                    break
                else:
                    first_ptr = first_ptr.next

        return l1
```

---

<a id="lc-237"></a>
:wavy_dash: :green_book: [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/): A very down-voted trick question, BUT it seems to be a popular interview question

```python
class Solution(object):
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        # Replace the node with the value of the node after it
        # And then skip the next node
        # BUT we are leaving the next node hanging there, possible memory leak
        # SO this is kind of a trick question (that's why there are so many downvotes)

        node.val = node.next.val
        node.next = node.next.next
```

---

<a id="lc-138"></a>
:wavy_dash: :orange_book: [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/): <key, val> = <original node, its copy>

---

## 4. Design

<a id="lc-146"></a>
:heavy_check_mark: :orange_book: [146. LRU Cache](https://leetcode.com/problems/lru-cache/): hash map of key -> previous node + singly linked list; move used keys to the tail, evict from the head

```python
class LinkedNode:
    def __init__(self, key = None, val = None, next = None):
        self.key = key
        self.val = val
        self.next = next

class LRUCache(object):

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.capacity = capacity
        self.dummy = LinkedNode()
        self.tail = self.dummy
        self.key_to_prev = {} # <key, val> = <key, node that is before key's node>

    def get(self, key):
        """
        :type key: int
        :rtype: int
        """
        if key not in self.key_to_prev: # key is not in cache
            return -1

        self.kick(key) # key is just visited, so kick it to the end of linked list
        return self.tail.val # key is not at tail

    def kick(self, key):
        prev = self.key_to_prev[key]
        key_node = prev.next

        if key_node == self.tail: # if the node is already the tail node, do nothing
            return

        prev.next = key_node.next # skip key node
        self.key_to_prev[key_node.next.key] = prev # prev node for the node after key node should be prev
        key_node.next = None # break the connection between key node and the node after it

        self.push_back(key_node) # put key node to the tail

    def push_back(self, node):
        self.key_to_prev[node.key] = self.tail
        self.tail.next = node
        self.tail = node

    def put(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: None
        """
        if key in self.key_to_prev:
            self.kick(key) # because we just accessed it
            self.tail.val = value # update its value
            return

        # if key is not present in cache
        self.push_back(LinkedNode(key, value)) # create a new node for it and put to tail

        if len(self.key_to_prev) > self.capacity:
            self.pop_front() # remove the least recently used element

    def pop_front(self):
        head = self.dummy.next
        del self.key_to_prev[head.key]
        self.dummy.next = head.next # skip head
        self.key_to_prev[head.next.key] = self.dummy
```

---
