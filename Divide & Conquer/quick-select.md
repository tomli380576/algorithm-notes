# Quick Select (122B)

> **Question.** Given an unsorted `A[1...N]: List<number>` and a positive integer `k`, `1 <= k <= N`, how do we find the $k$-th smallest element of $A$ in $O(n)$ time?

For example, `A = [3,2,1,5,6,4], k = 2` gives us `2`.

## One Armed Quick Select

The idea is similar to quick sort. Partition `A` by some pivot, then recursively search the left and right half with respect to the pivot.

- Let the pivot value be `p`, pivot index be `pivot_index`. Recall that `A[i]` < `p` if `i < pivot_index`, and `A[i] >= p` if `i >= pivot_index`

```c
function quickSelect(A: [1...N], k: int) -> int:
    pivot_value = selectPivot(A) // black box subroutine
    
    small, large = [], []

    for a in A:
        if a < pivot_value:
            small.push(a)
        else if a > pivot_value:
            large.push(a)

    s = len(small)
    l = len(large)

    if k < s:
        return quickSelect(small, k)
    else if k > N - s:
        return quickSelect(large, k - (N - s))
    else:
        return pivot_value
```

## Example Walkthrough

For `A = [3,2,1,5,6,4], k = 2`, we have:

```
N = 6
pivot_value = 4
small = [3, 2, 1]
large = [5, 6]

2 < len(small), call quickSelect(small, 2)

---

N = 3
pivot_value = 1
small = []
large = [3, 2]

Neither 2 < 0 nor 2 > 3 - 0, so we hit the else case
return 1

```


## Decide which half to search

If `p` is the pivot value, then after `partition()`, the array `A` looks like this:

$$
\tt \underbrace{\overbrace{...}^{\text{values < p}}}_{\text{left half}},\;p\dots p,\;\underbrace{\overbrace{...}^{\text{values > p}}}_{\text{right half}}
$$

Notice that there's a chunk of `p` next to each other in the middle. Recall that the idea is to recursively search the left and right half. If `k < len(left)`, then the `k`th smallest is in `left`, so we just do the recursive call on `left`.

If is in the right half,
