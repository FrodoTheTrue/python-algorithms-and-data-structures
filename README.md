# Python + Algorithms + Data Structures

Fast and helpful examples of python code to write algorithms

## Table of content
- [Basics]()
    - [Bit operations]()
- [Data structures]()
    - [PriorityQueue]()
- [Algorithms]()
    - [Djkstra algorithm with heap]()
    - [Segment tree](#segment-tree)
    - [Trie]()
    - [Binary Search in Python]()
    - [Sieve of Eratosthenes]()
    - [z-function + substring search]()
    - [Rabin-Karp alrorithm]()
    - [Backtracking template]()
    - [Sliding window template]()
    - [Union find (disjoint set union)]()


Segment tree
------------

```python
class TreeNode:
    def __init__(self, start, end, sum=0, left=None, right=None):
        self.start = start
        self.end = end
        self.sum = sum
        self.left = left
        self.right = right
```
