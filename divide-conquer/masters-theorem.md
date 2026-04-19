# Masters Theorem (122A)

## Shortcut version

For a recurrence relation of the form:

$$
\begin{aligned}
T(n) &= aT\left(\left\lceil\dfrac nb\right\rceil\right) + O(n^d)\\

a, b \in \Z&; a > 0, b> 1\\
d\in\R&; d\geq 0    
\end{aligned}
$$

we have this shortcut

$$
T(n) = \begin{cases}
    O(n^d) & \text{if }d > \log_ba\\
    O(n^d\log n) & \text{if } d = \log_b a\\
    O(n^{\log_ba})&{if }d < \log_ba
\end{cases}
$$

!!!tip 
This is a shortcut because a lot of the times we get polynomial runtimes for the work at each level $O(n^d)$. For a more general Masters theorem, see the next section.
!!!

## General version

For a runtime function $T(n)$ given by:

$$
T(n)=a\cdot T\left(\frac nb\right) + f(n)
$$

where $a, b, n\in \Bbb N$ and they represent:

- $a$: number of recursive calls. $a > 0$
- $b$: how much we reduce the problem. $b>1$
- $f(n)$ : work at each level.

!!!info Example
$$
T(n)=3T\left(\dfrac n4\right) + n^2\\
$$

In this case $a = 3, b=4, f(n)=n^2$. It means the main routine does 3 recursive calls, each with a problem size $\frac n4$, and the work at each level is $n^2$.
!!!

We have 3 cases depending on $n^{\log_ba}$ vs. $f(n)$

### Case 1. The recursive step is slow (lots of recursive calls)

First check if:

$$
n^{\log_ba} >\,f(n)
$$

If that’s true, then check if:

$$
f(n)=O\left(n^{(\log_ba) - \varepsilon}\right)\text{ for some }\varepsilon>0
$$

If such $\varepsilon$ exists, then:

$$
T(n)=\Theta(n^{\log_ba})
$$

`@Hint` Observe that $n^{\log_ba}$ is the depth of the recurrence tree. So the overall $T(n)$ is dominated by the recursion (a lot of leaves, not much work at each leaf)

### Case 2. Recursion and $\small f(n)$ are about the same speed

First check if:

$$
f(n) = \Theta(n^{\log_ba})
$$

If that’s true, then:

$$
T(n)=\Theta(n^{\log_ba}\cdot \log n)
$$

### Case 3. The work at each level is slow

First check if:

$$
n^{\log_ba} <\,O(f(n))
$$

Then check if:

1. There exists $\varepsilon > 0$ such that:
    
    $$
    f(n)=\Omega \left(n^{(\log_ba)+\varepsilon}\right)
    $$
    
2. **AND** if there exists positive $c<1$ such that:
    
    $$
    a\cdot f\left(\frac nb\right)\le c\cdot f(n)
    $$
    
If both are satisfied, then:

$$
T(n)=\Theta(f(n))
$$

All the big $O, \Omega, \Theta$ checks can be done using the limit lemma.
