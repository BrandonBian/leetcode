# [Facebook] Interview Coding Questions
## As collected from the LeetCode community

### [Sum To 100](https://leetcode.com/discuss/interview-question/357345/)

```
# Reference Solution:
# https://leetcode.com/discuss/interview-question/357345/Facebook-or-Phone-Screen-or-Sum-to-100/323572

class Solution:
    def __init__(self):
        self.ans =[]
    def dfs(self,s,ans,path=""):
        if len(s)==0 and ans==100:
            self.ans.append(path)
        for i in range(len(s)):
            self.dfs(s[i+1:],ans+int(s[:i+1]),path+"+"+s[:i+1])
            self.dfs(s[i+1:],ans-int(s[:i+1]),path+"-"+s[:i+1])

a = Solution()
a.dfs("123456789",0)
for i in a.ans:
    print(i)
```
