---
order: 99
---

# Rod Cutting (122A)

> **Question.** Given a rod with length $n$ and an array of prices of each length $p_i$, how can we cut the rod to make the most amount of money?

`n: int`
:   Length of the rod

`p[1...n]: List<int>`
:   List of prices for rod length from `i=1` to `i=n`. For example: $p_1 = 1, p_2 = 3, p_3 = 5, \dots$
    

We want to find:

- `revenue: int`, the max revenue possible from rod of length $n$
- `cuts: List<int>`, cut strategy to make the max revenue. Element at index $i$ represents the length to cut at step $i$.

Let's call this function `CutRod(n)`. Since the price array `p` is fixed, we will omit `p` from the call signature from now on. 


## Solve the Backtracking Problem

### Base Cases

Recall from basic recursion that the bases cases are the subproblems that we can immediately solve. We know how to solve:

1. $n = 0$, we have no rod, so we make no money. 
   :::center
    `CutRod(0) = 0`
   :::
    
2. $n = 1$, we have the shortest rod that we can possibly sell, so the profit is exactly $p_1$. 
   :::center
    `CutRod(1) = p[1]`
   :::
    

### Recursive Cases

We first consider what a choice is and how to make a choice.

- A choice is the **length** we can cut from the current rod.

So the set of choices is from $1$ to $n$ because they are all valid. To make a choice, we decrease the rod length by our chosen length and add that price to the current total price.

We also want to take the best choice, so we need a max call:

> From the all the possible cuts, take the one that gives the **maximum** profit

### The Recurrence

This problem falls under the [best solution]() case because we want the maximum amount of money.

$$
\text{CutRod}(n)  = \begin{cases}
0&\text{if } n = 0\\
p_1 & \text{if }n = 1\\
\text{best out of all possible cuts} & \text{otherwise}

\end{cases}
$$

Translate it into expressions:

$$
\text{CutRod}(n)  = \begin{cases}
0&\text{if } n = 0\\
p_1 & \text{if }n = 1\\
\displaystyle\max_{1\leqslant i\leqslant n}\Big\{
\text{CutRod}(n-i) +p_i
\Big\} & \text{otherwise}

\end{cases}
$$

+++ Pseudocode

```java
const prices = [...] // given, treat it as a frozen constant array

function CutRod(rodLen: int) -> int:
	if rodLen == 0:
		return 0
	if rodLen == 1:
		return prices[1]
	
	/**
	 * initial value.
	 * start at 0 because we can't have negative profit,
	 * but -Inifinity will also work
	*/
	best = 0 
	
	// we can cut 1, 2, ..., rod_len. Can't cut more rod than we have
	// in all the compatible cuts, which is the next optimal cut?
	for cutLen = 1 to rodLen: 
		// take the cut if it's better than the current decision
		// then let the recursion take care of the rest by shrinking the input
		cutProfit = CutRod(rodLen - cutLen) + prices[cutLen]
		best = max(best, cutProfit)

	return best
```

+++ No Comment Version
    
```java
const prices = [...]

function CutRod(rodLen: int) -> int:
    if rodLen == 0:
        return 0
    if rodLen == 1:
        return prices[1]
    
    best = 0
    for cutLen = 1 to rodLen:
        cutProfit = CutRod(rodLen - cutLen) + prices[cutLen]
        best = max(best, cutProfit)

    return best
```
+++

### :icon-code: Backtracking Implementation

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/backtracking-rod-cutting.py)

