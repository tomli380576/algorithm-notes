# All Pairs Shortest Paths (APSP)

Let `dist[s, t]` be the length of the shortest path from $s$ to $t$. We want to find `dist[s, t]` for all $s, t\in V$.

## Floyd-Warshall

### Basic Idea

Try a "prefix" of the vertices of the graph. Arbitrarily label the vertices with $1\dots|V|$. We are going to try to use only the vertices $1\dots i$ in each subproblem. Do this for all possible combinations of $s$ and $t$ 

Let $D(i, s, t)$ be the length of the shortest path from $s$ to $t$ using a subset of vertices $\{1\dots i\}$. 

### Base Case

No vertices:

$$
\forall s,t\in E, D(0, s, t) = \begin{cases}
    w(st) & \text{if }st\in E\\
    \infty & \text{if }st\notin E
\end{cases}
$$

### Recursive Cases

1. Not using vertex $i$:
   $$
   D(i-1, s, t)
   $$
2. Use vertex $i$:
   Recall that we are ONLY using vertices $1\dots i$, so the path looks like $s\to\text{something in }\{1, \dots i-1\} \to i\to \text{something in }\{1, \dots i-1\} \to t$ 
   
   $s\to\text{something in }\{1, \dots i-1\} \to i$ is just $D(i-1, s, i)$. Similarly $i\to \text{something in }\{1, \dots i-1\} \to t$ is $D(i-1, i, t)$

   Therefore we add them together
   $$
   D(i-1, s, i) + D(i-1, i, t)
   $$

Now take the min of the 2 cases:

$$
D(i,s,t) = \min\left\{\begin{aligned}
    &D(i-1, s, t)\\
    &D(i-1, s, i) + D(i-1, i, t)
\end{aligned}\right\}
$$

The final answer is in $D(|V|, *, *)$.

## Negative weight cycles in APSP

**Check if $D(|V|, y, y) < 0$ for some $y\in V$.** This can detect negative cycles anywhere in the graph unlike Bellman-Ford.