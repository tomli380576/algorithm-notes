---
order: 2
---

#  Breadth First Search (122A)

We can implement BFS from whatever first search by using a **Queue**.

##  1. Pseudocode

|||  Barebones 122A Version

```c
function BFS_Search(G, start):
	for each vertex v in G:
		mark v as NEW

	queue = Queue()
	put start in queue
	mark start as VISITED

	while queue is not empty:
		u = queue.getFirst()
		process(u)
		for each adjacent vertex v:
			if v is NEW:
				mark v as VISITED
				put v in queue
```

||| Queue based WhateverFirstSearch

```c
function WFS_Breadth(G, start):
	for each vertex v in G:
		mark v as NEW

	queue = Queue()
	put start in queue

	while queue is not empty:
		u = queue.getFirst()

		if u is NEW:
			process(u)
			mark u as VISITED
			for each adjacent vertex v:
				put v in queue
```
|||

The 122A version checks whether the adjacent vertex `v` is visited. The WFS version checks if the current vertex `u` is new. 

####  With time (full 122A Version)

```c
function BFS_Search(G, start):
	DISCOVER_TIME = {}
	queue = Queue()

	for each vertex v in G:
		DISCOVER_TIME[v] = Infinity
	
	DISCOVER_TIME[start] = 0
	put start in queue

	while queue is not empty:
		u = queue.getFirst()
		for each adjacent vertex v:
			if DISCOVER_TIME[v] == Infinity:
				process(v) // arbitrary subroutine
				DISCOVER_TIME[v] = DISCOVER_TIME[u] + 1
				put v in queue
```

`process(v)` is just a blackbox subroutine that you can do anything inside it.

##  Observing BFS’s Behavior

We can observe how each ‘layer’ of the graph is being visited by BFS by adding a special token.

The changes are highlighted. Everything else is the same. 

+++ Barebones Version

```c #8,12-15
function BFS_WithToken(G, start):
	queue = Queue()

	for each vertex v in G:
		mark v as NEW

	put start in queue
	put TOKEN in queue

	while queue has at least 1 vertex:
		u = queue.getFirst()
		if u == TOKEN:
			print(TOKEN)
			put u in queue
		else:
			if u is NEW:
				process(u)
				mark u as VISITED
				for each adjacent vertex v:
						put v in queue
```
+++ With Clock (122A Version)
    
```c #10,14-16
function BFS_WithToken(G, start):
	DISCOVER_TIME = {}
	queue = new Queue()

	for each vertex v in G:
		DISCOVER_TIME[v] = Infinity
	
	DISCOVER_TIME[start] = 0
	put start in queue
	put TOKEN in queue

	while queue has at least 1 vertex:
		u = queue.getFirst()
		if u = TOKEN:
			put u in queue
		else:
			for all edges u→v:
				if DISCOVER_TIME[v] == Infinity:
					process vertex v
					DISCOVER_TIME[v] = DISCOVER_TIME[u] + 1
					put v in queue
```
+++

####  Implementation Detail

!!!danger
`while queue has at least 1 vertex` does not mean `queue.size > 0`. The special token is not a vertex.
!!!

##  Python. BFS with Token

[!badge variant="dark" size='xl' target="blank" text="Github"](https://github.com/tomli380576/122Algorithms-python-implementation/blob/main/basic-BFS.py)

##  Examples

###  EX.1

![`example_unweighted_graphs.py UNDIRECTED_2`](../assets/bfs_dfs/Screen_Shot_2022-07-01_at_1.50.53_AM.png){class="image-m"}

If we start at $\tt{\color{darkorange}A}$, then the graph will be visited in this order: $\tt{{\color{darkorange} A}\to \red {BCE}\to\purple D}$

Vertices with the same color could be visited in any order depending on the order of insertion, but the overall order is Orange → Red → Purple.

Running the code from 2.3:

![Screen Shot 2022-07-01 at 2.10.29 AM.png](Breadth%20First%20Search%20(122A)%2093da5e914bde44dcb82fa021ef450a7f/Screen_Shot_2022-07-01_at_2.10.29_AM.png)

![`example_unweighted_graphs.py UNDIRECTED_2`](Breadth%20First%20Search%20(122A)%2093da5e914bde44dcb82fa021ef450a7f/Screen_Shot_2022-07-01_at_2.11.43_AM.png)

`example_unweighted_graphs.py UNDIRECTED_2`

###  EX.2

Given this graph, and we start at vertex $\tt{S}$

![`example_unweighted_graphs.py UNDIRECTED_3`](../assets/bfs_dfs/Screen_Shot_2022-07-01_at_1.48.40_AM.png){class="image-m"}

Running the code from 2.3:

|||Text
![](../assets/bfs_dfs/Screen_Shot_2022-07-01_at_1.50.34_AM.png)
|||Graph
![](../assets/bfs_dfs/Screen_Shot_2022-07-01_at_1.50.53_AM.png)

Starting from $\tt  S$, we can see that all the red nodes $\tt{\red{ACG}}$ are 1 edge away, and the purple nodes $\tt\purple{BDEFH}$ are 2 edges away.
|||

