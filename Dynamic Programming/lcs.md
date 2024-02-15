---
order: 98
---

# Longest Common Subsequence

[!badge size="l" variant="warning" text="Leetcode" icon="../assets/leetcode.svg"](https://leetcode.com/problems/longest-common-subsequence/description/)

> **Question.** Given 2 strings `X[1...m], Y[1...n]`, what is the longest common subsequence between them?
> A common sequence satisfies the following:
> - They appear in the same order as they do in the original sequence
> - Doesn't need to be contiguous

Example: `X = "DCUT", Y = "DUTC"`, then `LCS(X, Y) = "DUT"`

## 1. Solve the Backtracking Problem First

### 1.1 Function signature

Using the example above, let’s align the common letters:

$$
\begin{matrix}
X : &\tt{D}&\tt{C} & \tt{U}& \tt{T} &\\

&\mid & &\mid & \mid&\\

Y:&\tt{D}&&\tt{U} & \tt{T}& \tt{C}\\
\end{matrix}
$$

We check 1 pair of characters at a time. Let’s use $i$ for indexing $X$ and $j$ for indexing $Y$.

$$
\underbrace{\xrightarrow[

\begin{matrix}
&\green {\overset{\small{\text{match}}}{\Downarrow}}\\

X:&\tt{\green D}&\tt{C} & \tt{U}& \tt{T} &\\

&\mid & &\mid & \mid&\\

Y:&\tt{\green D}&&\tt{U} & \tt{T}& \tt{C}\\
\end{matrix}]{\text{Scan Direction}}}_{\text{After check: }
\texttt{i += 1, j += 1}}
$$

To “take” a pair, we advance the indices for both strings. So $j$ is pointing at `U` right now:

$$
\begin{matrix}
&&\overset{\small{{i}}}{\small \Downarrow}\\

X:&\tt{D}&\tt{\red C} & \tt{U}& \tt{T} &\\

&\mid & &\mid & \mid&\\

Y:&\tt{D}&&\tt{\red U} & \tt{T}& \tt{C}\\
&&&\underset{\small{{j}}}{\small\Uparrow}
\end{matrix}
$$

That’s a mismatch, we need to skip. To "skip" a pair, we advance only 1 index, either $i$ or $j$. Here the correct choice is $i$.

$$
\underbrace{\begin{matrix}
&&\to&\overset{\small{{i}}}{\small \Downarrow}\\

X:&\tt{D}&\tt{C} & \tt{\green U}& \tt{T} &\\

&\mid & &\mid & \mid&\\

Y:&\tt{D}&&\tt{\green U} & \tt{T}& \tt{C}\\
&&&\underset{\small{{j}}}{\small\Uparrow}
\end{matrix}}_{\text{skip }i}
\hspace {2cm}
\underbrace{\begin{matrix}
&&\overset{\small{{i}}}{\small \Downarrow}\\

X:&\tt{D}&\tt{\red C} & \tt{U}& \tt{T} &\\

&\mid & &\mid & \mid&\\

Y:&\tt{D}&&\tt{U} & \tt{\red T}& \tt{C}\\
&&&\to&\underset{\small{{j}}}{\small\Uparrow}
\end{matrix}}_{\text{skip } j}
$$

On the graph it might look obvious, but we won’t know that $i$ is correct until the recursion result comes back. So from the algorithm's point of view it probably looks like this at current recursion level:

$$
\underbrace{\begin{matrix}
&&&\overset{\small{{i}}}{\small \Downarrow}\\

X:&\tt{D}&\tt{C} & \tt{\green U}& \blacksquare &\blacksquare\\

&\mid & && &\\

Y:&\tt{D}&&\tt{\green U} & \blacksquare& \blacksquare\\
&&&\underset{\small{{j}}}{\small\Uparrow}
\end{matrix}}_{\text{skip }i}
\hspace {2cm}
\underbrace{\begin{matrix}
&&\overset{\small{{i}}}{\small \Downarrow}\\

X:&\tt{D}&\tt{\red C} & \blacksquare& \blacksquare &\blacksquare\\

&\mid & && \mid&\\

Y:&\tt{D}&&\tt{U} & \tt{\red T}& \blacksquare\\
&&&&\underset{\small{{j}}}{\small\Uparrow}
\end{matrix}}_{\text{skip } j}
$$

We don’t know what’s the length of LCS in the blacked out areas, so we continue until one of the strings becomes empty. Suppose we chose to skip $i$:

$$
\left.\underbrace{\begin{matrix}
&&\green{\overset{\footnotesize{\text{match}}}{\Downarrow}}\\
\tt{D}&\tt{C} & \tt{U}& \tt{T} &\\

\mid & &\mid & \mid&\\

\tt{D}&&\tt{U} & \tt{T}& \tt{C}\\
\end{matrix}}_\texttt{i += 1, j += 1}
\hspace{1.25cm}
%\middle |
%\hspace{0.5cm}
\underbrace{\begin{matrix}
&&&\green{\overset{\footnotesize{\text{match}}}{\Downarrow}}\\
\tt{D}&\tt{C} & \tt{U}& \tt{T} &\\

\mid & &\mid & \mid&\\

\tt{D}&&\tt{U} & \tt{T}& \tt{C}\\
\end{matrix}}_\texttt{i += 1, j += 1}

\hspace{1.25cm}
%\middle|
%\hspace{0.25cm}
\underbrace{\begin{matrix}
&&&&\red{\overset{\footnotesize{\text{end}}}{\Downarrow}}\\
\tt{D}&\tt{C} & \tt{U}& \tt{T} &\tt{\purple{\backslash0}}\\

\mid & &\mid & \mid&\\

\tt{D}&&\tt{U} & \tt{T}& \tt{C}\\
\end{matrix}}_\texttt{base case}
\right.
$$

Now we know what the function should look like:

:::center
`function LCS(i: int, j: int) -> int`
:::

The return value is the length of the longest common subsequence.

### 1.2 Base cases

1. If either string is empty, then their common subsequence is the empty string with 0 length.
2. If either `i` or `j` goes out of bounds, common subsequence is nothing $\implies$ length is also 0.

Therefore: 
$$
\begin{aligned}\text{LCS}&(i, \red {n + 1}) &= 0\\
\text{LCS}&(\red {m + 1}, j) &= 0
\end{aligned}
$$

### 1.3 Recursive Cases

Let’s first consider what a choice is. **A choice is whether to take or skip a character in the string.**

There are only 2 valid choices at a time:

1. If the current character matches, then we definitely *take* from both
2. If the current character doesn’t match, then we must *skip* either $i$ or $j$.

Now we consider how to **make a choice**. From 1.1 we observed that:

- If we want to take the current pair, we advance both $i$ and $j$.
- If we skip, we try to advance $i$ or $j$. Try both and we will take the better one with a `max(...)` call.

### 1.4 Building the recurrence

Combining 1.2 and 1.3, we can write:

$$
\text{LCS}(i, j) = \begin{cases}
0 & \text{if $i = m+1$ or $j = n+1$} \\
\text{Take both }&\text{if } X[i] = Y[j]\\
\max\left.\begin{cases}
\text{Skip }i\\
\text{Skip }j\\
\end{cases}\right\}
&\text{if }X[i]\ne Y[j]
\end{cases}
$$

