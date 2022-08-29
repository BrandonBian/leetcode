# LeetCode - Sorting Algorithms | Solutions
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---

:heavy_check_mark: :orange_book: [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)

```
See Study Guide for Sorting Algorithms
```

---
:heavy_check_mark: :orange_book: [148. Sort List](https://leetcode.com/problems/sort-list/)

```
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

:heavy_check_mark: :closed_book: [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

```
TODO
```

---

:wavy_dash: :closed_book: [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

```
class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        
        # Ref: https://leetcode.com/problems/count-of-smaller-numbers-after-self/discuss/445769/merge-sort-CLEAR-simple-EXPLANATION-with-EXAMPLES-O(n-lg-n)
        
        def merge_sort(array):
            if len(array) == 1:
                return array
            
            mid = len(array) // 2
            
            l = merge_sort(array[:mid])
            r = merge_sort(array[mid:])
            
            return merge(l, r)
        
        def merge(left, right):
            l, r = 0, 0
            result = []
            numElemsRightArrayLessThanLeftArray = 0
            
            while l < len(left) and r < len(right):
                if left[l][0] > right[r][0]:
                    result.append(right[r])
                    r += 1
                    numElemsRightArrayLessThanLeftArray += 1
                else:
                    result.append(left[l])
                    self.res[left[l][1]] += numElemsRightArrayLessThanLeftArray
                    l += 1
            
            if l < len(left):
                for i in range(l, len(left)):
                    result.append(left[i])
                    self.res[left[i][1]] += numElemsRightArrayLessThanLeftArray
            if r < len(right):
                for i in range(r, len(right)):
                    result.append(right[i])
            
            return result
                    
        
        array = []
        self.res = [0] * len(nums) # result[i] = # elements to the right of idex i and are smaller
        
        for idx, val in enumerate(nums):
            array.append((val, idx))
                
        merge_sort(array)
        return self.res
```

---

:wavy_dash: :closed_book: [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

```
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        
        # Ref: https://leetcode.com/problems/reverse-pairs/discuss/97268/General-principles-behind-problems-similar-to-%22Reverse-Pairs%22
        
        def merge_sort(left, right):
            
            if left >= right:
                return 0
            
            mid = (left + right) >> 1
            count = merge_sort(left, mid) + merge_sort(mid + 1, right) # pairs we can get from only left array and from only right array
            
            # count number of additional reverse pairs we can get when merging the two subarrays
            j = mid + 1
            for i in range(left, mid + 1):
                while j <= right and nums[i] > 2 * nums[j]:
                    j += 1
                count += j - mid - 1
            
            nums[left:right + 1] = sorted(nums[left:right + 1])
        
            return count
        
        return merge_sort(0, len(nums) - 1)
```


---
