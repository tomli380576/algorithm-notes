# Minimum Spanning Tree

!!!question Question
For a weighted graph $G = (V, E, W)$, how do we connect all the vertices in $V$ while minimizing the sum of the edge weights?
!!!

:::def

#### **Def.** Safe Edge / Crossing the Cut

An undirected edge $uv$ from $G\backslash F$ is **safe** with respect to intermediate spanning tree $F$ if

1. $uv$ is the edge with minimum weight in $G\backslash F$
2. $uv$ has exactly 1 vertex in some component of $F$

This is the edge that’s **crossing the cut** and the greedy choice for Prim’s and Kruskal’s.

`@Hint` If both $u$ and $v$ are in $F$, then we get a cycle. If neither are in $F$, then $uv$ is not connected to $F$
:::

## The only MST algorithm

```
function GenericMST(G: Graph) -> Set<Edge>:
	F = {} // empty tree or empty set

	while F is not a spanning tree:
        Add a safe edge to F

	return F
```

We just need to fill in the details for:

1. The while loop condition. What does it mean for $F$ to not be a spanning tree?
2. Classifying a safe edge. This might not be a direct translation of the definition above.