Convert to expressions:

$$
\text{LCS}(i, j) = \begin{cases}
0 & \text{if $i = m+1$ or $j = n+1$} \\
\text{LCS}(i + 1, j + 1) + 1&\text{if } X[i] = Y[j]\\
\max\left.\begin{cases}
\text{LCS}(i+1, j)\\
\text{LCS}(i, j + 1)\\
\end{cases}\right\}
&\text{if }X[i]\ne Y[j]
\end{cases}
$$

In pseudocode:

```c
// Givens
const X[1...m]: string = ...
const Y[1...n]: string = ...

function LCS_length(i: int, j: int) -> int:
	if i == m + 1 or j == n + 1:
		return 0
	if X[i] == Y[j]:
		return LCS_length(i + 1, j + 1) + 1  // always take if characters match
	else:
		skipI = LCS_length(i + 1, j)  // try to skip i
		skipJ = LCS_length(i, j + 1)  // try to skip j
		return max(skipI, skipJ)
```

### 1.5 Python: Backtracking LCS

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/backtracking-LCS.py)

!!! Sanity Check

1. We are making exactly 1 choice at each recursion level:
   1. **Take** when `X[i] == Y[j]`.
   2. **Skip** when `X[i] != Y[j]`. Compute the results of skipping $i$ and $j$, then take the better one.
