# LeetCode - Two Pointers | Problems
* **Notations**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode

---

## Two Pointers Problems | [[Study Guide](https://github.com/BrandonBian/LeetCode-Notes/blob/main/algorithms/two-pointers.md)] | [[Solutions](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LeetCode/two-pointers-solutions.md)]

### - "Face-to-Face" Two Pointers

:heavy_check_mark: :orange_book: [15. 3Sum](https://leetcode.com/problems/3sum/): sort, fixing least element and running **2 Sum** with target = -(that element), **remove duplicates** (**2 Sum**)

:heavy_check_mark: :orange_book: [LintCode 382 · Triangle Count](https://www.lintcode.com/problem/382/): sort, fixing the longest edge and running **2 Sum** on short edges with target > longest edge (**2 Sum**)

:heavy_check_mark: :orange_book: [LintCode 144 · Interleaving Positive and Negative Numbers](https://www.lintcode.com/problem/144/): **partition** numbers of different signs, then interleave (**Partition**)

:wavy_dash: :orange_book: [LintCode 148 · Sort Colors](https://www.lintcode.com/problem/148/): using **quick sort - partition** (**Partition**)

:wavy_dash: :orange_book: [LintCode 143 · Sort Colors II](https://www.lintcode.com/problem/143/): sorting arbitrary number of colors, using **quick sort - partition & recursion** (**Partition & Recursion**)

---

### - "Same-Direction" Two Pointers
 
:heavy_check_mark: :green_book: [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/): one pointing to the first non-replaced element, one pointing to non-zero elements, replace / swap
