---
order: 100
---

# Ready-to-go leetCode templates

## Whatever first search

### Adjacency List

Suppose the graph is stored as

```py
type Graph = dict[str, list[str]]
```

+++ DFS & BFS

||| BFS

```py !#10
from collections import deque

def bfs(G: dict[str, list[str]], start: str):
    queue = deque()
    seen = set()

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
    seen = set()

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

+++ BFS & DFS

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
