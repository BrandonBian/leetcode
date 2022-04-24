# Two Pointers | [[Selected Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/two-pointers.md)]
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [Full List](https://leetcode.com/tag/two-pointers/)
- **Reference 1**: [Sliding Window Guide](https://leetcode.com/tag/two-pointers/discuss/1122776/Summary-of-Sliding-Window-Patterns-for-Subarray-Substring)
- **Reference 2**: [When to use Two Pointers](https://leetcode.com/problems/subarray-sum-equals-k/discuss/301242/General-summary-of-what-kind-of-problem-can-cannot-solved-by-Two-Pointers)

## Relevant Concepts

### - Characteristics of problems that can be solved with Two Pointers:
  - "If a wider scope of the sliding window is valid, the narrower scope of that wider scope is valid" must hold.
  - "If a narrower scope of the sliding window is invalid, the wider scope of that narrower scope is invalid" must hold.

### - Types of Two Pointers: (see problems list below for more examples)
  - **Face-to-Face**: usually for problems of types "reverse, two sum, partition"
  - **Back-to-Back**: usually for problems of types "palindrome substring, find k-closest elements"
  - **Same-Direction**: usually for problems of types "sliding window, fast & slow pointers"

### - Idea of [Sliding Window]: 


## Typical Examples & Concepts
### "Face-to-Face -> 2 Sum": :heavy_check_mark: :orange_book: [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```
# Goal: find the location of two elements that sum to target
# two pointers starting from either end of array and going towards the middle

left_ptr, right_ptr = 0, len(numbers) - 1

while left_ptr < right_ptr:

    two_sum = numbers[left_ptr] + numbers[right_ptr]

    if two_sum == target:
        return [left_ptr + 1, right_ptr + 1] # + 1 because the question requires the index to + 1

    elif two_sum < target:
        left_ptr += 1

    else:
        right_ptr -= 1
```

---

### "Face-to-Face -> 4 Sum -> reduce to 2 Sum & REMOVING DUPLICATES": :heavy_check_mark: :orange_book: [18. 4Sum](https://leetcode.com/problems/4sum/)
- **There is also a K-Sum Template**: [Link](https://leetcode.com/problems/4sum/discuss/8545/Python-140ms-beats-100-and-works-for-N-sum-(Ngreater2)/185194) (basically reducing K-Sum to 2-Sum with removing duplicates, but with better organization)
```
# idea: fix first 2 elements, reduce to 2 Sum problem, then 2 Sum on the rest

def find_two_sum(left, right, target, result, num1, num2):

    while left < right:
        two_sum = nums[left] + nums[right]

        if two_sum < target:
            left += 1
        elif two_sum > target:
            right -= 1
        else:
            result.append([num1, num2, nums[left], nums[right]])

            left += 1
            right -= 1

            # skip duplicates
            while left < right and nums[left] == nums[left - 1]: # already visited in prior loop
                left += 1
            while left < right and nums[right] == nums[right + 1]: # already visited in prior loop
                right -= 1

nums.sort() # don't forget to sort first! 
length = len(nums)
result = []

if len(nums) < 4:
    return None

for i in range(0, len(nums) - 3): # fix the first (least) element

    if i > 0 and nums[i] == nums[i - 1]: # already visited in prior loop, skip
        continue

    for j in range(i + 1, len(nums) - 2): # fix the second (least) element

        if j > i + 1 and nums[j] == nums[j - 1]: # already visited in prior loop, skip
            continue

        # search space: two pointers from the element next to this element to the last
        left, right = j + 1, len(nums) - 1

        find_two_sum(left, right, target - nums[i] - nums[j], result, nums[i], nums[j])

return result
```

---

### "Face-to-Face -> Partition": :heavy_check_mark: :orange_book: [LintCode 31 · Partition Array](https://www.lintcode.com/problem/31/)

```
# Goal: partition array such that all elements < k are moved to left, >= k moved to right, return partition index

def partition_array(self, nums: List[int], k: int) -> int:

    if not nums:
        return 0

    # two pointers starting from either end of array
    left, right = 0, len(nums) - 1

    while left <= right:

        # find the numbers that are misplaced on either side
        while left <= right and nums[left] < k: 
            # stop when we found a nums[left] >= k, it is misplaced
            left += 1
        while left <= right and nums[right] >= k: 
            # stop when we found a nums[right] < k, it is misplaced
            right -= 1

        # exchange
        if left <= right:
            nums[left], nums[right] = nums[right], nums[left]

            left += 1
            right -= 1

    return left # index of the first element on the right partition
```

---
