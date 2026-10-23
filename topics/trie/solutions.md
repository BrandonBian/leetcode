# Tries (Prefix Trees) · Solutions

[← All topics](../../README.md) &nbsp;·&nbsp; [Problems](problems.md) &nbsp;·&nbsp; **Solutions**

:heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

All solutions are in Python 3.

## 1. Trie Problems

<a id="lc-208"></a>
:heavy_check_mark: :orange_book: [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/): each node holds a `children` dict and an `end_of_word` flag

```python
class Trie:

    # Ref: https://www.youtube.com/watch?v=AXjmTQ8LEoI

    def __init__(self):

        # <key, val> = <character, Trie Node>
        # Note: defaultdict will create any items that you try to access (if it is not already present)
        self.children = defaultdict(Trie)
        self.end_of_word = False

    def insert(self, word: str) -> None:
        cur = self
        for c in word:
            cur = cur.children[c]
        cur.end_of_word = True

    def search(self, word: str) -> bool:
        cur = self
        for c in word:
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return cur.end_of_word

    def startsWith(self, prefix: str) -> bool:
        cur = self
        for c in prefix:
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return True
```

---
