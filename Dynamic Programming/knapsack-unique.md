# Knapsack (Unique items)

> **Question.** Given a single backpack with capacity $C$, a list of $N$ unique items (can only take each once) with weight $w_i$ and value $v_i$, what combination of items that fits in the backpack will maximize the total value?

Let's first formalize what we want to maximize. Let the final selection of items be $S$,
$$
\begin{aligned}
\max &\sum_{i\in S} v_i \\
\text{such that} &\sum_{i\in S} w_i \leq C
\end{aligned}
$$

## Base cases

2 base cases:
1. No items: $\forall b, K(0, b) = 0$
2. No capacity: $\forall i, K(i, 0) = 0$

## Subproblems

Recall that we want to consider some sort of "prefix" of the problem. Notice that we have this "list" of items, so we can label them arbitrarily from $1\dots N$. We can also put all capacities in a "list" from 0 to $C$. In this case we can consider the first $i$ items out of all $N$ and all weights in $0 \dots C$. 

Let $K(i, c)$ be the maximum value we can get from using items $1\dots N$ with capacity $c$. which means the final answer is $K(N, C)$.

## Take or Skip

Now we have the subproblem, we want to **try to take each item and see if it makes it better**. Also don't forget that we can only take the item if it fits: $w_i \leq c$

To "take" an item  $i$, we decrease the capacity by its weight and move to the next item -> we will need the answer of $K(i-1, c-w_i)$, then add the value of the item itself $v_i$
- Note that we go to i-1 because we can only use **up** to item i, so we have to go to a lower index.

To "skip" item $i$, we just move the index: $K(i-1, c)$

Finally we take the better one:

$$
\max\left\{K(i-1, c-w_i) + v_i, K(i-1, c)\right\}
$$

## Pseudocode

```
KnapsackUnique(w[1...N], v[1...N], C):
    K = [][] # do array init here
    
    for i = 1...N:
        K[i][0] = 0
    for c = 0...C:
        K[0][C] = 0

    for i = 1...N:
        for c = 0...C:
            if w[i] <= c:
                take = K[i - 1, c - w[i]] + v[i]
                skip = K[i - 1, c]
                K[i][c] = max(take, skip)

    return K[N, C]
```

## Running time (NP Complete)

This is just plain nested for loops so the runtime is $O(NC)$.

!!!danger Not actually Polynomial Time
Notice that N and C are not counting the same type of item. C can be way larger than N.
!!!