2. Take is the only valid choice when `X[i] == Y[j]`.

!!!

---

## 2. Convert to Dynamic Programming

### 2.1 Data Structure

LCS needs 2 arguments, so let’s use a 2D array. Let’s define:

:::center
`dpTable = Array(shape=(m + 1, n + 1))`
:::

To get the result of a previous call, we can use `dpTable[i, j]`.

### 2.2 Order of Evaluation

From the recurrence above, the $\text{LCS}(i, j)$ call requires:

- $\text{LCS}(i+1, j + 1)$
- $\text{LCS}(i+1, j)$
- $\text{LCS}(i, j+1)$

Since $i + 1 > i$ and $j + 1 > j$, both $i$ and $j$ needs to go from **high values to low values**, which means we use reversed for loops for both. The nesting order doesn’t matter here because $i$ and $j$ don’t depend on each other.

In pseudocode:

+++ With Comments 
```c
function LCS_length_DP(X[1...m], Y[1...n]) -> int:
	dpTable = Array(shape=(m + 1, n + 1))

	// base cases
	for j = 1 to n + 1:
		dpTable[m + 1, j] = 0  // LCS(m+1, j)
	for i = 1 to m:
		dpTable[i, n + 1] = 0  // LCS(i, n+1)

	// Recursive cases & order of evaluation
	for i = m to 1:
		for j = n to 1:
			if X[i] == Y[j]:
				//  LCS(i, j) = LCS(i+1, j+1) + 1 if X[i] == Y[j]
				dpTable[i, j] = dpTable[i + 1, j + 1] + 1
			else:
				skipI = dpTable[i + 1, j]  // LCS(i+1, j)
				skipJ = dpTable[i, j + 1]  // LCS(i, j+1)
				dpTable[i, j] = max(skipI, skipJ)
				// LCS(i, j) = max(LCS(i+1, j), LCS(i, j+1))

	return dpTable[1, 1]  // LCS(1, 1) initial call
```

+++ No Comment Version

```c
function LCS_length_DP(X[1...m], Y[1...n]) -> int:
	dpTable = Array(shape=(m + 1, n + 1))

	for j = 1 to n + 1:
		dpTable[m + 1, j] = 0
	for i = 1 to m:
		dpTable[i, n + 1] = 0

	for i = m to 1:
		for j = n to 1:
			if X[i] == Y[j]:
				dpTable[i, j] = dpTable[i + 1, j + 1] + 1
			else:
				skipI = dpTable[i + 1, j]
				skipJ = dpTable[i, j + 1]
				dpTable[i, j] = max(skipI, skipJ)

	return dpTable[1, 1]
```
+++

#### For 122A students

Note that if we do the check direction **backwards** (last character to first), then the order of evaluation is reversed. This reversed order yields the original pseudocode from 122A.
- Basically flip the scan line in this [graph](#11-function-signature).

The backwards recurrence is:

$$
\text{RevLCS}(i, j) = \begin{cases}
0 & \text{if\red{ $i = 0$ or $j =0$}} \\
\text{Take both }&\text{if } X[i] = Y[j]\\
\max\left.\begin{cases}
\text{Skip }i\\
\text{Skip }j\\
\end{cases}\right\}
&\text{if }X[i]\ne Y[j]
\end{cases}
$$

In expressions:

$$
\text{RevLCS}(i, j) = \begin{cases}
0 & \text{if $i = 0$ or $j = 0$} \\
\text{RevLCS}(i -1, j -1) + 1&\text{if } X[i] = Y[j]\\
\max\left.\begin{cases}
\text{RevLCS}(i-1, j)\\
\text{RevLCS}(i, j -1)\\
\end{cases}\right\}
&\text{if }X[i]\ne Y[j]
\end{cases}
$$

