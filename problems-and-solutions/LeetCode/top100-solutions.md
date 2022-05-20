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

:orange_book: [31. Next Permutation](https://leetcode.com/problems/next-permutation/): **Two Pointers**

```
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        
        # Ref: https://www.nayuki.io/page/next-lexicographical-permutation-algorithm
        # Follow the steps in Ref
        
        if len(nums) == 1:
            return nums
        
        i = j = len(nums) - 1
        
        while i > 0 and nums[i - 1] >= nums[i]:
            i -= 1
        
        # now [i] is at the start of the longest non-increasing suffix
        
        if i == 0: # in this case, the suffix is the entire string, reverse it (no larger possible permutation)
            nums[:] = nums[::-1]
            return
        
        pivot = i - 1
        
        while nums[j] <= nums[pivot]:
            j -= 1
            
        # now nums[j] is the rightmost element that is larger than nums[pivot]
        # swap with pivot
        nums[pivot], nums[j] = nums[j], nums[pivot]
        
        # reverse the suffix portion
        nums[i:] = nums[len(nums) - 1: i - 1: -1]
        
        return
```

---

:closed_book: [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/): **Stack** / Dynamic Programming

```
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        
        if len(s) <= 1:
            return 0
        
        # stack will hold [indices] instead of the [parentheses]
        # Ref: https://www.youtube.com/watch?v=VdQuwtEd10M
        stack = [-1] # -1 here to account for the edge case in which given string is already valid
        result = 0
        
        for i in range(len(s)):
            char = s[i]
            
            if char == '(':
                stack.append(i)
            else:
                stack.pop()
                if not stack:
                    stack.append(i)
                else:
                    length = i - stack[-1]
                    result = max(result, length)
        
        return result
```

---

:orange_book: [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/): Binary Search

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        
        left, right = 0, len(nums) - 1
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if nums[mid] == target:
                return mid

            if nums[mid] > nums[left]:
                if nums[left] <= target <= nums[mid]:
                    right = mid
                else:
                    left = mid
            else:
                if nums[mid] <= target <= nums[right]:
                    left = mid
                else:
                    right = mid
            
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
        return -1
```

---

:orange_book: [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/): Binary Search

```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        
        # basic checking
        if not nums:
            return [-1, -1]

        first_pos, last_pos = -1, -1

        ####################################
        # find first position (MINIMIZING) #
        ####################################

        def sufficient_minimize(num):
            # find least number that is >= target
            return num >= target

        left, right = 0, len(nums) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            if sufficient_minimize(nums[mid]): # look left for smaller solution
                right = mid
            else:
                left = mid

        # for MINIMIZING position, check "left" first
        if nums[left] == target:
            first_pos = left
        elif nums[right] == target:
            first_pos = right

        ###################################
        # find last position (MAXIMIZING) #
        ###################################

        def sufficient_maximize(num):
            # find max number that is <= target
            return num <= target

        left, right = 0, len(nums) - 1

        while left + 1 < right:
            mid = (left + right) >> 1

            if sufficient_maximize(nums[mid]): # look right for larger solution
                left = mid
            else:
                right = mid

        # for MAXIMIZING position, check "right" first

        if nums[right] == target:
            last_pos = right
        elif nums[left] == target:
            last_pos = left

        ##########
        # return #
        ##########

        return [first_pos, last_pos]
```

---

:green_book: [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/): Binary Search

```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        
        # notice the len(nums) here instead of len(nums-1) for placing integer at end
        left, right = 0, len(nums) 
        
        def feasible(num):
            return num >= target
        
        while left + 1 < right:
            mid = (left + right) >> 1
            
            if feasible(nums[mid]):
                right = mid
            else:
                left = mid

        if feasible(nums[left]):
            return left
        else:
            return right
```

---

:orange_book: [39. Combination Sum](https://leetcode.com/problems/combination-sum/): DFS for Combinations

```
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """

        def dfs(nums, target, path, ret):
            
            print("Path: " + str(path))
            print("Candidates: " + str(nums))
            print('')
            
            if target < 0:
                return 
            if target == 0:
                ret.append(path)
                return 
            for i in range(len(nums)):
                # not nums[i+1:] because we can reuse the same number
                dfs(nums[i:], target-nums[i], path+[nums[i]], ret)
                
        ret = []
        dfs(candidates, target, [], ret)
        return ret
```

---

:closed_book: [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/): Hash Table

```
class Solution(object):
    def firstMissingPositive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        """
        For nums with length n, the possible result is in the range of
        [1 : n + 1], we want to know the smallest integer in the range 
        of [1 : n] that is not in nums, if [1 : n] are all in nums,
        the result is n + 1
        
        So those numbers not in [1 : n] are not useful and we can just
        change them to be 0
        
        Then we go through nums, if nums[i] is in the range of 
        [1 : n], we use index (nums[i] - 1) to record that we have
        seen nums[i] by adding n + 1 to nums[nums[i] - 1]
        
        Finally we just need to find the first index i for which 
        nums[i] is less than n + 1 (which means we never met number
        i + 1 so we did not add n + 1 to nums[i])
        """

        # Solution according to: https://www.youtube.com/watch?v=8g78yfzMlao
        
        for i in range(len(nums)):
            if nums[i] < 0:
                nums[i] = 0
                
        # Our solution must be in [1, len(nums)], or len(nums) + 1 if all numbers present
        # For each number in nums, map it to index [number - 1] to indicate its existence
        # To do so, we change the number at [number - 1] as negative (to indicate it exists in nums)
        # Since we already changed all negative numbers to 0, we don't worry about original negative numbers
        # Each time we check a number in nums, we take absolute value so that the sign does not interfere
        # If number at the index is 0, we need to change it to -(len + 1)
        # because [len + 1] is the worst / greatest solution possible, its index [len] is out of bound
            
        for i in range(len(nums)):
            val = abs(nums[i])
            if 1 <= val <= len(nums):
                if nums[val - 1] > 0:
                    nums[val - 1] *= -1
                elif nums[val - 1] == 0:
                    nums[val - 1] = -1 * (len(nums) + 1)
                
        # Finally we check each possible solution, go to its mapped index [i - 1]
        # If the value there is non-negative then this solution did not appear in the nums
        # So we return it as the smallest missing positive integer
        
        for i in range(1, len(nums) + 1): # check all possible solutions in range
            if nums[i - 1] >= 0: # it is not present in the nums
                return i
            
        # If all solutions present, then the smallest missing positive integer is the next integer after the max of nums
        return len(nums) + 1 
```

---

:closed_book: [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/): Two Pointers

```
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        
        # Ref:
        # https://leetcode.com/problems/trapping-rain-water/discuss/17391/Share-my-short-solution./185869
        
        if not height:
            return 0
        
        left, right = 0, len(height) - 1
        left_max, right_max = height[left], height[right]
        result = 0
        
        while left <= right:
            left_max = max(left_max, height[left])
            right_max = max(right_max, height[right])
            
            if left_max < right_max: # we are limited by the max left height
                result += left_max - height[left]
                left += 1
            else: # we are limited by the max right height
                result += right_max - height[right]
                right -= 1
                
        return result
```

---

:orange_book: [55. Jump Game](https://leetcode.com/problems/jump-game/): Dynamic Programming

```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        if not nums:
            return False
        
        # dp[i] = the farthest index we can reach given allowed steps nums[0:i]
        dp = [0 for _ in range(len(nums))]
        
        # initialization (the farthest position we can reach from nums[0] is the maximum jump length at this position = nums[0])
        dp[0] = nums[0]
        
        for i in range(1, len(nums) - 1):
            
            if dp[i - 1] < i: # index i cannot be reached starting from any index in range of [0: i - 1]
                return False  # we can never reach the end
            
            # the farthest position we can reach is either the same as before, 
            # OR is the current position + the maximum jump length at this position
            dp[i] = max(dp[i - 1], i + nums[i]) 
            
            if dp[i] >= len(nums) - 1: # we can already jump to the end
                return True
            
        # see whether we can reach the end starting from any index in range of [0: len(nums) - 2]
        return dp[len(nums) - 2] >= len(nums) - 1
```

---

:orange_book: [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/): Greedy (1-D BFS + 2 Pointers)

```
class Solution:
    def jump(self, nums: List[int]) -> int:
        
        # Ref:
        # https://www.youtube.com/watch?v=dJ7sWiOoK7g
        
        n = len(nums)
        if n == 1:
            return 0
        
        result = 0
        
        # the window of search space
        left, right = 0, 0
        
        # stop when our window's right boundary meets the end
        # cause when right == len(nums) - 1, we still have result += 1 even though we already reached the end
        # so we are taking a redundant step
        while right < len(nums) - 1: 
            farthest_idx = 0
            
            for i in range(left, right + 1): # we are considering the right index INCLUSIVE
                farthest_from_i = i + nums[i] # the farthest position we can reach by jumping from position [i]
                
                # we are finding the global farthest position -> 
                # i.e., farthest position we can reach from any position in window [left, right] (right inclusive)
                farthest_idx = max(farthest_idx, farthest_from_i) 
            
            # update the window boundary (left moves to the right of right, right moves to the farthest index)
            left = right + 1
            right = farthest_idx
            
            result += 1 # we made a jump
            
        return result
```

---

:orange_book: [46. Permutations](https://leetcode.com/problems/permutations/): DFS for Permutations

```
class Solution(object):
    
    def dfs(self, nums, path, res):

        if not nums:
            res.append(path)
            return # backtracking
        for i in range(len(nums)):
            # nums[:i] + nums[i+1:] -> nums but leaving out i
            self.dfs(nums[:i]+nums[i+1:], path+[nums[i]], res)
    
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        
        res = []
        self.dfs(nums, [], res)
        return res
```

---

:orange_book: [48. Rotate Image](https://leetcode.com/problems/rotate-image/): Matrix + Math

```
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        
        # reverse up and down
        l = 0
        r = len(matrix) -1
        while l < r:
            matrix[l], matrix[r] = matrix[r], matrix[l]
            l += 1
            r -= 1
            
        # transpose 
        for i in range(len(matrix)):
            for j in range(i):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
        
        return matrix
```

---

:orange_book: [49. Group Anagrams]([https://leetcode.com/problems/rotate-image/](https://leetcode.com/problems/group-anagrams/)): Hash Table

```
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        
        # Reference Solution:
        # https://leetcode.com/problems/group-anagrams/discuss/19202/5-line-Python-solution-easy-to-understand/262771
        
        hmap = collections.defaultdict(list)
        
        for this_str in strs:

            # hash map for the 26 letters
            array = [0] * 26
            
            for letter in this_str:
                array[ord(letter) - ord('a')] += 1
                
            # array/list not hashable, so change to tuple
            hmap[tuple(array)].append(this_str)

        return hmap.values()
```

---

:green_book: [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/): Kadane's Algorithm

```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        
        if not nums:
            return None
        
        # Kadane's Algorithm for Maximum Subarray problem
        
        curSum = maxSum = nums[0]
        
        for num in nums[1:]:
            curSum = max(num,            # either this num
                         curSum + num)   # OR this number plus the maximum subarray sum right before this element
            maxSum = max(maxSum, curSum) # update maxSum (i.e., global maximum sum)

        return maxSum
```

---

:orange_book: [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/): Sorting

```
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: List[List[int]]
        """
        
        # sort based on the start of each interval
        intervals.sort(key = lambda x: x[0])
        
        output = [intervals[0]]
        
        for start, end in intervals[1:]:
            # output[-1] is the last interval, so output[-1][1] is the last end time
            last_end = output[-1][1]
            
            if start <= last_end: # they are overlapping
                # set the last ending time as the max of itself or the current end value
                # consider the edge case of merging: [1, 5] & [2, 4]
                output[-1][1] = max(output[-1][1], end)
            
            else:
                output.append([start, end])
                
        return output
```

---

:orange_book: [62. Unique Paths](https://leetcode.com/problems/unique-paths/): Dynamic Programming

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Top-down DP

        # def: dp[i][j] = # of unique paths to reach (i, j) from (0, 0)
        dp = [[0 for _ in range(n)] for _ in range(m)]
        
        # initialize dp
        for i in range(n): # the first row
            dp[0][i] = 1
        for j in range(m): # the first column
            dp[j][0] = 1
        
        for i in range(1, m): # row
            for j in range(1, n): # col
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        return dp[m - 1][n - 1]
```

---

:orange_book: [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | Dynamic Programming

```
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        
        # Idea: dynamic programming
        # dp[i,j] = minimum sum to this location on grid
            
        dp = grid
        nrows = len(grid)
        ncols = len(grid[0])
        
        for i in range(1, ncols):
            dp[0][i] += dp[0][i-1]
        
        for i in range(1, nrows):
            dp[i][0] += dp[i-1][0]
            
        for row in range(1, nrows):
            for col in range(1, ncols):
                # comming from above (row - 1) or from the left (col - 1)
                dp[row][col] += min(dp[row-1][col], dp[row][col-1])
        
        return dp[nrows-1][ncols-1]       
```

---

:green_book: [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | Dynamic Programming (Fibonacci)

```
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        
        # Solution: https://leetcode.com/problems/climbing-stairs/discuss/25299/Basically-it's-a-fibonacci.
        # Idea: Fibonacci
        
        # Special Cases
        if n == 1:
            return 1
        if n == 2:
            return 2
    
        # dp[i] = number of distinct ways to reach step i
        dp = [0,1,2]
        
        for i in range(3, n+1):
            dp.append(dp[i-2] + dp[i-1])
        
        return dp[n]
```

---

:closed_book: [72. Edit Distance](https://leetcode.com/problems/edit-distance/): Dynamic Programming

```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        
        # Ref: https://www.youtube.com/watch?v=We3YDTzNXEk
        
        if not word1 and not word2: # they are already the same empty string
            return 0
        if not word1:
            return len(word2)
        if not word2:
            return len(word1)

        # dp[i][j] = minimum number of operations required to convert s1[0..i-1] string to s2[0..j-1] string
        m, n = len(word1), len(word2)
        dp = [[float('inf') for _ in range(n + 1)] for _ in range(m + 1)]
        
        # initialization
        for i in range(m): # the first column
            dp[i][0] = i
        for j in range(n): # the first row
            dp[0][j] = j
            
        # dp
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    # if we are comparing the same character, look to top-left corner
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    # we find the minimum element from left / top / top-left corner
                    # and then we add 1 to account for the operation of changing the current character
                    dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
            
        return dp[m][n] 
```

---

:orange_book: [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) | Hashing (in-place)

```
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        
        # Ref: https://www.youtube.com/watch?v=T41rL0L3Pnw
        
        # we need an Space = O(1) solution, since we need to do in-place
        # we use the first row and first col of matrix to record whether a 0 appeared
        m, n = len(matrix), len(matrix[0])
        
        # still, we would need extra variables to deal with the first row and column
        first_row_has_zero = False
        first_col_has_zero = False
        
        for row in range(m):
            for col in range(n):
                if matrix[row][col] == 0:
                    if row == 0:
                        first_row_has_zero = True
                    if col == 0:
                        first_col_has_zero = True
                    
                    matrix[0][col] = 0 # record in first row
                    matrix[row][0] = 0 # record in first col
                    
        # iterate through matrix to update the cell to be zero if it's in a zero row or col
        # BUT leaving out the first row and first col, since we will be dealing with them last
        for row in range(1, m):
            for col in range(1, n):
                if matrix[0][col] == 0 or matrix[row][0] == 0:
                    matrix[row][col] = 0
                    
        # update the first row and col if they're zero
        if first_row_has_zero:
            for col in range(n):
                matrix[0][col] = 0
        
        if first_col_has_zero:
            for row in range(m):
                matrix[row][0] = 0       
```

---

:orange_book: [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/): Binary Search (2-D coordinates -> 1-D)

```
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        
        # Reference
        # https://leetcode.com/problems/search-a-2d-matrix/discuss/26201/A-Python-binary-search-solution-O(logn)
        
        rows, cols = len(matrix), len(matrix[0])
        left, right = 0, rows * cols - 1
        
        while left + 1 < right:
            mid = (left + right) >> 1

            num = matrix[mid // cols][mid % cols] # transform 1D index into position in 2D array
            
            # then just classic binary search
            if num == target:
                return True
            elif num < target:
                left = mid
            else:
                right = mid
        
        if matrix[left // cols][left % cols] == target or matrix[right // cols][right % cols] == target:
            return True
        
        return False
```

---

:orange_book: [75. Sort Colors](https://leetcode.com/problems/sort-colors/): Counters / **Two Pointers (Partition)**

```
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        
        def partition(nums, k):
            # point to the last element in the (< k) region (i.e., a barrier)
            last_small_ptr = -1

            for i in range(len(nums)):
                if nums[i] < k:
                    # the barrier moves one step
                    last_small_ptr += 1

                    # exchange
                    nums[last_small_ptr], nums[i] = \
                    nums[i], nums[last_small_ptr]

        partition(nums, 1)
        partition(nums, 2)

        return nums
        
        
#         if len(nums) == 1:
#             return nums
        
#         nums_zero, nums_one, nums_two = 0, 0, 0
        
#         # count the number of each colors
#         for i in range(len(nums)):
#             if nums[i] == 0:
#                 nums_zero += 1
#             if nums[i] == 1:
#                 nums_one += 1
#             if nums[i] == 2:
#                 nums_two += 1
        
#         # replace
#         for i in range(len(nums)):
#             if nums_zero != 0:
#                 nums[i] = 0
#                 nums_zero -= 1
#             elif nums_one != 0:
#                 nums[i] = 1
#                 nums_one -= 1
#             else:
#                 nums[i] = 2
#                 nums_two -= 1
                
#         return nums
```

---

:closed_book: [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/): Two Pointers (Sliding Window)

```
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        
        # Ref:
        # https://leetcode.com/problems/minimum-window-substring/discuss/226911/Python-two-pointer-sliding-window-with-explanation
        
        # we need to make sure all letters in [t] appeared their corresponding number of times
        target_letter_counts = Counter(t)
        target_length = len(t)
        
        # start and end of the sliding window
        start, end = 0, 0
        
        min_window = ""
        
        # we first fix the [start] and move the [end] to the right
        for end in range(len(s)):
            # If we see a target letter, decrease the total target letter count
            # We might have more than needed number for a specific character
            # what we do is we dont count down target_length from then on when we see that element
            
            if target_letter_counts[s[end]] > 0:
                target_length -= 1 # we used one letter from the target pool/string
        
            # Decrease the letter count for the current letter
			# If the letter is not a target letter, the count just becomes -ve
            # Yes, we can decrement an element not yet present in the Counter
            target_letter_counts[s[end]] -= 1 # what this does makes all non-target characters negative as well
            
            # If all letters in the target are found
            # try to contract the window by moving the [start] pointer forward
            while target_length == 0:
                window_length = end - start + 1
                
                if not min_window or window_length < len(min_window):
                    # Note the new minimum window
                    min_window = s[start : end + 1]
                    
                # Increase the letter count of the current letter
                # we will increase this for all letters. but only the target elements have a chance to have positive counters
                target_letter_counts[s[start]] += 1
                
                # If all target letters have been seen and now, a target letter is seen with count > 0
                # what does this mean? one of the target characters that we use to validate the target is now out of the window
                if target_letter_counts[s[start]] > 0:
                    target_length += 1
                
                start += 1 # move the [start] pointer forward to further contract the window
                
        return min_window        
```

---







