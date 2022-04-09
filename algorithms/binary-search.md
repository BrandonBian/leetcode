# Binary Search
* **Note**: 
  * :heavy_check_mark: means **MUST DO (i.e., very important, typical, or good) problems** that should definitely be familiar with
  * :wavy_dash: means problems that are less typical
  * :green_book: means **EASY problems** as defined by LeetCode
  * :orange_book: means **MEDIUM problems** as defined by LeetCode
  * :closed_book: means **HARD problems** as defined by LeetCode
  * The brackets after each LeetCode problem: summarizes **relevant keypoints / algorithms** used in solving that problem

- **LeetCode Problems**: [Full List](https://leetcode.com/tag/binary-search)
- **Reference 1**: [Study Guide - Most Upvoted One](https://leetcode.com/tag/binary-search/discuss/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)
- **Reference 2**: [Study Guide - Solving "K-th" Type of Problems](https://leetcode.com/tag/binary-search/discuss/1529866/Solving-kth-kind-of-problems)
- **Reference 3**: [Study Guide - Approach to Writing Bug-free Binary Search Code](https://leetcode.com/tag/binary-search/discuss/1089533/An-approach-to-writing-bug-free-Binary-Search-code)
- **Selected LeetCode Problems**: [List](https://leetcode.com/list/xls4oirv/)


## - General Binary Search Ideas
- **Conceptually**: Basically, it splits the search space into two halves and only keep the half that probably has the search target and throw away the other half that would not possibly have the answer. In this manner, we reduce the search space to half the size at every step, until we find the target. Binary Search helps us reduce the search time from linear O(n) to logarithmic **O(log n)**.


## - Backtracking Problem Solving Templates

- **General Backtracking Questions and Template**: [link-1](https://leetcode.com/problems/combination-sum/discuss/429538/General-Backtracking-questions-solutions-in-Python-for-reference-%3A), [link-2](https://leetcode.com/problems/letter-combinations-of-a-phone-number/discuss/780232/Backtracking-Python-problems%2B-solutions-interview-prep)

- **General Idea**: 
1. Transform problem into a **tree problem**
2. Use "candidates" to track the set of leaf nodes under current node
3. Use "path" to track the path taken to reach the current node in the tree
4. Use "res" to save all the paths.
5. Use "target" to keep track of current progress and the limits
6. To **remove duplicates**: sort the input in ascending order, and check candidates with for loop to skip duplicate subtrees (see below)

```
# Base Function

def combine(n, target):
    res = []
    candidates = range(1, n+1) # Or the array of input numbers as given in the question
    # To remove duplicates, add this: candidates.sort()
    dfs(candidates, target, 0, [], res)
    return res
    
########################################################################################################    
# Combinations: combinations of list [candidates] elements that form a size [target] window (See LeetCode 77)
# All elements UNIQUE

def dfs(candidates, target, path, res):
    # candidates: leaf nodes under current node
    # target: our restriction (that the size of considered nodes should be equal to [target])
    # path: current path down the entire tree
    # res: to save all the paths
    
    if target < 0:  # backtracking WITHOUT appending the path to res (we went too far) 
        return 
        
    if target == 0:
        res.append(path)
        return # backtracking AFTER appending path to res (we met target)
        
    for i in range(len(candidates)):
        # Note the candidates[i+1:] here - we only consider candidates AFTER current node
        # effect: remove duplicates ([3,5] = [5,3]), and to remove itself ([3,3])
        # since nodes [:i] prior to i are already considered when traversing trees with root = prior nodes
        # also we don't want selections like [3,3] because that is combination of an element with itself, invalid
        dfs(candidates[i+1:], target-1, path+[candidates[i]], res)
        
########################################################################################################
# Combination Sum: UNIQUE combinations of list [candidates] that sum up to [target] (See LeetCode 39)
# All elements UNIQUE, each number can be used unlimited times

def dfs(candidates, target, path, res):
    # candidates: leaf nodes under current node
    # target: our restriction (that the sum should be equal to [target])
    # path: current path down the entire tree
    # res: to save all the paths
    
    if target < 0:  # backtracking WITHOUT appending the path to res (we went too far)
        return  
        
    if target == 0:
        res.append(path)
        return # backtracking AFTER appending path to res (we met target)
        
    for i in range(len(candidates)):
        # Note the candidates[i:] here - we don't consider candidates prior to current node, but we can reuse current number
        # since those combinations / paths are already considered when traversing trees with root = prior nodes
        # to eliminate repetition, since we want UNIQUE combinations
        # which enforces each combination of candidates to run once (e.g., [3,5] = [5,3])
        dfs(candidates[i:], target-candidates[i], path+[candidates[i]], res)  

########################################################################################################
# Permutations: all permutations of list [candidates] (See LeetCode 46)
# All elements UNIQUE

def dfs(self, candidates, path, res):
    # candidates: leaf nodes under current node
    # path: current path down the entire tree
    # res: to save all the paths
    # Note that we don't need a [target] to restrict the results
    
    if not candidates:
        res.append(path)
        return # backtracking
        
    for i in range(len(candidates)):
        # candidates[:i] + candidates[i+1:] -> all candidates but leaving out the i-th element
        # because we already included it into our path: path+[candidates[i]]
        self.dfs(candidates[:i]+candidates[i+1:], path+[candidates[i]], res)

########################################################################################################

# If we have NON-UNIQUE candidates, REMOVE DUPLICATES by inserting the following code inside the for loop and before self.dfs():
# "if i > 0 and candidates[i] == candidates[i-1]: continue" to skip duplicate subtrees
# ALSO: don't forget to SORT the candidates in ascending order before running the dfs (see the base function above)
```


## - Selected LeetCode Core Problems:

- **UTMOST MUST DO Problems - Base Problems**:

:heavy_check_mark: :orange_book: [77. Combinations](https://leetcode.com/problems/combinations/): (**Backtracking for Combinations**)

:heavy_check_mark: :orange_book: [39. Combination Sum](https://leetcode.com/problems/combination-sum/): (**Backtracking for Combination Sum**)

:heavy_check_mark: :orange_book: [46. Permutations](https://leetcode.com/problems/permutations/): (**Backtracking for Permutations**)

:heavy_check_mark: :orange_book: [78. Subsets](https://leetcode.com/problems/subsets/): running combinations for lengths from [0, len(nums)] (**Backtracking for Subsets / Power Set**)

---

- **Variation of Base Problems**:

:heavy_check_mark: :orange_book: [47. Permutations II](https://leetcode.com/problems/permutations-ii/): elements in given number array may not be unique, need to **remove duplicates**

:heavy_check_mark: :orange_book: [90. Subsets II](https://leetcode.com/problems/subsets-ii/): elements in given number array may not be unique, need to **remove duplicates**

:heavy_check_mark: :orange_book: [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/): need to **remove duplicates**, each number can only be used once

:heavy_check_mark: :orange_book: [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/): fixed length, each number can only be used once

---

- **Harder & More Varied Core Problems**:

:heavy_check_mark: :orange_book: [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

:heavy_check_mark: :orange_book: [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

:heavy_check_mark: :orange_book: [79. Word Search](https://leetcode.com/problems/word-search/)

:heavy_check_mark: :orange_book: [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/): a similar method as the DFS template above

:heavy_check_mark: :closed_book: [51. N-Queens](https://leetcode.com/problems/n-queens/)

:heavy_check_mark: :closed_book: [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

---

- **Other Problems**:

