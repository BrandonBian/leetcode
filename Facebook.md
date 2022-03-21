# [Facebook] Interview Coding Questions
## As collected from the LeetCode community

### [Sum To 100](https://leetcode.com/discuss/interview-question/357345/) (Related: DFS/Backtracking)

![sum_100](figures/sum_100.png)

```
# Reference Solution:
# https://leetcode.com/discuss/interview-question/357345/Facebook-or-Phone-Screen-or-Sum-to-100/323572

def dfs(candidates, target, path, res):

    print("Path: " + str(path))
    print("Candidates: " + str(candidates))
    print("Result: " + str(res))
    print('')
    
    if target == 10 and len(candidates) == 0:
        res.append(path)
        return

    for i in range(len(candidates)):
        # Note the candidates[i+1:] here - we only consider candidates AFTER current node
        # candidates[:i+1] -> path to current node (all selections BEFORE and INCLUDING current node)
        # candidates[i+1:] -> all possible selections AFTER current node
        
        dfs(candidates[i+1:], target + int(candidates[:i+1]), path + '+' + candidates[:i+1], res)
        dfs(candidates[i+1:], target - int(candidates[:i+1]), path + '-' + candidates[:i+1], res)
        
res = []
candidates = "1234"
dfs(candidates, 0, "", res)

for i in range(len(res)):
    print(res[i])
```
