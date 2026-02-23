# Arrays

> Contiguous blocks of memory holding elements of the same type. The backbone of every DSA interview.

---

## 01 — What is an Array?

`Memory layout · indexing · Python lists`

An array stores elements in **contiguous memory**. Each element has an index starting at `0`. Python's **list** is a dynamic array — it can grow/shrink automatically.

```
Array: [10, 20, 30, 40, 50]
        ↑                ↑
      idx 0           idx 4 / idx -1
```

```python
# Creating arrays (lists in Python)
arr = [10, 20, 30, 40, 50]

# Indexing — O(1)
print(arr[0])    # 10  (first)
print(arr[-1])   # 50  (last)
print(arr[1:4])  # [20, 30, 40]  slicing

# Common operations
arr.append(60)   # O(1) amortized
arr.pop()        # O(1) — removes last
arr.insert(0, 5) # O(n) — shifts elements!
arr.pop(0)       # O(n) — shifts elements!

# Length
n = len(arr)     # O(1)
```

### Complexity

| Operation | Complexity |
|---|---|
| Access | O(1) |
| Search | O(n) |
| Append | O(1)* amortized |
| Insert at index 0 | O(n) |
| Space | O(n) |

---

## 02 — Traversal Patterns

`for loops · enumerate · zip · reversed`

Knowing **which loop style** to use matters. Python gives you elegant ways to traverse that eliminate off-by-one errors and make code more readable.

```python
arr = ['a', 'b', 'c', 'd']

# 1. Basic forward
for x in arr:
    print(x)

# 2. With index (prefer over range(len()))
for i, x in enumerate(arr):
    print(i, x)     # 0 a, 1 b ...

# 3. Reverse traversal
for x in reversed(arr):
    print(x)        # d, c, b, a

# 4. Two arrays together
a = [1, 2, 3]; b = [4, 5, 6]
for x, y in zip(a, b):
    print(x + y)    # 5, 7, 9

# 5. List comprehension (Pythonic)
squares = [x**2 for x in arr if x > 1]
```

---

## 03 — Sorting & Searching

`sort · sorted · bisect · binary search`

Python's built-in sort uses **Timsort** (O(n log n)), stable and highly optimized. For sorted arrays, use *binary search* via the **bisect** module for O(log n) lookup.

```python
import bisect

arr = [3, 1, 4, 1, 5, 9, 2]

# Sort in-place — O(n log n)
arr.sort()                        # [1,1,2,3,4,5,9]
arr.sort(reverse=True)            # descending

# Sort by key (doesn't modify original)
words = ['banana', 'apple', 'fig']
sorted(words, key=len)            # by length

# Binary search — O(log n)
arr = [1, 3, 5, 7, 9]
idx = bisect.bisect_left(arr, 5)  # 2

# Manual binary search
def binary_search(arr, target):
    l, r = 0, len(arr) - 1
    while l <= r:
        mid = (l + r) // 2
        if arr[mid] == target: return mid
        elif arr[mid] < target: l = mid + 1
        else: r = mid - 1
    return -1
```

### Complexity

| Operation | Complexity |
|---|---|
| sort() | O(n log n) |
| Binary Search | O(log n) |
| Linear Search | O(n) |

---

## 04 — 2D Arrays & Matrices

`grid traversal · row/col operations`

A 2D array is a list of lists. Essential for **grid problems**, dynamic programming tables, and image processing. Know how to traverse rows, columns, and diagonals.

```python
# Create 3×3 matrix
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Access: matrix[row][col]
print(matrix[1][2])   # 6

# Traverse all elements
for row in matrix:
    for val in row:
        print(val)

# Create m×n grid of zeros
m, n = 3, 4
grid = [[0] * n for _ in range(m)]

# 4-directional neighbors (common in grid BFS)
dirs = [(0,1),(0,-1),(1,0),(-1,0)]
for dr, dc in dirs:
    nr, nc = r + dr, c + dc
    if 0 <= nr < m and 0 <= nc < n:
        pass  # valid neighbor
```

---

## 05 — Kadane's Algorithm

`Maximum subarray sum · classic DP`

Find the contiguous subarray with the **largest sum** in O(n). The key insight: at each element, decide to *start fresh* or *extend the current subarray*.

```python
def max_subarray(nums):
    # curr = best ending here, best = global max
    curr = best = nums[0]
    for n in nums[1:]:
        curr = max(n, curr + n)  # extend or restart
        best = max(best, curr)
    return best

# Example
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
print(max_subarray(nums))  # 6 → [4, -1, 2, 1]
```

### Why `max(n, curr + n)`?

| Condition | Decision |
|---|---|
| `curr + n >= n` | Extend the existing subarray |
| `curr + n < n` | Start a fresh subarray at `n` |

If `curr` is dragging the sum down (negative), it's better to discard it and start fresh.

### Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
