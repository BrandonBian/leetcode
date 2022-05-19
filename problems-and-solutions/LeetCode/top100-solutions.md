# LeetCode - [Top 100 Problems](https://leetcode.com/problemset/all/?listId=79h8rn6&page=1) | Solutions
- **All problems are LeetCode problems, solved in Python.**
- **Relevant key points are also noted.**
- **Problem set selected by LeetCode officials.**

---
:green_book: [1. Two Sum](https://leetcode.com/problems/two-sum/): **Two Pointers** / Hash Map

```
class Solution:
    
    # Method 1: Two Pointers
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        # combine each element with its index (since we are later sorting, which will mess up the indices)
        nums_ = [
            (number, idx)
            for idx, number in enumerate(nums)
        ]
        
        nums_.sort() # sorting a tuple will sort based on the first element (i.e., number not index)
        left, right = 0, len(nums_) - 1
        
        while left <= right:
            if nums_[left][0] + nums_[right][0] > target:
                right -= 1
            elif nums_[left][0] + nums_[right][0] < target:
                left += 1
            else:
                return [nums_[left][1], nums_[right][1]]
    
    # Method 2: Hash Map
#     def twoSum(self, nums: List[int], target: int) -> List[int]:
        
#         # <key, val> = <number, its index in nums>
#         hash_map = {}
        
#         for idx, val in enumerate(nums):
#             key = target - val
            
#             if key in hash_map:
#                 return [hash_map[key], idx]
#             else:
#                 hash_map[val] = idx
```

---

:orange_book: [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/): Linked List + Mathematics

```
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
:orange_book: [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/): Two Pointers (Sliding Window) + Hash Table

```
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        # Reference Solution:
        # https://leetcode.com/problems/longest-substring-without-repeating-characters/discuss/347818/Python3%3A-sliding-window-O(N)-with-explanation
        
        seen = {} # <key, val> = <char, last appear index so far>
        l = 0
        output = 0
        
        for r in range(len(s)):
            """
            If s[r] not in seen, we can keep increasing the window size by moving right pointer
            """
            if s[r] not in seen:
                output = max(output,r-l+1) # take current window size and compare with max
            """
            There are two cases if s[r] in seen:
            case1: s[r] is inside the current window, we need to change the window by moving left pointer to seen[s[r]] + 1.
            case2: s[r] is not inside the current window, we can keep increase the window
            """
            else:
                if seen[s[r]] < l: # outside of current window, keep increasing window
                    output = max(output,r-l+1) # take current window size and compare with max
                else:
                    l = seen[s[r]] + 1 # inside current window, move left pointer to seen[s[r]] + 1
           
            seen[s[r]] = r
            
        return output
```

---

:closed_book: [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/): Binary Search

```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        
        # Ref: https://www.youtube.com/watch?v=LPFhl65R7ww
        
        len_1, len_2 = len(nums1), len(nums2)
        
        if len_1 > len_2:
            return self.findMedianSortedArrays(nums2, nums1)
        
        # at this point, nums1 will always be the smaller one
        
        left, right = 0, len_1
        
        while left <= right: # binary search on partition index of nums1
            partitionX = (left + right) >> 1
            partitionY = (len_1 + len_2 + 1) // 2 - partitionX
            
            # if partitionX is 0 it means nothing is there on left side. Use -INF for maxLeftX
            # if partitionX is length of input then there is nothing on right side. Use +INF for minRightX
            xLeftMax = float('-inf') if partitionX == 0 else nums1[partitionX - 1]
            xRightMin = float('inf') if partitionX == len_1 else nums1[partitionX]
            
            yLeftMax = float('-inf') if partitionY == 0 else nums2[partitionY - 1]
            yRightMin = float('inf') if partitionY == len_2 else nums2[partitionY]
            
            if xLeftMax <= yRightMin and yLeftMax <= xRightMin: 
                # we partitioned at the correct place (note that num1 and num2 both sorted)
                
                if (len_1 + len_2) % 2 == 0: # even number of total elements
                    return (max(xLeftMax, yLeftMax) + min(xRightMin, yRightMin)) / 2
                else: # odd number of total elements
                    return max(xLeftMax, yLeftMax)
                
            elif xLeftMax > yRightMin: # we are too far on right side for partitionX. Go on left side.
                right = partitionX - 1
            else: # we are too far on left side for partitionX. Go on right side.
                left = partitionX + 1
                
        return -1
```

---

:orange_book: [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/): Dynamic Programming / **Two Pointers**

```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        
        # Method: Two Pointers
        res = ""
        
        for i in range(len(s)):        
            odd  = self.palindromeAt(s, i, i)
            even = self.palindromeAt(s, i, i+1)

            res = max(res, odd, even, key=len)
            
        return res

    # starting at l,r expand outwards to find the biggest palindrome
    def palindromeAt(self, s, l, r):   
        
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1
            
        return s[l + 1:r]
```

---

:closed_book: [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/): Dynamic Programming

```
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        
        # Ref: https://leetcode.com/problems/regular-expression-matching/discuss/5723/My-DP-approach-in-Python-with-comments-and-unittest
        
        # YouTube: https://www.youtube.com/watch?v=l3hda49XcDE
                
        # The DP table and the string s and p use the same indexes i and j, but
        # table[i][j] means the match status between s[:i] and p[:j], i.e.
        # table[0][0] means the match status of two empty strings, and
        # table[1][1] means the match status of s[0] and p[0]. Therefore, when
        # refering to the i-th and the j-th characters of s and p for updating
        # table[i][j], we use s[i - 1] and p[j - 1].
        
        # Initialize the table with False.
        # The first column is automatically satisfied (if we have an empty pattern, there will never be a match)
        table = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]
        
        # Update the corner case of matching two empty strings.
        table[0][0] = True
        
        # Update the corner case of when s is an empty string but p is not.
        # Since each '*' can eliminate the character before it, the table is
        # horizontally updated by the one before previous (on first row).
        for i in range(1, len(p) + 1):
            if p[i - 1] == '*':
                table[0][i] = table[0][i - 2]
        
        # Populate DP table
        for i in range(1, len(s) + 1):
            for j in range(1, len(p) + 1):
                
                # since the table index starts at 1, we need to decrement by 1 to access the original arrays
                this_pttn, this_char = p[j - 1], s[i - 1]
                
                if this_pttn == '.' or this_pttn == this_char: # if pattern is '.' or the characters match
                    # we can just ignore both character in string and pattern (by looking to the top-left diagonal element)
                    table[i][j] = table[i - 1][j - 1] 
                    
                elif this_pttn == '*':
                    
                    # 0 occurrence of the pattern
                    # we look two steps to the left in table
                    table[i][j] = table[i][j - 2]
                    
                    # one or more occurrences of the pattern
                    # we need the pattern char before '*' to match the char in string or is '.' for arbitrary char
                    prev_pttn = p[j - 2]
                    
                    if prev_pttn == this_char or prev_pttn == '.':
                        # look up one row on the table, i.e. try ignoring current char in string and re-do comparison
                        # the OR here is to account for the "0 occurrence of the pattern" case we just assigned before
                        table[i][j] = table[i][j] or table[i - 1][j] 

                else:
                    table[i][j] = False

        return table[-1][-1]
