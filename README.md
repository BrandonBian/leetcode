# LeetCode Notes

Study notes for coding interviews (SDE and quant developer roles): pattern guides with reusable templates, curated problem lists, and commented Python 3 solutions, organized by topic.

![Python 3](https://img.shields.io/badge/language-Python%203-3776AB?logo=python&logoColor=white)
![Problems](https://img.shields.io/badge/problems-231-orange)
![Topics](https://img.shields.io/badge/topics-14-blue)

---

## How to use this repo

1. **Learn the pattern.** Read a topic's **guide** for the idea and the template to memorize.
2. **Practice.** Work through the topic's **problem list**. Start with the :heavy_check_mark: must-do problems; each line has a one-line hint.
3. **Check your work.** Each problem links to its **solution**, with the reasoning written as comments in the code.

**Legend:** :heavy_check_mark: must-do &nbsp;·&nbsp; :wavy_dash: less typical &nbsp;·&nbsp; :green_book: Easy &nbsp;·&nbsp; :orange_book: Medium &nbsp;·&nbsp; :closed_book: Hard

---

## Topics

### Algorithms

| Topic | Guide | Problems | :green_book: | :orange_book: | :closed_book: | Template to memorize |
| --- | :-: | :-: | :-: | :-: | :-: | --- |
| **Two Pointers & Sliding Window** | [Guide](topics/two-pointers/guide.md) | [Problems](topics/two-pointers/problems.md) | 2 | 7 | 2 | [2 Sum / K-Sum / partition](topics/two-pointers/guide.md#typical-examples--concepts) |
| **Binary Search** | [Guide](topics/binary-search/guide.md) | [Problems](topics/binary-search/problems.md) | 4 | 18 | 6 | [General template](topics/binary-search/guide.md#white_check_mark-general-template---very-good-please-memorize) |
| **Sorting** | [Guide](topics/sorting/guide.md) | [Problems](topics/sorting/problems.md) | 0 | 3 | 3 | [Quick / merge / counting / heap sort](topics/sorting/guide.md#--quick-sort) |
| **Recursion & Backtracking** | [Guide](topics/backtracking/guide.md) | [Problems](topics/backtracking/problems.md) | 0 | 12 | 2 | [Combinations / permutations](topics/backtracking/guide.md#--backtracking-problem-solving-templates) |
| **Graphs** (grid DFS / BFS, topological sort) | — | [Problems](topics/graphs/problems.md) | 2 | 7 | 1 | — |
| **Dynamic Programming** | [Guide](topics/dynamic-programming/guide.md) | [Problems](topics/dynamic-programming/problems.md) | 4 | 31 | 5 | [The DP patterns](topics/dynamic-programming/guide.md) |
| **Math** (GCD & LCM) | [Guide](topics/math/guide.md) | — | — | — | — | [GCD / LCM formulae](topics/math/guide.md#formulae) |

### Data Structures

| Topic | Guide | Problems | :green_book: | :orange_book: | :closed_book: | Template to memorize |
| --- | :-: | :-: | :-: | :-: | :-: | --- |
| **Arrays & Matrices** | — | [Problems](topics/arrays-matrix/problems.md) | 0 | 6 | 0 | — |
| **Hash Tables** | [Guide](topics/hash-table/guide.md) | [Problems](topics/hash-table/problems.md) | 22 | 16 | 1 | [dict / Counter / sorting them](topics/hash-table/guide.md#--python-dictionary-and-counters-and-how-to-sort-them) |
| **Linked Lists** | [Guide](topics/linked-list/guide.md) | [Problems](topics/linked-list/problems.md) | 7 | 8 | 1 | [Slow / fast pointers](topics/linked-list/guide.md#--cycle-detection-and-finding-with-slow--fast-pointers) |
| **Stacks & Queues** | — | [Problems](topics/stack-queue/problems.md) | 2 | 2 | 4 | — |
| **Heaps & Priority Queues** | — | [Problems](topics/heap/problems.md) | 0 | 1 | 1 | — |
| **Binary Trees & BSTs** | [Guide](topics/binary-tree/guide.md) | [Problems](topics/binary-tree/problems.md) | 19 | 29 | 2 | [Traversals](topics/binary-tree/guide.md#--binary-tree-traversals-and-operations-usually-with-recursion) |
| **Tries** | — | [Problems](topics/trie/problems.md) | 0 | 1 | 0 | — |

### Curated lists

- [**LeetCode Top 100**](lists/leetcode-top-100.md): LeetCode's list of the 100 most frequently asked problems, with links to each solution in its topic folder.

---

## Pattern cheat sheet

If the problem says this, try these first.

| The problem involves… | Try | Topic |
| --- | --- | --- |
| A sorted array | Binary search, two pointers | [Binary Search](topics/binary-search/guide.md), [Two Pointers](topics/two-pointers/guide.md) |
| A minimum / maximum / k-th value that is easy to *check* | Binary search on the answer | [Binary Search](topics/binary-search/guide.md) |
| All permutations / subsets / combinations | DFS backtracking | [Backtracking](topics/backtracking/guide.md) |
| A binary tree or BST | DFS recursion, BFS with a queue | [Binary Trees](topics/binary-tree/guide.md) |
| Path finding / flooding on a grid | DFS or BFS from the cells | [Graphs](topics/graphs/problems.md) |
| Dependencies or an order of tasks | Topological sort | [Graphs](topics/graphs/problems.md) |
| A linked list | Slow / fast pointers, dummy node | [Linked Lists](topics/linked-list/guide.md) |
| K-Sum / partitioning | Two pointers | [Two Pointers](topics/two-pointers/guide.md) |
| A substring / subarray with a constraint | Sliding window | [Two Pointers](topics/two-pointers/guide.md) |
| Subarray sums | Prefix sums + hash map | [Hash Tables](topics/hash-table/problems.md) |
| Brackets or nested structure | Stack | [Stacks & Queues](topics/stack-queue/problems.md) |
| Next greater / smaller element, window maximum | Monotonic stack / deque | [Stacks & Queues](topics/stack-queue/problems.md) |
| Top k / running median | Heap | [Heaps](topics/heap/problems.md) |
| Counting things / matching a pattern | Hash table (`dict`, `Counter`) | [Hash Tables](topics/hash-table/guide.md) |
| Prefix lookups on many words | Trie | [Tries](topics/trie/problems.md) |
| Max / min / count over choices, subarrays or subsequences | Dynamic programming | [DP](topics/dynamic-programming/guide.md) |

Further reading: [LeetCode Patterns](https://seanprashad.com/leetcode-patterns/) · [14 Patterns to Ace Any Coding Interview](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)

<details>
<summary><b>Python time complexity quick reference</b></summary>

Average-case costs in CPython.

| Structure | Operation | Cost |
| --- | --- | --- |
| `list` | index, `append`, `pop()` | O(1) |
| `list` | `insert(0, x)`, `pop(0)`, `x in lst`, slicing | O(n) |
| `list` | `sort()`, `sorted()` | O(n log n) |
| `dict` / `set` | get, set, delete, `x in d` | O(1) |
| `collections.deque` | `append`, `appendleft`, `pop`, `popleft` | O(1) |
| `heapq` | `heappush`, `heappop` | O(log n) |
| `heapq` | `heapify` | O(n) |
| `heapq` | `nlargest(k, …)`, `nsmallest(k, …)` | O(n log k) |
| `bisect` | `bisect_left`, `bisect_right` | O(log n) |
| `bisect` | `insort` | O(n) |
| `str` | `+=` in a loop | O(n²) total, use `''.join(parts)` |

</details>

---

## Resources

- [Competitive Programmer's Handbook](resources/books/Competitive%20Programmer's%20Handbook.pdf), Antti Laaksonen
- [LeetCode 101: A LeetCode Grinding Guide (C++ Version)](resources/books/LeetCode%20101%20-%20A%20LeetCode%20Grinding%20Guide%20%28C++%20Version%29.pdf), Chang Gao

## Roadmap

- [ ] Fix known bugs in a few solutions and move all code to the Python 3 style (`class Solution:` with type hints)
- [ ] Write guides for the topics that only have problem lists (graphs, stacks & queues, heaps, tries, arrays)
- [ ] New topics: intervals, greedy, bit manipulation, union-find, Fenwick tree
- [ ] Math & probability for quant developer interviews
- [ ] C++ solutions
