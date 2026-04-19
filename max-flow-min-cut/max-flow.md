# Maximum Flow

In a graph with flows $G = (V, E, C)$, each edge $e$ has a maximum flow capacity $c_e, c_e > 0\,\forall e$ (think of the train network etc.). We want to find, for a starting vertex $s$ and target vertex $t$, what's the maximum amount of flow we can achieve in this graph (e.g. maximum amount of goods we can deliver from $s$ to $t$)

Formally:

!!!base Maximum Flow Problem
Find $f_e$ for all $e\in E$ such that:

1. Capacity constraint: $\forall f_e, 0\leq f_e\leq c_e$
2. Conservation of Flow: for anything that's not $s$ and $t$, what comes in must come out:
$$
\forall v\in V- \{s, t\}\\
\sum_{wv\in E}f_{wv} = \sum_{vz\in E}f_{vz}
$$
!!!

## Residual Network

For a flow network $G=(V, E, C)$ and flow $f_e$ for each $e\in E$, the residual network $G^f=(V, E^f)$ is given by:

1. If edge is not saturated: $vw \in E$ and $f_{vw} < c_{vw}$, then add the edge in to $G^f$ with its remaining capacity $c_{vw} - f_{vw}$
2. If the edge $vw$ has a positive flow, then add the backward edge $wv$ t $G^f$ with capacity $f_{vw}$

## Ford-Fulkerson

1. Init flow graph $f$ with $f_e= 0$ for all $e\in E$. 
2. Make $G^f$ for current flow $f$
3. Check if there's any path from $s$ to $t$ in $G^f$, call it $\cal P$, in $G^f$. If not, return $f$.
4. In $\cal P$, let $c(\cal P)$ be the minimum capacity along $\cal P$ in $G^f$
5. For each forward edge in $\cal P$, increase its flow by $c(\cal P)$. For each backward edge, decrease its flow by $c(\cal P$).

## Running Time

A big assumption here is that all capacities are **integers**. If we have this, we know that the flow always increase by $\geq 1$ every round.

Let $f^*$ be the maximum flow, then we have at most $f^*$ rounds.

The time per round is the sum of 
1. Time to built the residual network: O(V), just copy the vertexes
2. Check st-path: O(V+E) from DFS and BFS
3. Find min capacity: O(V) because worst case scenario we visit every vertex -> V-1 edges
4. Update flows: same as finding min capacity, we just go through each edge in $\cal P$

$\implies$ Time per round is $O(E)$
$\implies$ Total running time is $O(f^*E)$

This running time is pseudo-polynomial because it involves $f^*$.

## Convert to Edmond-Karp

**Take the shortest path from s to t in $G^f$ instead of any path.** Shortest path here means the path with the minimum number of vertices $\implies$ use BFS only.

### Running time

$O(E^2V)$
