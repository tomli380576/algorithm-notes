---
order: 100
icon: light-bulb
---

# Introduction

## General DP Formulation

Typically a DP problem involves searching for a sequence of decisions that arrives at a maximum or minimum value. Let's define:

$t = 1...N$
:   A "stage" of the problem. When $t=N$, we have reached the end of the problem. This is usually the variable representing time / planning horizon, but not necessarily

$s_t\in S$  
:   State at stage $t$; $S$ is the space of all possible states

$a_t\in A$  
:   Action at stage $t$; $A$ is the space of all possible actions

$r_t(s_t, a_t)$  
:   Reward function if we take action $a_t$ at state $s_t$

$R(s_N)$
:   Reward if we end on state $s_N$. This is the cost/reward of our base case, a subproblem that we can immediately solve without recursion. If we reach $s_N$, we have no possible actions.

Now the **optimality equation** / **value function** / **objective recurrence** is:

$$
v_t(s_t) = \left.\underset{a_t\in A}{\max/\min}\,\middle\{v_{t+1}(s_{t+1}) + r_t(s_t, a_t) \right\}
$$

## Examples

### Rod Cutting

For a rod of length $N$,

$$
\text{CutRod}(n)  = \begin{cases}
    0       &   \text{if } n = 0\\
    p_1     &   \text{if }n = 1\\
    \displaystyle\max_{1\leqslant i\leqslant n}\{
    \text{CutRod}(n-i) + p_i
    \}  &   \text{otherwise}
\end{cases}
$$

In this case, the remaining length of the rod is the state $s_t$, and the number of units of rod to cut is the action $a_t$. We immediately encounter the first exception of the definition of stage $t$. The only thing that's constant is the **initial** length of the rod, or the maximum length we can cut in 1 go. Therefore the stage is technically time, but it ranges from $1...N$ since the max number of cuts we can make is just the total length $N$ (just cut 1 unit at a time).

The rest of the variables are straightforward to find: the revenue of a cut is the reward function; final reward is either 0 or $p_1$, and $s_{n+1}$ is the new length after the cut.

Let's list each component:

$t = 1...N$
:   Stage is the "time" elapsed or the total number of cuts we have made

$s_t\in S$  
:   State is the length of the remaining rod

$a_t\in A$  
:   Action the the possible cuts we can make. Ranges from 1 to $s_t$

$r_t(s_t, a_t)$  
:   Reward is the revenue of the cut

$R(s_N)$
:   Base cases, $s_N = 0$ or $s_N = 1$ which means $R(0) = 0, R(1) = p_1$

optimality equation is:

$$
\max_{1\leqslant i\leqslant n}\{\text{CutRod}(n-i) + p_i\}
$$

### Edit Distance

For strings `A[1...n], B[1...m]`

$$
{\text{EditDist}(i, j)} = \begin{cases}
i & \text{if } j = 0\\j & \text{if } i = 0\\
\min \left.\begin{cases}

\text{EditDist}(i, j-1) + 1\\
\text{EditDist}(i-1, j) + 1\\
\text{EditDist}(i-1, j-1) + \begin{cases}1 & \text{if }A[i]\ne B[j]\\
0 & \text{if }A[i]= B[j]
\end{cases}\\

\end{cases}\right\} & \text{Otherwise}
\end{cases}
$$