```

---

:orange_book: [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Two Pointers (Greedy)

```
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        
        # Reference Solution:
        # https://leetcode.com/problems/container-with-most-water/discuss/6110/Very-simple-O(n)-solution
        
        left = 0
        right = len(height) - 1
        maxArea = 0
        
        while left < right:
        
            area = (right - left) * min(height[left], height[right])
            maxArea = max(maxArea, area)
            
            # limited by the lowest bar, so change the lowest bar to maximize it
            if height[left] > height[right]:
                right -= 1
            else:
                left += 1
        
        return maxArea
```

---

:orange_book: [15. 3Sum](https://leetcode.com/problems/3sum/): Two Pointers (reduce to 2 Sum)

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        
        # idea: fix one of the nums, then use 2 Sum method so that the sum of the other 2 elements = -(first element)
        
        def find_two_sum(left, right, target, result):
            while left < right:
                two_sum = nums[left] + nums[right]
                
                if two_sum < target:
                    left += 1
                elif two_sum > target:
                    right -= 1
                else:
                    result.append([-target, nums[left], nums[right]])
                    left += 1
                    right -= 1
                    
                    # skip duplicates
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1
        
        
        if len(nums) < 3:
            return None
        
        nums.sort()
        length = len(nums)
        result = []
        
        for i in range(0, length - 2): # fix the first element and run 2 Sum on rest 
            
            # skip duplicate (nums[i] already visited in prior loop)
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            # two pointers from the element next to this first element to the last
            left, right = i + 1, length - 1
                        
            find_two_sum(left, right, -nums[i], result)
        
        return result
```

