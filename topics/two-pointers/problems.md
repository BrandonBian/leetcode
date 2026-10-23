# Two Pointers & Sliding Window · Problems

[← All topics](../../README.md) &nbsp;·&nbsp; [Guide](guide.md) &nbsp;·&nbsp; **Problems** &nbsp;·&nbsp; [Solutions](solutions.md)

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

## 1. "Face-to-Face" Two Pointers

:heavy_check_mark: :orange_book: [15. 3Sum](https://leetcode.com/problems/3sum/): sort, fixing least element and running **2 Sum** with target = -(that element), **remove duplicates** (**2 Sum**) &nbsp;→ [solution](solutions.md#lc-15)

:heavy_check_mark: :orange_book: [611. Valid Triangle Number](https://leetcode.com/problems/valid-triangle-number/): sort, fix the longest edge and run **2 Sum** on the shorter edges (count pairs with sum > longest edge) &nbsp;→ [solution](solutions.md#lc-611)

:heavy_check_mark: :orange_book: [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/): start from both ends and always move the shorter bar inwards &nbsp;→ [solution](solutions.md#lc-11)

:heavy_check_mark: :closed_book: [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/): two pointers with running left_max / right_max; the smaller side decides the water level &nbsp;→ [solution](solutions.md#lc-42)

:heavy_check_mark: :orange_book: [75. Sort Colors](https://leetcode.com/problems/sort-colors/): two partition passes (< 1, then < 2), the quick-sort partition step &nbsp;→ [solution](solutions.md#lc-75)

## 2. "Back-to-Back" Two Pointers (Expand from the Middle)

:heavy_check_mark: :orange_book: [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/): expand around every center (odd and even length) with two pointers moving outwards &nbsp;→ [solution](solutions.md#lc-5)

## 3. "Same-Direction" Two Pointers & Sliding Window

:heavy_check_mark: :green_book: [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/): one pointing to the first non-replaced element, one pointing to non-zero elements, replace / swap &nbsp;→ [solution](solutions.md#lc-283)

:heavy_check_mark: :green_book: [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/): left pointer keeps the lowest price so far, right pointer scans for the best profit &nbsp;→ [solution](solutions.md#lc-121)

:heavy_check_mark: :closed_book: [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/): sliding window with a Counter of needed chars; shrink from the left while the window is valid &nbsp;→ [solution](solutions.md#lc-76)

:heavy_check_mark: :orange_book: [763. Partition Labels](https://leetcode.com/problems/partition-labels/): record each letter's last index; cut when the current index reaches the farthest last index &nbsp;→ [solution](solutions.md#lc-763)

## 4. Other Pointer Tricks

:heavy_check_mark: :orange_book: [31. Next Permutation](https://leetcode.com/problems/next-permutation/): find the longest non-increasing suffix, swap the pivot with the rightmost larger element, reverse the suffix &nbsp;→ [solution](solutions.md#lc-31)
