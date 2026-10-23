# Linked Lists · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. Cycle Detection & Finding (Slow / Fast Pointers)

:heavy_check_mark: :green_book: [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/): (either using **slow/fast pointers** or a clever way of node deletion) &nbsp;→ [solution](solutions.md#lc-141)

:heavy_check_mark: :orange_book: [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/): (using **slow/fast pointers**) &nbsp;→ [solution](solutions.md#lc-142)

## 2. More Slow / Fast Pointer Problems

:heavy_check_mark: :green_book: [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/): (**Slow/Fast Pointers to divide list & reversing linked list**) &nbsp;→ [solution](solutions.md#lc-234)

:heavy_check_mark: :green_book: [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/): (**Slow/Fast Pointers for cycle detection & concatenating lists**) &nbsp;→ [solution](solutions.md#lc-160)

:heavy_check_mark: :orange_book: [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/): (**Slow/Fast Pointers for finding N-th node from end of list**) &nbsp;→ [solution](solutions.md#lc-19)

:heavy_check_mark: :orange_book: [148. Sort List](https://leetcode.com/problems/sort-list/): Merge sort on linked list &nbsp;→ [solution](solutions.md#lc-148)

## 3. Dummy Nodes, Double Pointers & Reversal

:heavy_check_mark: :green_book: [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/): dummy node + two pointers, attach the smaller node each time &nbsp;→ [solution](solutions.md#lc-21)

:heavy_check_mark: :orange_book: [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/): (**Dummy Nodes** with a super helpful **visual guide**) &nbsp;→ [solution](solutions.md#lc-24)

:heavy_check_mark: :green_book: [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) &nbsp;→ [solution](solutions.md#lc-206)

:heavy_check_mark: :closed_book: [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/): find k nodes ahead, reverse that group in place, reconnect with a `jump` pointer &nbsp;→ [solution](solutions.md#lc-25)

:heavy_check_mark: :green_book: [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/) &nbsp;→ [solution](solutions.md#lc-203)

:heavy_check_mark: :orange_book: [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/): (**Double Pointers**, one for odd elements and one for even elements) &nbsp;→ [solution](solutions.md#lc-328)

:wavy_dash: :orange_book: [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/): (**Double Pointers**, one for each linked list) &nbsp;→ [solution](solutions.md#lc-2)

:wavy_dash: :green_book: [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/): A very down-voted trick question, BUT it seems to be a popular interview question &nbsp;→ [solution](solutions.md#lc-237)

:wavy_dash: :orange_book: [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/): <key, val> = <original node, its copy>

## 4. Design

:heavy_check_mark: :orange_book: [146. LRU Cache](https://leetcode.com/problems/lru-cache/): hash map of key -> previous node + singly linked list; move used keys to the tail, evict from the head &nbsp;→ [solution](solutions.md#lc-146)
