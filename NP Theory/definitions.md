# NP introduction

## P vs. NP 

P: problems that are solvable in polynomial time

NP: "all" problems, including those that are not solvable in polynomial time. But the answers to these problems can be verified in polynomial time

## P = NP?

This basically says "If I can verify the solution in polynomial time, then I can solve the problem in polynomial time"

## Search Problems

Given an instance $I$ of the problem, find solution $S$ for $I$ if one exists, otherwise return NO.

For this to be a search problem, if we have $I$ and $S$, then we can verify $S$ is a solution to $I$ in polynomial time (poly-time w.r.t the size of $I$)

### Ex.1 Satisfiability Problem (SAT)

Input: boolean formula in conjunctive normal form (CNF) with $n$ vars and $m$ clauses

Output: satisfying assignment if 1 exists

$$
f = (x_3\vee \bar x_2 \vee \bar x_1)\wedge(x_1)\wedge(x_2\vee\bar x_3)\wedge(\bar x_1\vee\bar x_3)
$$

To show SAT is NP, we show that the answer can be checked in poly time. The actual running time of the solution doesn't matter because even if it's in P, it's still in NP.

For a solution of $x_1\dots x_n$ for $m$ clauses, the running time to verify the solution is $O(nm)$
- Each clause takes $O(n)$, we have $m$ clauses, so worst case $O(nm)$


### Ex.2 $k$-Coloring 

Input: undirected $G=(V,E)$ and $k\in \N$

Output: Assign each vertex a color in $1\dots k$ such that adjacent vertices get different colors. Return NO if there's no such coloring.

A solution can be checked by going through every edge, check each edge has 2 colors, i.e. 

$$
\forall uv\in E, color(u) \neq color(v)
$$

### Ex.3 MST

Recall that MST is in P, so it's also in NP. For a solution tree $T$, we can check whether $T$ is the MST by first checking if $T$ is a tree. This can be done with BFS/DFS, check if there's no cycles, has all vertices. To check if $T$ is an MST, compare weight(T) with the weight of the tree from the result of prim's or kurskal's.

### Ex.4 Knapsack Problem

Input: $n$ objects with weights $w_1\dots w_n$ and values $v_1\dots v_n$, a capacity $B$

Output: Set of objects such that total weight $\leq B$ and maximizes total value

Both the repeated version and unique item versions are NP

**Knapsack is not KNOWN to be in NP or P,** because we can't check the solution in polynomial time. We would have to check every subset of the items, so $2^n$ possible combinations. But there could be a faster way to do this, so we don't really know. If it was proved that knapsack has no poly time solution, then that would show $P\neq NP$.

### Ex.5 NP variant of Knapsack

Input: 
- $n$ objects with weights $w_1\dots w_n$ 
- values $v_1\dots v_n$
- capacity $B$
- goal $g$

Output:

- A subset $S$ such that $\sum_{i\in S}w_i\leq B$ and $\sum_{i\in S}v_i\geq g$
- NO if $S$ doesn't exist

This variant is NP because we can check the solution by doing a binary search on the items and $g$. Let $V=\sum_{i\dots n}v_i$, binary search on $g$ from 1 to $V$.

## NP Completeness

If $P\neq NP$, there will be a set of intractable problems known as NP-complete. These problems do not have a polynomial time solution.

### SAT is NP complete

If SAT is NP complete:
- SAT is NP
- If we can solve SAT in poly time, then we can solve every NP problem in poly time

