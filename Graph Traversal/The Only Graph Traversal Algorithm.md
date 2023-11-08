---
order: 3
---

# The Only Graph Traversal Algorithm

## Whatever First Search

> **Question.** How to visit every vertex exactly once in a graph?
 

!!!info **Def.** Vertex Status

To keep track of which vertices we have seen, we use 2 $\texttt{STATUS}$ values:

- $\texttt{NEW}$: Never seen before.
- $\texttt{VISITED}$: Processed exactly once.
!!!

### Pseudocode

Let **T** be the generic typename. We will make up an imaginary data structure called a **`Bag<T>`**. It has 3 methods:

1. `put(elem: T)` puts an element in the bag.
2. `getFirst() -> T` returns whatever is the “first” thing in the bag and removes it.
3. `isEmpty() -> bool` returns whether the bag is empty or not.

Then the pseudocode for whatever first search is:

```c
function WhateverFirstSearch(G: Graph, start: Vertex):
	for each vertex v in G:
		mark v as NEW

	bag = Bag<Vertex>()
	bag.put(start)

	while bag is not empty:
		u = bag.getFirst()
		if u is NEW:
			process(u) 
			mark u as VISITED
			for each adjacent vertex v of u:
				bag.put(v)
```

Here `process(u)` is just a blackbox subroutine. We can do anything inside `process(u)` as long as we don’t change the vertex status.

### Important Variants

By changing the bag's behavior of `getFirst()`, we get the familiar search algorithms:

#### Bag is a Stack: Depth First Search

DFS is usually implemented with recursion allowing cycle detection. 

See [Recursive Depth First Search (122A)](BFS%20&%20DFS%20e6658526c6884bc2ac345aeb09e4ffc0/Recursive%20Depth%20First%20Search%20(122A)%20c0c4990a6357427eb4880aea9fec181a.md) 

#### Bag is a Queue: Breadth First Search

See [Breadth First Search (122A)](BFS%20&%20DFS%20e6658526c6884bc2ac345aeb09e4ffc0/Breadth%20First%20Search%20(122A)%2093da5e914bde44dcb82fa021ef450a7f.md) 

#### Bag is a Priority Queue: Best First Search (For weighted graphs)

An example is [**❖** Handling Negative Edges: Full Dijkstra’s](Single%20Source%20Shortest%20Paths%20ccab559c3b5c4f018913429bf3b1091c/SSSP%20c9e75bc7de304c659a7da221c1837584/%E2%9D%96%20Handling%20Negative%20Edges%20Full%20Dijkstra%E2%80%99s%205065ff19c39f4f4595295d79e648bfa2.md) 

### Python. Whatever First Search

The `WhateverFirstSearch_Connected` function implements this.

[!button variant="dark" size="l" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/whateverFirstSearch.py)

!!!danger Assumption
This version of Whatever First Search assumes that the `start` can actually reach every other vertex in the graph. This works if we know the graph is connected.
!!!

To handle [disconnected graphs](https://mathworld.wolfram.com/DisconnectedGraph.html#:~:text=A%20graph%20is%20said%20to,has%20those%20nodes%20as%20endpoints.) we need to make the following modifications.

## Whatever First Search–All

> For each vertex in $G$, if we’ve never visited this vertex before, visit everything that we can reach from this vertex.
> 

We will make a small wrapper.

```c
function WhateverFirst_Wrapper(G):
	for each vertex v in G:
		mark v as NEW
	for each vertex v in G:
		if v is NEW:
			WhateverFirst_visit(G, v)
			
function WhateverFirst_Visit(G, start):
	bag = Bag<Vertex>()
	bag.put(start)

	while bag is not empty:
		u = bag.getFirst()
		if u is not VISITED:
			process(u)
			mark u as VISITED
			for each adjacent vertex v:
				bag.put(v)
```

Changing the bag's behavior on `getFirst()` will result in the same [variants](#important-variants), but now they can handle disconnected graphs.

### Python. WFS–All

The `WhateverFirstSearch_All` function implements this.

[!button variant="dark" size="l" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/whateverFirstSearch.py)

---

Go to next:

[BFS & DFS](BFS%20&%20DFS%20e6658526c6884bc2ac345aeb09e4ffc0.md)