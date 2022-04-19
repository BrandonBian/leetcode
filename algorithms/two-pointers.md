# Two Pointers
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
  - **Face-to-Face**: for problems of types "reverse, two sum, partition"
  - **Back-to-Back**: for problems of types "palindrome substring, find k-closest elements"
  - **Same-Direction**: for problems of types "sliding window, fast & slow pointers"


## Typical Examples
### "Face-to-Face": :heavy_check_mark: :orange_book: [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```
# two pointers starting from either end of array and going towards the middle

left_ptr, right_ptr = 0, len(numbers) - 1

while left_ptr < right_ptr:

    two_sum = numbers[left_ptr] + numbers[right_ptr]

    if two_sum == target:
        return [left_ptr + 1, right_ptr + 1]

    elif two_sum < target:
        left_ptr += 1

    else:
        right_ptr -= 1

```



## Selected LeetCode Problems
### Face-to-Face Two Pointers