!!! Sanity Check
In short, the components of this [recursive backtracking problem](https://www.notion.so/Recursive-Backtracking-Summary-03fc781b681940b290ae3733330dcfde?pvs=21) are:

1. **Sequence of decisions** - We need to make a sequence of cuts.
2. **Make 1 decision at a time** - We make 1 cut at every recursive call.
    - A choice here is the length to cut. All lengths from 1 to the remaining length is valid
    - To make a choice, decrease the remaining length.
3. **Satisfy all the constraints** -  We can’t cut more rod than we currently have.
!!!

Make sure you are comfortable with the brute force approach before moving to DP conversion.

## Convert to Dynamic Programming

### Data structure

Observe that every call requires only 1 argument `rodLen`, so we can use an 1D array to store our results. Let’s define:

```java
dpTable: List<int>
```

To get the result of a previous call, we can use `dpTable[rodLen]`.

- This `dpTable[rodLen]` corresponds to the brute force result `CutRod(rodLen)`.
- If you really wish to make this pretty, name the DP table `cutRod`.

The final result should be in `dpTable[maxRodLen]` where `maxRodLen` is the initial parameter (represents the full rod length)

### Order of evaluation

The function depends on other recursive calls in this way:

```java
// brute force version
function CutRod(rodLen):
	// ...
	cutProfit = CutRod(rodLen - cutLen) + prices[cutLen];
```

Equivalently from the recurrence:

$$
\max_{i\in[1,\,n]}\Big\{
\text{CutRod}(n\red{-i}) +p_i
\Big\}
$$

So we need to evaluate all of $\{\text{CutRod}(n-i)\}_{1\leqslant i \leqslant n}$ first before we can know the result of `CutRod(n)`. Since $n-i < n$, the first thing that goes into the table is `CutRod(0)` and we evaluate in ascending order. 

### Build the DP Table

+++ Pseudocode

```java
const prices = [...] // given

function DpCutRod(maxRodLen: int) -> int:
	dpTable = Array(shape=(maxRodLen)) // preallocate the space
	
	// pretend 0 is valid here
	dpTable[0] = 0          // corresponds to CutRod(0)
	dpTable[1] = prices[1]  // corresponds to CutRod(1)
	
	// ↓ the for loop that simulates recursive calls
	for rodLen = 2 to maxRodLen:
		best = 0
		// ↓ the exact same for loop from backtracking, but now using dpTable
		for cutLen = 1 to rodLen:
			profit = dpTable[rodLen - cutLen] + prices[cutLen]
			best = max(best, profit)
		// recursive call location, store the result of CutRod(rodLen)
		dpTable[rodLen] = best
	
	// initial call location
	return dpTable[maxRodLen] // Corresponds to CutRod(maxRodLen)
```

+++ No Comment Version
    
```java
const prices = [...]

function DpCutRod(maxRodLen: int) -> int:
    dpTable = Array(shape=(maxRodLen))
    
    dpTable[0] = 0
    dpTable[1] = prices[1]
    
    for rodLen = 2 to maxRodLen:
        best = 0
        for cutLen = 1 to rodLen:
            profit = dpTable[rodLen - cutLen] + prices[cutLen]
            best = max(best, profit)
        dpTable[rodLen] = best
    
    return dpTable[maxRodLen]
```
+++

### Reconstructing the Solution

The pseudocode above only finds the optimal profit, but it doesn't tell us how to actually cut the rod. We can record the cut by adding the following

```c #12-14,3
function DpCutRod(maxRodLen: int) -> int, List<int>:
    dpTable = Array(shape=(maxRodLen))
    cuts = Array(shape=(maxRodLen))
    
    dpTable[0] = 0
    dpTable[1] = prices[1]
    
    for rodLen = 2 to maxRodLen:
        best = 0
        for cutLen = 1 to rodLen:
            profit = dpTable[rodLen - cutLen] + prices[cutLen]
            if profit > best:
                best = profit
                cuts[rodLen] = cutLen

        dpTable[rodLen] = best
    
    return dpTable[maxRodLen], elements of cuts that are not null

```


### :icon-code: DP Implementation

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/DP-rod-cutting.py)

## Runtime Analysis

We can see that this is the classic nested for loop:

```java
for rodLen = 2 to maxRodLen:
	for cutLen = 1 to rodLen:
		// some O(1) work
```

So the runtime is $O(n^2)$ and memory is $O(n)$ from 1D Array.

## Proof. Optimal Substructure

1. State the given:
    - Given $\text{OPT}(n)$ for rod of length $n$ with first cut $i$
    - $\text{OPT}(n)=p_i + a$, where $a$ is the amount of money made on rod of length $n-i$

2. Prove: $a$ is the optimal solution for rod of length $n-i$, or $a = \text{OPT}(n-i)$
3. Assume $a$ is not optimal for rod of length $n-i$
    
    Let $rev(t)$ denote the revenue from strategy $t$
    
    Then there exists another strategy $b$ such that $rev(b) > rev(a)$
    

$$
\begin{aligned}
    \underbrace{rev(b) + p_i}_\text{revenue of entire rod} &> \underbrace{rev(a) + p_i}_\text{given assumption}
    \\
    rev(\text{strategy of length $n$}) &>\text{OPT}(n)
\end{aligned}
$$

We already assumed OPT is the best solution, then a better one cannot exist.

Contradiction with given. Therefore $a$ is optimal for $n-i$.

!!!success Hint
The key point is that the *optimal solution to the subproblem is unique*.
!!!

