# Heaps & Priority Queues · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Heap Problems

<a id="lc-347"></a>
:heavy_check_mark: :orange_book: [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/): Counter, then pop k items from a max-heap of (-freq, num)

```python
class Solution(object):
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """

        # Solution using Max Heap (https://leetcode.com/problems/top-k-frequent-elements/discuss/1502514/C%2B%2BPython-2-solutions%3A-MaxHeap-Bucket-Sort-Clean-and-Concise)

        # Time: O(N + KlogN), heapify = O(N), heappop for k times = O(K logN)
        cnt = Counter(nums)
        maxHeap = [[-freq, num] for num, freq in cnt.items()]
        heapify(maxHeap)

        result = []
        for i in range(k):
            _, num = heappop(maxHeap) # popping the element with the smallest value at first position
            result.append(num)

        return result
```

---

<a id="lc-295"></a>
:heavy_check_mark: :closed_book: [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/): max-heap for the smaller half, min-heap for the larger half, keep their sizes balanced

```python
class MedianFinder:

    # Ref: https://www.youtube.com/watch?v=itmhHWaHupI

    def __init__(self):

        # small heap is a max heap, all elements <= elements in large heap
        self.small = []

        # large heap is a min heap, all elements >= elements in small heap
        self.large = []

    def addNum(self, num: int) -> None:

        # by default add to small heap
        # since Python heap is min heap by default, and we want max heap, we negate the added number
        heapq.heappush(self.small, -1 * num)

        # make sure every element in small heap <= elements in large heap
        if self.small and self.large and (-1 * self.small[0]) > self.large[0]:
            # if there is some element in small heap > element in large heap
            # pop the max element from the small heap and add to large heap
            val = heapq.heappop(self.small)
            heapq.heappush(self.large, -1 * val)

        # make sure the sizes of the two heaps are approximately equal
        if len(self.small) > len(self.large) + 1:
            # if size of small heap is more than one element larger than large heap
            # pop the max element from the small heap and add to large heap
            val = heapq.heappop(self.small)
            heapq.heappush(self.large, -1 * val)

        if len(self.large) > len(self.small) + 1:
            # if size of large heap is more than one element larger than small heap
            # pop the min element from the large heap and add to small heap
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -1 * val)

    def findMedian(self) -> float:

        # see if the length is odd, return the max of small heap
        if len(self.small) > len(self.large):
            return -1 * self.small[0]

        # or the min of large heap
        if len(self.large) > len(self.small):
            return self.large[0]

        # otherwise, the length is even, return mean of the middle two numbers
        return (-1 * self.small[0] + self.large[0]) / 2
```

---

<a id="lc-215"></a>
:heavy_check_mark: :orange_book: [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/): quick select with a random pivot, O(n) on average

Solution: see [Sorting](../sorting/solutions.md#lc-215).

---
