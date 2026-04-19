# Single Source Shortest Paths (SSSP)

We want to find the shortest distance from starting vertex $s$ to all other vertices in $G$. The final output is an array `dist[z]`, the distance from $s$ to $z$.

## Bellman Ford

If there are no negative cycles, the shortest path $\cal P$ exists and $|\cal P| \leq n-1$ edges.

### DP Recurrence

Label each edge from $i = 0\dots n-1$. 

For $i = 0\dots n-1$ and $z\in V$, **the subproblem is: $D(i, z) =$ the length of the shortest path from $s$ to $z$ using $\leq i$ edges.** 

The base cases is when we use 0 edges:
$$
\begin{aligned}
D(0, s) &= 0\\
D(0, z) &= \infty \,\forall z\neq s    
\end{aligned}
$$

In the recursive case, consider:
1. The second-to-last vertex $y$ where $yz\in E$. 
    Try each possible choice of $y$ i.e. every vertex that has $z$ as its neighbor:

    $$
    \min_{y:yz\in E}\{D(i-1, y) + w(yz)\}
    $$

    where $w(yz)$ is the weight.

2. Don't use $y$, see if we can reach $z$ with $i-1$ edges
    $$
    D(i-1, z)
    $$

Finally take the min of both:
$$
D(i, z) = \min\left\{\begin{aligned}
    &\min_{y:yz\in E}\{D(i-1, y) + w(yz)\}\\ 
    &D(i-1, z)
\end{aligned}\right\}
$$

### Find Negative weight cycles

**Check if $D(|V|, z) < D(|V| - 1, z)$ for some $z\in V$.** If so, then we have a negative cycle. The $z$ that's not a sink is where the negative cycle happens.

If not, then we can use either $D(|V|, *)$ or $D(|V|-1, *)$ as the final return array. 

Note that Bellman-Ford can only find negative cycles that are REACHABLE from $s$.
