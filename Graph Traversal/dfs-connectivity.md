# Finding Connected Components with DFS

Let's modify the DFS algorithm to label each connected component


## Undirected

```
// map from vertex to its connected component number
cc_num: map[Vertex, int] = {} 
prev: map[Vertex, Vertex] = {}
seen: map[Vertex, bool] = {}

function DFS(G=(V, E)):
    cc = 0 // connected component index
    forall v in V:
        seen[v] = false
        prev[v] = NULL

    forall v in V:
        if not seen[v]:
            cc += 1
            Explore(v, cc)

function Explore(z: Vertex, cc: int):
    cc_num[z] = cc
    seen[z] = True
    forall w in z.neighbors:
        if w is NEW:
            Explore(w)
            prev[w] = z
```

## Directed

```
// map from vertex to its connected component number

pre: map[Vertex, int] = {} // pre-order number
post: map[Vertex, int] = {} // post-order number of a vertex
prev: map[Vertex, Vertex] = {}
seen: map[Vertex, bool] = {}

function DFS(G=(V, E)):
    clock = 1
    forall v in V:
        seen[v] = false
        prev[v] = NULL

    forall v in V:
        if not seen[v]:
            cc += 1
            Explore(v, cc)

function Explore(z: Vertex):
    pre[z] = clock
    clock += 1

    seen[z] = True
    forall w in z.neighbors:
        if w is NEW:
            Explore(w)
            prev[w] = z

    post[z] = clock
    clock += 1
```

**Post order numbers are the useful ones.**

## Edge Classification

For an edge $z\to w$:
- Tree edge: `post[z] > post[w]`
- Back edge: `post[z] < post[w]`
- Forward edge: `post[z] > post[w]`
  - Forward edges can jump multiple levels down the tree
  - Tree edges go down 1 at a time (new discoveries) 
- Cross edge: `post[z] > post[w]`
  - These edges happen across nodes that don't share ancestors

## Cycles

**$G$ has a cycle *iff* the DFS tree has a back edge.**

## Topological Sort

This applies to directed acyclic graphs (DAG). A DAG is topologically sorted when all vertices are arranged such that all edges go from vertices with **higher `post` to lower ones**.

To sort a DAG, run DFS and sort vertices by `post` in descending order. Sorting here takes $O(V)$ because we don't need comparison sort, DFS takes $O(V+E)$.

Source vertex has the highest `post`, sink vertex has the lowest `post`

### Alternative topological sort

1. Find a sink vertex, output and delete it from $G$
2. Repeat (1) until the graph is empty

## General Directed Graphs

### Strongly Connected Components (SCC)

Vertices $v, w\in V$ are strongly connected if there's a path $v \rightsquigarrow w$ and $w \rightsquigarrow v$.

A strongly connected component is the maximal set of strongly connected vertices. There can be multiple SCCs in a single graph.

### Metagraph

Treat each SCC as a single vertex. If there's any edge in $E$ that connects 2 SCCs, then there's an metagraph edge between them. 

**The metagraph of SCCs is always a DAG.** If there's a cycle between 2 SCCs $S$ and $S'$, then they are just subgraphs of the same SCC $S\cup S'$.

**Every arbitrary directed graph is a DAG of its SCCs.**

## Use 2 DFS runs to find all SCCs

### Basic Idea

Observe that the metagraph must have a sink and a source. This allows us to use [alternative topological sort](#alternative-topological-sort), but on SCCs.

1. Find a sink SCC, remove 
2. Repeat (1) until G is empty

In this case we want to use **sink** because we can easily find it as long as we have a sink vertex `z` from the original graph. Run `Explore(z)` from it, then collecting everything reachable from `z` gives us the sink SCC.
- Source won't work like this because all vertices in the source SCC can reach all other SCCs, then we have no way to know when did we leave the source SCC.

### Guaranteed sink?

**$v$ with the lowest `post` is NOT always in the sink SCC.** But $v$ with the highest `post` is in a source SCC.

Idea: flip all the edges to turn the sink SCC into a source SCC.

For $G = (V, E)$, the reverse is $G^R = (V, E^R)$.

$$
E^R = \{wv: vw\in E\}
$$

source SCC in $G$ = sink SCC in $G^R$
sink SCC in $G$ = source SCC in $G^R$

So we just need the vertex with the highest `post` in $G^R$. That vertex is the sink in $G$.

## Final SCC Algorithm

```
function SCC(G=(V, E)):
    build G_R = (V, E_R)
    post_R = DFS(G_R)
    sort V descending by post_R
    cc_num = undirected_DFS(G)
```

The `cc_num` will now have the connected component number of each vertex.


