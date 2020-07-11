# Python + Algorithms + Data Structures

Fast and helpful examples of python code to write algorithms

## Table of content
- [Basics]()
    - [Sorting in Python](#sorting)
    - [Bit operations](#bit-operations)
    - [Permutations](#permutations)
- [Data structures]()
    - [Priority queue](#priority-queue)
    - [Segment tree](#segment-tree)
    - [Trie]()
    - [Union find (disjoint set union)]()
- [Algorithms]()
    - [Djkstra algorithm with heap]()
    - [Binary Search in Python]()
    - [Sieve of Eratosthenes]()
    - [z-function + substring search]()
    - [Rabin-Karp alrorithm]()
    - [Backtracking template]()
    - [Sliding window template]()
- [Templates]()
    - [Backtracking template](#backtracking-template)
    - [Sliding window template](#sliding-window-template)

Priority Queue
-------------
```python
from queue import PriorityQueue  

q = PriorityQueue()

# insert into queue
q.put((2, 'ex1’)) 
q.put((1, 'ex2’)) 

# remove and return 
# lowest priority item
print(q.get()) # (1, 'ex2')

# check queue size
print('Items in queue :', q.qsize())
  
# check if queue is empty
print('Is queue empty :', q.empty())

```
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

Backtracking template
-------------
Generate all subsets (without duplicates):
Input: nums = `[1,2,3]`
Output:
`
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
`

```python
def subsets(self, nums: List[int]) -> List[List[int]]:
    result = []

    def backtracking(current, index):
        result.append(current)

        for i in range(index, len(nums)): 
            backtracking(current + [nums[i]], i + 1) # generate next possible subset

    backtracking([], 0) # start generation

    return result

```

Generate all subsets (with duplicates):
Input: nums = `[1,2,2]`
Output:
`
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
`

```python
def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
    nums.sort()

    result = []

    def backtracking(current, index):
        result.append(current)

        for i in range(index, len(nums)):
            if i > index and nums[i] == nums[i - 1]:
                continue

            backtracking(current + [nums[i]], i + 1)

    backtracking([], 0)

    return result
```
Sliding window template
-------------
Given two strings s1 and s2, write a function to return true if s2 contains the permutation of s1. 
In other words, one of the first string's permutations is the substring of the second string.

```python
def checkInclusion(self, s1: str, s2: str) -> bool:
    d = {}
    counter = 0
    
    # precalculation
    
    for s in s1:
        if s not in d:
            counter += 1
            d[s] = 1
        else:
            d[s] += 1

    # window is [i - len(s1), i]
    for i in range(0, len(s2)): # move right border
        if s2[i] in d:
            d[s2[i]] -= 1

            if d[s2[i]] == 0:
                counter -= 1 # special counter for task


        if i >= len(s1):
            if s2[i - len(s1)] in d: # move left border
                if d[s2[i - len(s1)]] == 0:
                    counter += 1 
                d[s2[i - len(s1)]] += 1

        if counter == 0:
            return True

    return False
```
