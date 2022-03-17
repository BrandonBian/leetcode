## Enumerate()
### Syntax
```
enumerate(iterable, start=0)

Iterable: any object that supports iteration
Start: the index value from which the counter is to be started, by default it is 0
```
### Example
```
test = [0, 1, 2, 3, 3]

for index, value in enumerate(test):
    print("index: ", index)
    print("value: ", value)
    print("")

# Output
index:  0
value:  0

index:  1
value:  1

index:  2
value:  2

index:  3
value:  3

index:  4
value:  3
```
### Utilized In LeetCode Problem(s)
1. [Two Sum](https://leetcode.com/problems/two-sum/)
