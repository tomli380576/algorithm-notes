---
order: 96
---

# Pretty Printing

[!badge size="l" variant="warning" text="Leetcode" icon="../assets/leetcode.svg"](https://leetcode.com/problems/text-justification/description/)

> **Question.** Given an array of `N` words and a maximum page width, what is the most optimal printing strategy that minimizes the total cost? Assume that any individual word can fit on one line.
> 

`words[1...N]: List<string>`
:   the words we want to print neatly on a page

`page_width: int`
:   the maximum number of characters on a single line

`cost: (spaces: int) -> int`
:   a function that measures how "bad" a line is. The more empty spaces there are, the worse a line is. Let’s use $x^3$ here, where $x$ is the number of trailing spaces.

We want to find:

`List<int>`
:   a list of line break indices

`int`
:   the total cost after applying the line breaks


Depending on the variant of this question, sometimes the last line is free, which means we don’t calculate the cost of the final line.

!!! Def. Line Break

To avoid weird indexing problems, let’s define a **break** at index $i$ to mean putting $\tt words[i]$ on a new line.
!!!

## 1. Solve the Backtracking Problem


### 1.1 Function Signature & Base Case

To keep track of how many words are remaining, we need an index `j` to indicate **the last word** that we need to place onto the paper. So the function looks like:

:::center
`PrettyPrint(j: int) -> int`
:::

and the initial call will be:

:::center
`PrettyPrint(N)`
:::

There’s only 1 base case hereL: when there are no words, then there’s no cost, return 0.

:::center
`PrettyPrint(0) = 0`
:::

In this case we are scanning from the last word to the first word, but reversing the scanning order also works. Flip the base case and initial call accordingly.

### 1.2 Line Cost

From the problem inputs, we know that the cost of a line is given by:

```c
function cost(spaces) {
    return pow(spaces, 3) // raise to 3rd power
}
```

Then number of empty spaces is:

:::center
`num_empty_spaces = page_width - sentence_length`
:::

For the length of the sentence, we also need to count the number of spaces between the words.

Let’s define a function `LineCost(i, j)`.

- $i$ is the index of the first word on this line
- $j$ is the index of the last word on this line
- Both $i$ and $j$ are indices for the $\texttt{words}$ array

$$
\text{LineCost}(i, j) = \begin{cases}
\infty&\text{if the words doesn't fit}\\
0 &\text{if the words fit and $j$ is the last}\\
(\text{num empty spaces})^3 & \text{Otherwise}

\end{cases}
$$

!!!info
This is a bit different from the previous backtracking problems. We are incorporating the `find valid choices` step into this cost function by using $\infty$ as 1 of the costs.
!!!

To compute the number of empty spaces, lets define:

$$
\text{Extras}(i, j) =  \texttt{page\_width} -\underbrace{ (j-i)}_\text{space between words} - \underbrace{\sum_{k=i}^j \texttt{len(words[k])}}_\text{actual letters}
$$

where $i, j$ are the indices of the original words array. We can see that computing $\text{Extras}$ will take $O(n)$ time where $n$ is the number of words. 

```c
const pageWidth = ...
const words = [...]

function Extras(i: int, j: int) -> int:
	numSpacesBetweenWords = j - i
	numLetters = 0

	for word in words:
		numLetters += len(word)
	
	return pageWidth - numSpaces - numLetters
```

Now we can translate $\text{LineCost}$ into expressions:

$$
\text{LineCost}(i, j) = \begin{cases}
\infty&\text{if Extras}(i, j) < 0\\
0 &\text{if Extras}(i, j) \ge 0\text{ and }j= n\\
\text{Extras}(i, j)^3 & \text{Otherwise}

\end{cases}
$$

```c
function LineCost(i, j) -> int:
	extras = Extras(i, j)
	if extras < 0:
		return Infinity
	else if extras >= 0 and j == n:
		return 0 // last line is free
	return pow(extras, 3)
```

So $\text{LineCost}$ also runs in $O(n)$ time.

### 1.3 Recursive Cases

This falls under the **Best Solution** case with the `FindNext` strategy, so we need:

1. Choices
    
    - Every index before the current index is a choice. We can place a line break at any of them.
    - The "valid" choice part is handled by the $\text{LineCost}$ function.
    - If the choice is not valid, $\text{LineCost}$ will return $\infty$ and it’s definitely not going to be selected.
    
    <p></p>
2. How to make a choice
    
    To make a choice, we go to the "next" word we want to place.  We can either go from the end or go from the beginning.
    
    - If we go from the end $j=n$, the next one is $j-1$.
    - If we go from the beginning $j=1$, the next one is $j+1$.

Since we want to minimize the total cost, we will also need a `min(...)` call

### 1.4 Recurrence & Pseudocode

Combining 1.1 ~ 1.3, the recurrence is:

$$
\text{PrettyPrint}(j)  =\begin{cases}
0 &\text{if }j = 0\\
\displaystyle\min_{\text{possible breaks }i}\left\{\text{total cost before break}  + \text{LineCost}(i, j)\right\}&\text{Otherwise}

\end{cases}
$$

In expressions:

$$
\text{PrettyPrint}(j)  =\begin{cases}
0 &\text{if }j = 0\\
\displaystyle\min_{1\le i\le j}\left\{\text{PrettyPrint}(i-1)  + \text{LineCost}(i, j)\right\}&\text{Otherwise}

\end{cases}
$$

Convert to pseudocode:

```c
const words = [...]
const pageWidth = ...


function Extras(i: int, j: int) -> int:
	numSpacesBetweenWords = j - i
	numLetters = 0

	for word in words:
		numLetters += len(word)
	
	return pageWidth - numSpaces - numLetters


function LineCost(i, j) -> int:
	extras = Extras(i, j)
	if extras < 0:
		return Infinity
	else if extras >= 0 and j == n:
		return 0 // last line is free
	return pow(extras, 3)


function PrettyPrint(j) -> int:
	if j == 0:
		return 0
	
	best = Infinity
	for i = 1 to j:
		breakHereCost = PrettyPrint(i - 1) + LineCost(i, j)
		best = min(breakHereCost, best)
	
	return best
```

Don’t forget $i\red{-1}$ in the recursive call, otherwise some words are considered multiple times.

### :icon-code: 1.5 Python. Backtracking Pretty Printing

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/backtracking-pretty-printing.py)

!!! **Sanity Check**

1. At each recursion level, we “choose” a line break
2. All indices before the current $j$ is a “valid” choice
3. We take the `min` from all the choice results

Make sure you feel comfortable with the brute force solution before moving on to section 2.

!!!

## 2. Convert to Dynamic Programming

### 2.1 Order of Evaluation & Data Structure

From the recurrence:

$$
\text{PrettyPrint}(j)  =\begin{cases}
0 &\text{if }j = 0\\
\displaystyle\min_{\red{1\le i\le j}}\left\{\text{PrettyPrint}(i-1)  + \text{LineCost}(i, j)\right\}&\text{Otherwise}

\end{cases}
$$

Unwrapping the $\min$ call shows that $\text{PrettyPrint}(j)$ needs: `@Hint` Plug in possible values of $i$.

- $\text{PrettyPrint}(0)$
- $\text{PrettyPrint}(1)$
- …
- $\text{PrettyPrint}(j-1)$

We need to compute the function results of the **smaller** $j$’s first, thus the order of evaluation for $j$ goes in **ascending order**.

The function only needs 1 parameter, so we can use a 1D array.

### 2.2 Pseudocode

+++Pseudocode

```c
function PrettyPrint_DP(words[1...N], page_width) -> int:
	dpTable = Array(shape=(N + 1))
	// Pretend 0 is a valid index here
	// we can put it at the end or handle it separately
	dpTable[0] = 0 // PrettyPrint(0) = 0
	
	// for loop for simulating recursive calls, iterate through: 
	// PrettyPrint(0), PrettyPrint(1), PrettyPrint(2), PrettyPrint(3) 
	for j = 1 to N:
		best = Infinity
		// for loop from the backtracking solution
		for i = 1 to j:
			// PrettyPrint(i - 1) + LineCost(i, j)
			breakHereCost = dpTable[i - 1] + LineCost(i, j) 
			best = min(breakHereCost, best)
		dpTable[j] = best

	return dpTable[N] // PrettyPrint(N)
```
    
+++ No comment version
    
```c
function PrettyPrint_DP(words[1...N], page_width) -> int:
    dpTable = Array(shape=(N + 1))
    dpTable[0] = 0

    for j = 1 to N:
        best = Infinity
        for i = 1 to j:
            breakHereCost = dpTable[i - 1] + LineCost(i, j) 
            best = min(breakHereCost, best)
        dpTable[j] = best

    return dpTable[N]
```

+++

Time complexity is $O(n^3)$ and space complexity is $O(n)$.

## ❖ 3. Memoizing `LineCost` and `Extras`

Calling $\text{LineCost}$ inside the nested for loops can be expensive, so we should isolate that from the main memoization loop.

$\text{LineCost}$ needs 2 parameters, so we can use a 2D array. But directly going through all combinations of $i, j$ doesn’t really help because that’s still $O(n^3)$

```c
lineCost = Array(shape=(N + 1, N + 1))

for i = 1 to N:
	for j = 1 to N:
		lineCost[i, j] =...// normal LineCost evaulation here is O(n)

// total work is still O(n^3)
```

So we need to somehow use previous results to make the evaluation `lineCost[i, j]` $O(1)$.

**Observe that the actual work happens inside $\text{Extras}(i, j)$.** Instead of recomputing the total number of characters every time we increase $j$, we can build on top of the previous result like this:

$$
\underbrace{\text{Extras}(i, j)}_{\text{including }\texttt{word[j]}} = \underbrace{\text{Extras}(i, j-1)}_{\text{without }\texttt{word[j]}} - \underbrace{1}_\text{space} - \underbrace{\text{len(words[$j$])}}_\text{\texttt{word[j]} itself}
$$

We can assume the $\text{len}$ function for finding the number of characters is $O(1)$ because the work is done before the $\text{PrettyPrint}$ routine starts.

The base case is a single word:

:::center
`Extras(i, i) = page_width - len(words[i])`
:::

$\text{Extras}(i, j)$ requires $\text{Extras}(i, j-1)$, so the order of evaluation goes in **ascending order**:

```c
extras = Array(shape=(N + 1, N + 1))
for i = 1 to N:
	for j = i to N:
		if j == i:　// Extras(i, i)
			extras[i, j] = pageWidth - len(words[i])
		else:
			extras[i, j] = extras[i, j-1] - 1 - len(words[j]) // recurrence
```

Now memoizing $\text{Extras}$ becomes $O(n^2)$.

## 4. Putting everything together

The recurrences are:

$$
\begin{aligned}
    \text{PrettyPrint}(j) &= \begin{cases}
        0 &\text{if }j = 0\\[5pt]
    \displaystyle\min_{1\le i\le j}\left\{\text{PrettyPrint}(i-1)  + \text{LineCost}(i, j)\right\}&\text{Otherwise}

    \end{cases}
\\[25pt]
    \underbrace{\text{Extras}(i, j)}_{\text{including }\texttt{word[j]}} &= \underbrace{\text{Extras}(i, j-1)}_{\text{without }\texttt{word[j]}} - \underbrace{1}_\text{space} - \underbrace{\text{len(words[$j$])}}_\text{\texttt{word[j]} itself}
\end{aligned}
$$


Line cost itself isn’t really a recurrence but it’s important:

$$
\text{LineCost}(i, j) = \begin{cases}
\infty&\text{if Extras}(i, j) < 0\\
0 &\text{if Extras}(i, j) \ge 0\text{ and }j= n\\
\text{Extras}(i, j)^3 & \text{Otherwise}

\end{cases}
$$

+++ Pseudocode
```c
function PrettyPrint_DP_Full(words[1...N], pageWidth: int) -> int:
	totalCost = Array(shape=(N + 1))
	// base case: PrettyPrint(0) = 0
	totalCost[0] = 0 
	
	// Memoizing Extras(i, j)
	extras = Array(shape=(N + 1, N + 1))
	for i = 1 to N:
		for j = i to N:
			if j == i: // base case: Extras(i, i)
				extras[i, j] = pageWidth - words[i].length
			else:
				extras[i, j] = extras[i, j-1] - 1 - words[j].length
	
	// Memoizing the main recurrence
	for j = 1 to N: // PrettyPrint(1), PrettyPrint(2) ... PrettyPrint(N)
		best = Infinity // simulate min(…)

		for i = 1 to j:
			// simulate LineCost(i, j)
			lineCost = Infinity
			emptySpaces = extras[i, j]
			
            if numTrailingSpaces > 0:
				if j == N:
					lineCost = 0
				else:
					lineCost = pow(emptySpaces, 3)
			
            // Simulate PrettyPrint(i-1) + LineCost(i, j)
			breakHereCost = totalCost[i - 1] + lineCost
			best = min(breakHereCost, best) 

		totalCost[j] = best // the result of PrettyPrint(j)
	
	// Initial call: PrettyPrint(N)
	return totalCost[N]
```    
+++ No comment version

```c
function PrettyPrint_DP_Full(words[1...N], pageWidth: int) -> int:
    totalCost = Array(shape=(N + 1))
    totalCost[0] = 0 
    
    extras = Array(shape=(N + 1, N + 1))
    for i = 1 to N:
        for j = i to N:
            if j == i:
                extras[i, j] = pageWidth - words[i].length
            else:
                extras[i, j] = extras[i, j-1] - 1 - words[j].length
    
    for j = 1 to N:
        best = Infinity
        
        for i = 1 to j:
            lineCost = Infinity
            numTrailingSpaces = extras[i, j]
            
            if numTrailingSpaces > 0:
                if j == N:
                    lineCost = 0
                else:
                    lineCost = pow(numTrailingSpaces, 3)
            
            breakHereCost = totalCost[i - 1] + lineCost
            best = min(breakHereCost, best) 
       
        totalCost[j] = best 
    
    return totalCost[N]
```

+++

Runtime is $O(n^2)$ with $O(n^2)$ space.

## 5. Python: DP Pretty Printing

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/DP-pretty-printing.py)
