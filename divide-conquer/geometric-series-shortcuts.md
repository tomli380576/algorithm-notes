# Geometric Series Shortcuts

For a constant $r \in \R_{>0}$ and some upper bound $k$ ($k\in\Z$ is not required when we are dealing with big Os since we can bound it with $\lceil k\rceil$), we have this nice shortcut to get the upper bound:

$$
\begin{aligned}
    \sum^k_{n=0} r^n &= 1 + r + r^2 + \cdots + r^k\\
    &= \begin{cases}
        O(r^k) & \text{if } r > 1\\
        O(k) & \text{if } r = 1\\
        O(1) & \text{if } r < 1
    \end{cases}
\end{aligned}
$$

The basic idea is to **bound each term** in this series. For example when $r > 1$, the last term of the series $r^k$ is the largest. So we can bound each term with it and get:

$$
\sum^k_{n=1} r^n \leq \sum^k_{n=1}r^{\red k} = O(kr^k) = O(r^k)
$$

When $r=1$, it's just $k$. Since we are conveniently allowing non-integer $k$ values, the bound is $O(k)$ instead of just $k$.

Similarly when $r <1$, the first term dominates since the rest is just powers of $0<r<1$. 

## Examples

$$
T(n) = 3T\left(\frac n2\right) + O(n)
$$

Expand, the number of levels is $i = \log_2n$

$$

T(n) \leq cn\cdot \left(1 + \frac 32 + (\frac32)^2 + \cdots (\frac32)^{\log_2(n-1)}\right) + 3
$$
