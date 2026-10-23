# Sorting · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Sorting Algorithms & Quick Select

<a id="lc-912"></a>
:heavy_check_mark: :orange_book: [912. Sort an Array](https://leetcode.com/problems/sort-an-array/): All types of sorting algorithms can work

See the [sorting guide](guide.md): quick sort, merge sort, counting sort and heap sort, all written for this problem.

---

<a id="lc-215"></a>
:heavy_check_mark: :orange_book: [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/): quick select with a random pivot, O(n) on average

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:

        # plain sorting: O(NlogN)
        # return sorted(nums)[-k]

        # quick select: O(logN -> N)
        # https://leetcode.com/problems/kth-largest-element-in-an-array/discuss/1019513/Python-QuickSelect-average-O(n)-explained

        if not nums:
            return
        pivot = random.choice(nums)

        left =  [x for x in nums if x > pivot]
        mid  =  [x for x in nums if x == pivot]
        right = [x for x in nums if x < pivot]

        L, M = len(left), len(mid)

        if k <= L:
            return self.findKthLargest(left, k)
        elif k > L + M:
            return self.findKthLargest(right, k - L - M)
        else:
            # all elements here are equal, just return one of them
            return mid[0]
```

---

## 2. Merge Sort Applications

<a id="lc-148"></a>
:heavy_check_mark: :orange_book: [148. Sort List](https://leetcode.com/problems/sort-list/): Merge sort on linked list

Solution: see [Linked Lists](../linked-list/solutions.md#lc-148).

---

<a id="lc-23"></a>
:heavy_check_mark: :closed_book: [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/): Merge sort on linked lists

```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:

        if not lists:
            return None
        if len(lists) == 1:
            return lists[0]

        mid = len(lists) // 2

        l = self.mergeKLists(lists[:mid])
        r = self.mergeKLists(lists[mid:])

        return self.merge(l, r)

    def merge(self, left, right):

        if not left:
            return right
        if not right:
            return left

        dummy = p = ListNode()

        while left and right:
            if left.val < right.val:
                p.next = left
                left = left.next
            else:
                p.next = right
                right = right.next
            p = p.next

        if left:
            p.next = left
        if right:
            p.next = right

        return dummy.next
```

---

<a id="lc-315"></a>
:wavy_dash: :closed_book: [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/): Merge sort

```python
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

<a id="lc-493"></a>
:wavy_dash: :closed_book: [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/): Merge sort

```python
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

## 3. Sorting as a First Step

<a id="lc-56"></a>
:heavy_check_mark: :orange_book: [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/): sort by start; merge while start <= last end

```python
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