Both $i$ and $j$ now need to go from **low values to high values**, hence the normal for loops.

## :icon-code: 3. Python: DP Longest Common Subsequence

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/DP-LCS.py)

- $O(n^2)$ runtime from the nested for loop, $O(n^2)$ space for the 2D array

## 4. Recovering the Matches

So far our solution correctly computes the length, but we don’t know which characters are in the final LCS. More formally, how do we record the matching pair of indices $i, j$ as the loops in the DP solution progresses?

### 4.1 Recording the Choices

We can use a technique called **parent pointers**. This technique will show up again in [SSSP]().

Recall from the recurrence that for all possible pairs of $i,j$, we always make a choice if we are not at the base case:

$$
\text{LCS}(i, j) = \begin{cases}
0 & \text{if $i = m+1$ or $j = n+1$} \\
\text{\red{Take} both }&\text{if } X[i] = Y[j]\\
\max\left.\begin{cases}
\text{\red{Skip} }i\\
\text{\red{Skip} }j\\
\end{cases}\right\}
&\text{if }X[i]\ne Y[j]
\end{cases}
$$

We can record the choices with a map, where the keys are the input parameters and the values are the choices:

```java
lcsChoices: Map<Inputs, Choices>
```

We can add the choice-recording step:

```c !#9,14,20,23
function LCS_length_DP(X[1...m], Y[1...n]) -> number:
	dpTable = Array(shape=(m + 1, n + 1))

	for j = 1 to n + 1:
		dpTable[m + 1, j] = 0
	for i = 1 to m:
		dpTable[i, n + 1] = 0

	lcsChoices = Map()
	for i = m to 1:
		for j = n to 1:
			if X[i] == Y[j]:
				dpTable[i, j] = dpTable[i + 1, j + 1] + 1
				lcsChoices[i, j] = [i + 1, j + 1]
			else:
				skipI = dpTable[i + 1, j]
				skipJ = dpTable[i, j + 1]
				if skipI > skipJ:
					dpTable[i, j] = skipI
					lcsChoices[i, j] = [i + 1, j]
				else:
					dpTable[i, j] = skipJ
					lcsChoices[i, j] = [i, j + 1]

	return dpTable[1, 1]
```

Since we are iterating over all combinations of $i$ and $j$, the `lcsChoices` map will have exactly $m\cdot n$ entries. Assuming we are not accounting for any additional memory overhead, the total space complexity remains $O(mn)$.

### 4.2 Following the parent pointers

When we store the result `lcsChoices[i, j] = [i + 1, j]`, we are saying that “the next best choice for $\text{LCS}(i,j)$ is $(i+1, j)$”.

The initial call for $\text{LCS}$ is $i=1,j=1$, so we start with this entry and follow the parent pointers until we hit the base case:

```c
function recoverMatches(parentPointers: Map) -> List, List:
	i, j = [1, 1]  // initial call params, corresponds to LCS(1, 1)
	matchIdxX = [] // matching indices in X
  	matchIdxY = [] // matching indices in Y

	// while curr is still a key, if curr is the base case then it's not a key
	while (i, j) in parentPointers:
		if X[i] == Y[j]:
			matchIdxX.push(i)
			matchIdxY.push(j)
		// follow the parent pointer, this is saying:
		// LCS(i, j) made the choice parentPointers[curr]
		i, j = parentPointers[i, j]
	return matchIdxX, matchIdxY
```

The while loop condition works because these loops from DP never go beyond $m$ and $n$:

```c
for i = m to 1:
	for j = n to 1:
```

$(m, n)$ will still be a key in `lcsChoices`, but none of its possible values, stored in `lcsChoices[m, n]`, will be a key:

$$
(m+1, n+1)\\
(m, n+1)\\
(m+1, n)
$$

so we are guaranteed that this loop will stop.

### Implementation Detail

We are taking a shortcut here by assuming `lcsChoices[i, j]` is a valid key. This is valid in python because tuples are hash-able, but it might not be the case for other languages.

In that case we can always use a 2D array or a nested hash map and access by integer indices like `lcsChoices[i][j]`
