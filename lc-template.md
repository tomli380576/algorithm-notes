---
order: 100
icon: project-template
---

# Ready-to-go LeetCode templates

## Basic Utilities

### Count character frequency

```py
def countFreq(s: str) -> dict[str, int]:
    frequency = {} # key is character, value is frequency count
    for char in s:
        frequency[char] = frequency.get(char, 0) + 1
    return frequency
```

### Binary Search

Returns -1 if `target` is not found

```py
def binSearch(arr: list[int], target: int) -> int:
    if len(arr) == 0:
        return -1

    l, r = 0, len(arr) - 1

    while l <= r:
        mid = (l + r) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] > target:
            r = mid - 1
        else:
            l = mid + 1

    return -1
```

## Whatever first search

### Adjacency List

Suppose the graph is stored as

```py
type Graph = dict[str, list[str]]
```

+++ Iterative DFS & BFS

||| BFS

```py !#10
from collections import deque

def bfs(G: dict[str, list[str]], start: str):
    queue = deque()
    seen = set[str]()

    queue.append(start)

    while len(queue) > 0:
        u = queue.popleft()
        if u in seen:
            continue

        # process u here
        seen.add(u)

        for adj in G[u]:
            queue.append(adj)
```

||| DFS

```py !#10
from collections import deque

def dfs(G: dict[str, list[str]], start: str):
    stack = deque()
    seen = set[str]()

    stack.append(start)

    while len(stack) > 0:
        u = stack.pop()
        if u in seen:
            continue

        # process u here
        seen.add(u)

        for adj in G[u]:
            stack.append(adj)

```

|||

+++ Recursive DFS

```py
def dfs(G: dict[str, list[str]], start: str):
    seen = set[str]()

    def dfsVisit(G: dict[str, list[str]], curr: str):
        # Pre-process

        seen.add(curr)
        for adjacent in G[curr]:
            if adjacent not in seen:
                dfsVisit(G, adjacent)

        # Post-process

    for vertex in G:
        # handle disconnected graphs
        if vertex not in seen:
            dfsVisit(G, vertex)
```

+++ Best First

This assumes min-heap (elements with numerically smaller priority is popped first). Negate the priority value if max-heap is needed.

```py
import heapq # min heap
from typing import Callable

type PriorityFn = Callable[[str, str], int | float]

def bestFirst(
    G: dict[str, list[str]], start: str, priority: PriorityFn
):
    queue = []
    seen = set()

    heapq.heappush(queue, (0, start))

    while len(queue) > 0:
        _, u = heapq.heappop(queue)
        if u in seen:
            continue

        print(u)
        seen.add(u)

        for adj in G[u]:
            heapq.heappush(queue, (priority(u, adj), adj))
```

+++

### Grid

For simplicity we assume the grid is not empty and the first element is a list, i.e. an empty grid is `[[]]`. Validate the grid if necessary.

+++ Iterative BFS & DFS

```py
from collections import deque

def bfsGrid(grid: list[list[int]], start: tuple[int, int]):
    R = len(grid)
    C = len(grid[0])

    # add more directions here if needed
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    seen = set()
    queue = deque()

    queue.append(start)

    while len(queue) > 0:
        r, c = queue.popleft() # use .pop() for dfs

        if (r, c) in seen:
            continue
        seen.add((r, c))

        # process grid[r][c] here

        for dr, dc in directions:
            nr = r + dr # new r
            nc = c + dc # new c
            if nr >= 0 and nr < R and nc >= 0 and nc < C:
                queue.append((nr, nc))

```

+++ Recursive DFS

```py
def dfsGrid(grid: list[list[int]], start: tuple[int, int]):
    R = len(grid)
    C = len(grid[0])

    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    seen = set()

    def dfsVisit(grid: list[list[int]], curr: tuple[int, int]):
        # pre-process curr

        seen.add(curr)
        r, c = curr

        for dr, dc in directions:
            nr = r + dr  # new r
            nc = c + dc  # new c
            if nr >= 0 and nr < R and nc >= 0 and nc < C:
                dfsVisit(grid, (nr, nc))
        
        # post-process curr

    dfsVisit(grid, start)  # optional, enforce starting position

    # handle disconnected graphs
    for r in range(R):
        for c in range(C):
            if (r, c) not in seen:
                dfsVisit(grid, (r, c))
```

+++ Best First
```py
import heapq
from typing import Callable

def bestFirstGrid(
    grid: list[list[int]],
    start: tuple[int, int],
    priority: Callable[..., int | float],
):
    R = len(grid)
    C = len(grid[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    seen = set()
    queue = []

    queue.append((0, *start))

    while len(queue) > 0:
        _, r, c = heapq.heappop(queue)

        if (r, c) in seen:
            continue
        seen.add((r, c))

        # process grid[r][c]

        for dr, dc in directions:
            nr = r + dr
            nc = c + dc
            if nr >= 0 and nr < R and nc >= 0 and nc < C:
                # priorityFn call signature could be different
                heapq.heappush(queue, (priority(grid, nr, nc), nr, nc))
```
+++
