---
order: 1
---

# Recursive Depth First Search (122A)

# 1. Vertex Status

Similar to whatever first search, we assign each vertex a $\purple{\texttt{STATUS}}$.

- $\texttt{NEW}$: literally never seen before, all vertices start with this status
- $\texttt{ACTIVE}$: seen before, but the adjacent vertices are not done processing yet
- $\texttt{FINISHED}$: the vertex is seen and all its children are done processing

# 2. Pseudocode

### Barebones Version

```c
// main routine
function DFS_Search(G):
	for each vertex v in G:
		mark v as NEW
	for each vertex v in G:
		if v is NEW:
			DFS_Visit(v)

function DFS_Visit(u):
	mark u as ACTIVE
	// process when u is ACTIVE
	Preprocess(u) 
	
	for each adjacent vertex v:
		if v is NEW: 
			DFS_Visit(v) 

	mark u as FINISHED
	// process when u is FINISHED
	Postprocess(u)

```

### With Clock (122A)

```c
// globals
time = 0 
DISCOVER_TIME = {}
FINISH_TIME = {}

// main routine
function DFS_Search(G):
	for each vertex v in G:
		mark v as NEW
	for each vertex v in G:
		if v is NEW:
			DFS_Visit(v)

function DFS_Visit(u):
	mark u as ACTIVE
	time++
	DISCOVER_TIME[u] = time
	Preprocess(u)
	
	for each adjacent vertex v:
		if v is NEW: 
			DFS_Visit(v) 

	mark u as FINISHED
	time++
	FINISH_TIME[u] = time
	Postprocess(u)
```

- Long Version
    
    ```tsx
    // pseudocode
    
    // Global Variables
    // shorthand syntax, it means STATUS can only be 1 of the following
    type STATUS = 'undiscovered' | 'discovered' | 'finished'
    
    // When actually implementing this use an vertex id to replace 'Vertex'
    // technically Vertex objects are not hashable so it won't work as a key
    VISIT_STATUS: Map<Vertex, STATUS> = [...]
    DISCOVER_TIME: Map<Vertex, int> = [...]
    FINISH_TIME: Map<Vertex, int> = [...]
    
    // time is global var
    let time = 0 
    
    // main routine
    function DFS_Search(G: Graph) returns void:
    		// initialize all vertices as not discovred
    		for v in G.Vertices: 
    			VISIT_STATUS[v] = 'undiscovered';
    		for v in G.Vertices:
    			if VISIT_STATUS[v] == 'undiscovered':
    				DFS_Visit(v)
    
    // helper
    function DFS_Visit(u: Vertex) returns void:
    		// 1st opportunity to process vertex u
    		VISIT_STATUS[u] = 'discovered'
    		time++
    		DISCOVER_TIME[u] = time
    	
    		// check all of u's adjacent nodes
    		for adjacent_vertex in u.adjacent_vertexes: 
    			// if never visited
    			if VISIT_STATUS[adjacent_vertex] == 'undiscovered': 
    				// the 'stack' is the literal call stack
    				DFS_Visit(adjacent_vertex) 
    	
    		// now all children of u are finished
    		// 2nd opportunity to process vertex u
    		VISIT_STATUS[u] = 'finished'
    		time++
    		FINISH_TIME[u] = time;
    ```
    

### Comparison with stack based Whatever First Search

```c
function WFS_Depth(G):
	for each vertex v in G:
		mark v as NEW
	for each vertex v in G:
		if v is NEW:
			WFS_Visit(G, v)
			
function WFS_Visit(G, start):
	stack = Stack()
	stack.put(start)

	while stack is not empty:
		u = stack.popFirst()
		if u is not VISITED:
			process u
			mark u as VISITED
			for each adjacent vertex v:
				stack.put(v)
```

```c
function DFS_Search(G):
	for each vertex v in G:
		mark v as NEW
	for each vertex v in G:
		if v is NEW:
			DFS_Visit(v)

function DFS_Visit(u):
	mark u as ACTIVE
	Preprocess(u)
	
	for each adjacent vertex v:
		if v is NEW: 
			DFS_Visit(v) 

	mark u as FINISHED
	Postprocess(u)

```

The recursive version replaces `stack.put(v)` with the recursive call `DFS_Visit(v)`.

`while stack is not empty` is the same as coming back to the initial call (empty call stack).

Every time `DFS_Visit(v)` is called in the main routine, a new call stack is initialized. WFS does this by calling the stack constructor.

# 3. Runtime Analysis

We call the `DFS_Visit(v)` function $V$ times in the main routine

The total number of recursive calls inside `DFS_Visit(v)` is $E$ times because we will traverse each edge exactly once. So $E$ times for the entire graph.

The total runtime is $O(V+E)$

# 4. Python: Basic DFS

![`example_unweighted_graphs.py DIRECTED_1`](../Graph%20Visualizations%20e194fa603a6d4f42859959ad780a053c/Screen_Shot_2022-07-01_at_2.28.39_AM.png)

`example_unweighted_graphs.py DIRECTED_1`

[ECS122A-Algorithms-python-implementation/basic-DFS.py at main · tomli380576/ECS122A-Algorithms-python-implementation](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/basic-DFS.py)

# 5. Classifying Edges and Cycle Detection

There are 4 possible types of edges:

$$
\begin{array}{||c:cccc||}
\hline\\
\text{Tree} & &\texttt{ACTIVE}&\longrightarrow& \texttt{NEW}\\\\
\text{Back}&& \texttt{ACTIVE}&\longrightarrow& \texttt{ACTIVE}\\\\
\text{Forward}&&\texttt{ACTIVE}&\longrightarrow& \texttt{FINISHED}\\\\
\text{Cross} &&& \text{Everything else}\\\\
\hline

\end{array}
$$

More specifically:

![Source: Prof Bai’s 122A slides. Gray = Active, White = New, Black = Finished](Recursive%20Depth%20First%20Search%20(122A)%20c0c4990a6357427eb4880aea9fec181a/Screen_Shot_2022-06-20_at_6.18.34_PM.png)

Source: Prof Bai’s 122A slides. Gray = Active, White = New, Black = Finished

## 5.1 Edge Interpretations

- At least one back edge is found $\iff$ the graph has a cycle
    
    So Directed Acyclic Graphs (Like a DP dependency graph) cannot have a back edge
    
- Found at least 1 cross or forward edge $\implies$ the graph is directed

[](https://courses.csail.mit.edu/6.006/spring11/rec/rec13.pdf)

# 6. Python: DFS with edge classification

[ECS122A-Algorithms-python-implementation/DFS-cycle-detection.py at main · tomli380576/ECS122A-Algorithms-python-implementation](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/DFS-cycle-detection.py)

Right now it only finds back and tree edges correctly, still figuring out how to find forward and cross edges

---

Go back:

[$\star$ Basics of DFS & BFS](../BFS%20&%20DFS%20e6658526c6884bc2ac345aeb09e4ffc0.md) 

Go to next:

[Breadth First Search (122A)](Breadth%20First%20Search%20(122A)%2093da5e914bde44dcb82fa021ef450a7f.md)