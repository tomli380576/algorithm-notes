# Geometric Series Shortcuts

For a constant $\alpha \in \R_{>0}$ and some upper bound $k$ (doesn't have to be an integer when we are dealing with big Os), we have this nice shortcut to get the upper bound:

$$
\begin{aligned}
    \sum^k_{n=0} \alpha^n &= 1 + \alpha + \alpha^2 + \cdots + \alpha^k\\
    &= \begin{cases}
        O(\alpha^k) & \text{if } \alpha > 1\\
        O(k) & \text{if } \alpha = 1\\
        O(1) & \text{if } \alpha < 1
    \end{cases}
\end{aligned}

$$

## Examples

$$
T(n) = 3T\left(\frac n2\right) + O(n)
$$

Expand, the number of levels is $i = \log_2n$

$$

T(n) \leq cn\cdot \left(1 + \frac 32 + (\frac32)^2 + \cdots (\frac32)^{\log_2(n-1)}\right) + 3
$$