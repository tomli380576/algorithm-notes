---
order: 3
---

# The Only Graph Traversal Algorithm

> **Question.** How to visit every vertex exactly once in a graph?

## Whatever First Search

!!! **Def.** Vertex Status

To keep track of which vertices we have seen, we use 2 `STATUS` values:

- `NEW`: Never seen before.
- `VISITED`: Processed exactly once.
!!!

Let `T` be the generic type name. We will make up an imaginary data structure called a `Bag<T>`. It has 3 methods:

1. `put(element: T)` puts an element in the bag.
2. `popFirst() -> T` returns whatever is the "first" thing in the bag and removes it.
3. `isEmpty() -> bool` returns whether the bag is empty or not.

Then the pseudocode for whatever first search is:

```c
function WhateverFirstSearch(G: Graph, start: Vertex):
	for each vertex v in G:
		mark v as NEW

	bag = Bag<Vertex>()
	bag.put(start)

	while bag is not empty:
		u = bag.popFirst()
		if u is NEW:
			process(u) 
			mark u as VISITED
			for each adjacent vertex v of u:
				bag.put(v)
```

Here `process(u)` is just a blackbox subroutine. We can do anything inside `process(u)` as long as we don’t change the vertex status.

### Important Variants

By changing the bag's behavior of `popFirst()`, we get the familiar search algorithms:

Stack
:	[Depth First Search](./dfs.md). DFS is usually implemented with recursion allowing cycle detection. 

Queue
:	[Breadth First Search](./bfs.md)

Priority Queue
:	[Best First Search]()

!!!success Tip
In python, we can easily swap between DFS and BFS by using `collections.deque`.
- `<deque>.popleft()` gives us BFS
- `<deque>.pop()` gives us DFS

For priority queue we could use `heapq` on a regular list `[]`.
- Use `heappush` and `heappop` to append and pop from the priority queue. See [here](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/5a7df2b8860fca70fa0f15713fa7d25610accb74/Implementations/SSSP-Dijkstras.py#L31-L50).

!!!

### :icon-code: Python

The `WhateverFirstSearch_Connected` function implements this. [!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/whateverFirstSearch.py)

!!!danger Assumption
This version of Whatever First Search assumes that the `start` can actually reach every other vertex in the graph. This works if we know the graph is connected.
!!!

To handle [disconnected graphs](https://mathworld.wolfram.com/DisconnectedGraph.html#:~:text=A%20graph%20is%20said%20to,has%20those%20nodes%20as%20endpoints.) we need to make the following modifications.

## Whatever First Search–All

> For each vertex in $G$, if we’ve never visited this vertex before, visit everything that we can reach from this vertex.
> 

We will make a small wrapper.

```c #1-6
function WhateverFirst_Wrapper(G: Graph):
	for each vertex v in G:
		mark v as NEW
	for each vertex v in G:
		if v is NEW:
			WhateverFirst_visit(G, v)
			
function WhateverFirst_Visit(G: Graph, start: Vertex):
	bag = Bag<Vertex>()
	bag.put(start)

	while bag is not empty:
		u = bag.popFirst()
		if u is not VISITED:
			process(u)
			mark u as VISITED
			for each adjacent vertex v:
				bag.put(v)
```

Changing the bag's behavior on `popFirst()` will result in the same [variants](#important-variants), but now they can handle disconnected graphs.

### :icon-code: Python

The `WhateverFirstSearch_All` function implements this. 
[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/whateverFirstSearch.py)
