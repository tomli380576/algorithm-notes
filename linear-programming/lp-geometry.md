# Geometry

## Simple Example

A company makes 2 products, $A$ and $B$, each unit of A makes $1 of profit and each unit of B makes $2. The demand is $\leq 300$ units of A and $\leq 200$ units of B. Suppose the total number of work hours available is 700, and making each A takes 1 hour, making each B takes 3. 

We can write this into a linear program that looks like

$$
\begin{aligned}
    \max\,& x_1 + 6x_2\\
    \text{such that}\,& x_1 + 3x_2 &&\leq 700\\
    &x_1&&\leq 300\\
    &x_2&&\leq 200\\
    &x_1&&\geq 0\\
    &x_2&&\geq 0\\
\end{aligned}
$$

## Properties of LP

1. Real number LP is solvable in poly time.
2. Integer LP (ILP) is NP-complete.
3. Feasible region is convex. (pick any 2 points in the space, the line connecting them is in the feasible region)
4. Optimal answers appear only at vertices

## Standard Form

We want to solve for values of $n$ variables $x_1, x_2, \dots x_n$ that satisfies $m$ constrains while minimizing or maximizing the objective function.

$$
\begin{aligned}
    \max\, & c^\top\bold x\\
    \text{such that}\,& A\bold x &&\leq\bold b\\
    &\bold x&&\geq 0
\end{aligned}
$$

where $A$ is a $m\times n$ matrix. A and $\bold b$ together form the constraints.

## Geometric view

The feasible region is the intersection of halfspaces cut by the hyperplanes given by the constraints in $\R^n$. 

There are at least ${n+m}\choose{n}$ vertices. For each vertex, it has $\leq nm$ neighbors.

## LP algorithms

**Simplex algorithm** has worst case exponential time, but fast in practice.

Basic idea:
1. Start at $\bold x = \bold 0$
2. Check all neighbor vertices with better objective value. Keep doing this
3. If we can't find anything better in the neighborhood, then $\bold x$ is already the global best.

## Check for Infeasibility

Add a new variable $z\in \R$, then try to solve

$$
\begin{aligned}
    \max\, & z\\
    A\bold x + z &\leq \bold b\\
    \bold x&\geq 0
\end{aligned}
$$
