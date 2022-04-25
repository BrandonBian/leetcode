# LintCode - Chapter by Chapter Practice Solutions | [[Problems](https://github.com/BrandonBian/LeetCode-Notes/blob/main/problems-and-solutions/LintCode/chapter-by-chapter.md)]
**All problems are LintCode problems, solved in Python.**

---

## Chapter 1: Introduction (Part I)
**No practice Problems**

---

## Chapter 2: Introduction (Part II)

:orange_book: [200 · Longest Palindromic Substring](https://www.lintcode.com/problem/200/): Enumeration from middle with two pointers / Dynamic Programming

- **Enumeration from middle with two pointers**
```
class Solution:
    """
    @param s: input string
    @return: a string as the longest palindromic substring
    """
    def longest_palindrome(self, s: str) -> str:
        if not s:
            return None

        answer = (0, 0) # (substring length, substring starting position)

        for mid in range(len(s)):
        
            # Note: max is comparing the first element of each tuple
            answer = max(answer, self.get_palindrome_from(s, mid, mid))
            answer = max(answer, self.get_palindrome_from(s, mid, mid + 1))

        # s[starting position : starting position + substring length]
        return s[answer[1] : answer[1] + answer[0]]
        
    def get_palindrome_from(self, s, left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            # two pointers move to either side
            left -= 1
            right += 1

        # right - left - 1 -> length of the palindrome substring
        # left + 1 -> index at which the palindrome substring starts
        return right - left - 1, left + 1
```

- **Dynamic Programming**

```
class Solution:
    """
    @param s: input string
    @return: a string as the longest palindromic substring
    """
    def longest_palindrome(self, s: str) -> str:
        if not s:
            return None

        n = len(s)

        # whether s[i] ~ s[j] is a palindrome substring
        is_palindrome = [[False for _ in range(n)] for _ in range(n)]

        # each single character is itself a palindrome
        for i in range(n):
            is_palindrome[i][i] = True

        # smaller than a single character is itself a palindrome (e.g., an empty string)
        for i in range(1, n):
            is_palindrome[i][i-1] = True

        start_pos, longest_length = 0, 1

        # first enumerate possible substring lengths, then enumerate start position for that length
        for length in range(2, n + 1):
            for start in range(n - length + 1):
                end = start + length - 1
                # is palindrome if two characters at either end are same and the substring in-between is palindrome
                is_palindrome[start][end] = is_palindrome[start + 1][end - 1] and s[start] == s[end]

                # if this is the longest palindrome substring we have encountered, record its start position and length
                if is_palindrome[start][end] and length > longest_length:
                    longest_length = length
                    start_pos = start

        return s[start_pos : start_pos + longest_length]
```

---

:orange_book: [667 · Longest Palindromic Subsequence](https://www.lintcode.com/problem/667/): Dynamic Programming

```
class Solution:
    """
    @param s: the maximum length of s is 1000
    @return: the longest palindromic subsequence's length
    """
    def longest_palindrome_subseq(self, s: str) -> int:
        
        # idea: Dynamic Programming

        if len(s) <= 1:
            return len(s)

        size = len(s)

        # dp[i][j] = the length of the longest palinidromic subsequence in range s[i ~ j]
        dp = [[0 for _ in range(size)] for _ in range(size)]

        # each single character is itself a palindrome of length 1
        for i in range(size):
            dp[i][i] = 1

        for i in range(size - 1, -1, -1):
            for j in range(i + 1, size):
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else: 
                    dp[i][j] = max(dp[i][j - 1], dp[i + 1][j])

        return dp[0][size - 1]
```

---

## Chapter 3: Introduction (Part III)

:green_book: [13 · Implement strStr()](https://www.lintcode.com/problem/13/): Naive for loop / Rabin-Karp

- **Naive for loop**

```
class Solution:
    """
    @param source: 
    @param target: 
    @return: return the index
    """
    def str_str(self, source: str, target: str) -> int:

        if len(source) < len(target):
            return -1

        for i in range(len(source) - len(target) + 1):
            if source[i:i+len(target)] == target:
                return i
                
        return -1
```

- **Rabin-Karp**

```
# Reference: https://www.lintcode.com/problem/13/solution/19657
# Usually it's OK to use the naive O(N^2) approach since this approach is too advanced for a normal interview
```

---

:orange_book: [187 · Gas Station](https://www.lintcode.com/problem/187/): Greedy algorithm

```
class Solution:
    """
    @param gas: An array of integers
    @param cost: An array of integers
    @return: An integer
    """
    def can_complete_circuit(self, gas: List[int], cost: List[int]) -> int:
        
        if sum(gas) < sum(cost):
            return -1
            
        index = 0
        gas_remain = 0
        
        for i in range(len(gas)): # we start at station No.0
            
            gas_remain += gas[i]-cost[i]

            if gas_remain < 0:
                gas_remain = 0 # reset to 0, indicating we are starting fresh from station No.[i+1]
                index = i+1
                
        # there is no looping back, so this solution assumes an answer exists
        # relevant proof: https://www.lintcode.com/problem/187/solution/17249
        return index
```

---

## Chapter 4: Algorithm Complexity & Two Pointers

:orange_book: [415 · Valid Palindrome](https://www.lintcode.com/problem/415/): Remove special characters, face-to-face two pointers

```
class Solution:
    """
    @param s: A string
    @return: Whether the string is a valid palindrome
    """
    def is_palindrome(self, s: str) -> bool:
        # write your code here
        if not s or len(s) == 1:
            return True

        left, right = 0, len(s) - 1

        while left < right:

            # skip the non-alphabetical characters
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1
            
            # compare the characters at both ends
            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1
        
        return True
```

---

:orange_book: [891 · Valid Palindrome II](https://www.lintcode.com/problem/891/): Face-to-face two pointers

```
class Solution:
    """
    @param s: a string
    @return: whether you can make s a palindrome by deleting at most one character
    """
    def valid_palindrome(self, s: str) -> bool:
        
        if len(s) == 1:
            return True

        # go from either end of string and find where the palindrome is broken
        left, right = 0, len(s) - 1

        while left < right:
            if s[left] != s[right]:
                break
            left += 1
            right -= 1
        
        if left >= right: # if we haven't break then s is already a palindrome
            return True

        # now [left] = starting position of where the palindrome is broken
        # now [right] = ending position of where the palindrome is broken

        # remove s[left] OR remove s[right]
        return self.is_palindrome(s, left + 1, right) or self.is_palindrome(s, left, right - 1)

    def is_palindrome(self, s, left, right):

        # see if s[left : right] is a palindrome
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        
        return True
```

---
