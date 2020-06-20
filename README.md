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
    def __init__(
        self,
        start: int,
        end: int,
        summ: int = 0,
        left=None,
        right=None,
    ):
        self.start = start
        self.end = end
        self.summ = summ
        self.left = left
        self.right = right


class SegmentTree:

    def __init__(self, arr: List[int]):
        self._arr = arr
        self.root = self._build(0, len(self._arr) - 1)

    def update(self, i: int, val: int) -> None:
        self._update(self.root, i, val)

    def sum_range(self, i: int, j: int) -> int:
        return self._range_query(self.root, i, j)

    def _build(self, start, end) -> TreeNode:
        if end < start:
            return

        if start == end:
            return TreeNode(start, end, self._arr[start])

        mid = (start + end) // 2

        left = self._build(start, mid)
        right = self._build(mid + 1, end)

        return TreeNode(start, end, left.summ + right.summ, left, right)

    def _range_query(self, node: TreeNode, start: int, end: int) -> int:
        if node.start == start and node.end == end:
            return node.summ

        mid = (node.start + node.end) // 2

        if end <= mid:
            return self._range_query(node.left, start, end)

        if start > mid:
            return self._range_query(node.right, start, end)

        return self._range_query(node.left, start, mid) + self._range_query(node.right, mid + 1, end)

    def _update(self, node: TreeNode, index: int, value: int):
        if node.start == node.end == index:
            node.summ = value
            return

        mid = (node.start + node.end) // 2

        if index <= mid:
            self._update(node.left, index, value)
        else:
            self._update(node.right, index, value)

        node.sum = node.left.summ + node.right.summ


arr = [1, 2, 3, 4, 5]

tree = SegmentTree(arr)

print(tree.sum_range(0, 3))  # 10
print(tree.sum_range(1, 4))  # 14

```
