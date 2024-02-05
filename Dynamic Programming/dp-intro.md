---
order: 100
---

# Introduction

Typically a DP problem involves searching for a sequence of decisions that arrives at a maximum or minimum value. Let's define:

$t = 1...N$
:   A "stage" of the problem. When $t=N$, we have reached the end of the problem

$s_t\in S$  
:   State at stage $t$; $S$ is the space of all possible states

$a_t\in A$  
:   Action at stage $t$; $A$ is the space of all possible actions

$r_t(s_t, a_t)$  
:   Reward function if we take action $a_t$ at state $s_t$

$R(s_N)$
:   Reward if we end on state $s_N$. This is the cost/reward of our base case, a subproblem that we can immediately solve without recursion.

Now the optimality equation / value function is:

$$
v_t(s_t) = \left.\underset{a_t\in A}{\max/\min}\,\middle\{r_t(s_t, a_t) + v_{t+1}(s_{t+1})\right\}
$$
