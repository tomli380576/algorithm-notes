---
order: 97
---


# Maximum Sum After Partitioning

[!badge size="l" variant="warning" text="Leetcode" icon="../assets/leetcode.svg"](https://leetcode.com/problems/partition-array-for-maximum-sum/)

> **Question.** Given an array `A[1...N]` and a partition size `k, 1 <= k <= N`, partition the array into (contiguous) subarrays of length at most k. After partitioning, each subarray has their values changed to become the maximum value of that subarray. What is the largest sum of the given array after partitioning?

## Recurrence

$$
\text{PartitionSum}(i) = \begin{cases}
    0 & \text{if } i = 0\\
    \left.\displaystyle\max_{1\,\leqslant\,j\,\leqslant\, k}\middle\{\text{PartitionSum}(i-j) + \max (A[i - j + 1\dots i]) \cdot j\right\} & \text{otherwise}
\end{cases}
$$

The final answer is `PartitionSum(N)`. Visually, the recurrence describes the follows, (suppose $j=3$):

![](/assets/dp-1/partition-sum.png)

Adjust the indices with -1 offset when implementing this.