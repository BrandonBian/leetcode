# Sorting Methods
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [Full List](https://leetcode.com/problemset/all/?topicSlugs=sorting&page=1)
- **Reference 1**: [Sliding Window Guide](https://leetcode.com/tag/two-pointers/discuss/1122776/Summary-of-Sliding-Window-Patterns-for-Subarray-Substring)
- **Reference 2**: [When to use Two Pointers](https://leetcode.com/problems/subarray-sum-equals-k/discuss/301242/General-summary-of-what-kind-of-problem-can-cannot-solved-by-Two-Pointers)

---

## Relevant Concepts

- Example Problem: [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)

### - Quick Sort

```
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        
        # Quick Sort:
        
        # Picks an element (leftmost element) as pivot and partitions the given array around the pivot
        # YouTube: https://youtu.be/7h1s2SojIRw
        
        def partition(array, low, high):
            
            l, r = low, high
            pivot = array[low]
            l += 1
            
            while l <= r:

                if array[l] > pivot and array[r] < pivot:
                    array[l], array[r] = array[r], array[l]
                    
                if not array[l] > pivot: # find first occurrence from left > pivot
                    l += 1
                if not array[r] < pivot: # find first occurrence from right < pivot
                    r -= 1           
            
            # now r is at the left of l, swap pivot with array[h]
            array[low], array[r] = array[r], array[low]
            
            return r # return position from where partition is done

        def quick_sort(array, low, high):
            
            if low < high:
                pi = partition(array, low, high)
                quick_sort(array, low, pi - 1)
                quick_sort(array, pi + 1, high)
        
        quick_sort(nums, 0, len(nums) - 1)
        
        return nums
```

---


### - Merge Sort