---

:orange_book: [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/): DFS for Combinations

```
class Solution(object):
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        
        maps = {"2": "abc", "3": "def", "4":"ghi", "5":"jkl", "6":"mno", "7":"pqrs", "8":"tuv", "9":"wxyz"}
        
        def dfs(candidates, target, path, res):
            if target >= len(digits): # we have gone through all digits
                res.append(path)
                return
            
            string1 = maps[candidates[target]] # all letters corresponding to this digit
            
            for i in string1:
                dfs(candidates, target + 1, path + i, res)
        
        
        if len(digits) == 0:
            return None
        
        res = []
        
        dfs(digits, 0, "", res)
        return res
```

---

:orange_book: [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/): Linked List + Fast/Slow Pointers

```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
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
            
        slow.next = slow.next.next
        
        return head
```

---

:green_book: [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/): Stack

```
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        
        brackets = {'(':')', '{':'}', '[':']'}
        stack = []
        
        for idx, value in enumerate(s):
            if value == '(' or value == '[' or value == '{':
                stack.append(value)
            else:
                if len(stack) == 0: # If nothing in stack, definitely no match for this value
                    return False
                
                val = stack.pop() # Obtain the top-most element in stack
                
                if brackets[val] != value:
                    return False

        return len(stack) == 0
```

---

:green_book: [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/): Linked List + Two Pointers

```
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

:orange_book: [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/): DFS for Combinations

```
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
    
        # Backtracking solution
        
        def backtracking(nOpen, nClose, path, ans):
            if n == nClose:  # We have placed [n] number of ')'
                ans.append(path)
                return

            if nOpen < n:  # Number of '(' up to `n`
                backtracking(nOpen + 1, nClose, path + "(", ans)
                
            if nClose < nOpen:  # Number of ')' up to number of '('
                backtracking(nOpen, nClose + 1, path + ")", ans)

        ans = []
        backtracking(0, 0, "", ans)
        
        return ans
```

---

:closed_book: [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/): Linked List + Merge Sort

```
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        
        if not lists:
            return None
        if len(lists) == 1:
            return lists[0]
        
        # Merge Sort
        
        mid = len(lists) // 2
        l, r = self.mergeKLists(lists[:mid]), self.mergeKLists(lists[mid:])
        
        return self.merge(l, r)
    
    def merge(self, l, r):
        # merge [l] and [r] lists into one list in ascending order
        
        if not l:
            return r
        if not r:
            return l
        
        dummy = p = ListNode()
        
        while l and r:
            if l.val < r.val:
                p.next = l
                l = l.next
            else:
                p.next = r
                r = r.next
            p = p.next
            
        p.next = l or r
        
        return dummy.next
```

---

:orange_book: [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/): Linked List + Two Pointers

```
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
            
            prev, cur = cur, cur.next
            
        return dummy.next
```

---

:closed_book: [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/): Linked List

```
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




