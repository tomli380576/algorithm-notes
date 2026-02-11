# Knapsack (Unlimited supply)

!!!question Problem Statement
Given a single backpack with capacity $C$, a list of $N$ items (they now have $\infty$ supply) with weight $w_i$ and value $v_i$, what combination of items that fits in the backpack will maximize the total value?
!!!

## Base cases

The same 2 base cases:
1. No items: $\forall b, K(0, b) = 0$
2. No capacity: $\forall i, K(i, 0) = 0$


## Basic DP solution

The new take-or-skip is "take another copy of item $i$ or move to the next item". To take another copy, **we don't change $i$** unlike the unique items variant:

$$
K(\red{i}, c-w_i) + v_i
$$

To skip item $i$:

$$
K(i-1, c)
$$

Then take the max:

$$
K(i, c) = \max\{K(i, c-w_i) + v_i, K(i-1, c)\}
$$

## Simpler Subproblem

Since the "skip" subproblem is just moving $i$ to $i-1$, we can actually just keep track of the "take" subproblem:

$$
K(c) = \max_{1\leq i\leq N}\{v_i + K(c-w_i): w_i \leq c\}
$$




