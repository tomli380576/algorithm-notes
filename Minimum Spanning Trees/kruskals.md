# Kruskal's 

> Scan $E$ by increasing weight.
> If the current edge is safe, add it to $F$

## MakeSet, Union, Find

To combine the newly selected safe edge to the current tree, Kruskal’s Algorithm uses these 3 helper functions:

1. `MakeSet(v: Vertex)` Makes a new set containing the given vertex
2. `Find(v: Vertex)` Finds a set that contains the given vertex
3. `Union(set1: Set<Vertex>, set2: Set<Vertex>)` Merges 2 sets

In actual implementation the function arguments might be different (passing objects on the stack is expensive) but the behavior should be the same.

## Pseudocode

```
function KruskalsMST(G) -> Array<Edges>:
	F = {}
	sorted_edges = sorted(G.Edges)

	for each vertex **v** in G:
		MakeSet(v)

	for each edge **uv** in sorted_edges:
		if Find(u) != Find(v): // if safe
			put uv in F
			Union(u, v)

	return F
```

## Runtime Analysis

Initialization + Sort + Find + Union:

$$
V + E\log E + E\cdot O(\ce{find}) + (V-1)\cdot O(\ce{union}) = O(E\log E)
$$

Usually $O(E\log E)$. This can also be improved to $O(E\log V)$
