---
order: 95
---

# Levenshtein Edit Distance


[!badge size="l" variant="warning" text="Leetcode" icon="../assets/leetcode.svg"](https://leetcode.com/problems/edit-distance/description/)

> **Question.** Given 2 strings `A[1...n]` and `B[1...m]`, what is minimum number of edits to $A$  required to make 2 strings identical? An edit could be 1 of the following:
> - Insert a character
> - Delete a character
> - Replace a character

For example, when `A = "FOOD", B = "MONEY"`, the edit distance is 4.

```
FOOD -> MOOD -> MOND -> MONED -> MONEY
```

when `A = "HORSE", B = "ROS"`, the edit distance is 3.

```
HORSE -> RORSE -> ROSE -> ROS
```


## 1. Observing the Problem

From the example we can see that there are 3 operations at any given index `i`:

1. `Insert(i)` Cost = 1
2. `Delete(i)` Cost = 1
3. `Replace(i, newChar)` Cost = 1 if `A[i] != B[i]`, else Cost = 0

## 2. Solve the backtracking problem first

### 2.1 Decide the Function Signature

Since we eventually need to convert this to DP, it’s easier to use string indices to keep track of which subproblem we are solving. Let’s use $i$ for string `A[1...n]` and $j$ for string `B[1...n]`.

$$
\text{EditDist}(i, j)
$$

Our final result is:

$$
\text{EditDist}(n, m)
$$

### 2.2 Base Cases

If any of the string is empty, then the cost would be the length of that non-empty string. This will happen when we call:

$$
{\text{EditDist}}(\red0, j)\\
\text{ or }\\
\text{EditDist}(i, \red0)
$$

!!!info Implementation Detail

Here we used 0 to indicate that an index is going out of bounds. In actual code 0 is a valid index; use -1 with an if statement to catch it.

!!!

### 2.3 Recursive Cases

If both strings are not empty, we will try to **insert**, **delete**, or **replace**. These are the 3 possible choices. To make a choice:

1. Let’s consider what it really means to “insert”.
    
    `@EX` $A=\texttt{"FOOD"}, B=\texttt{"MONEY"}$
    
    $$
    \begin{matrix}
    &&&&&\Downarrow\\
    A:&\tt{F} & \tt{O}&\tt{O}&\tt{D}&\texttt{\red?}\\
    B:&\tt{M} & \tt{O}&\tt{N}&\tt{E} &\tt{\red Y} 
    \end{matrix}
    $$
    
    If we insert a $\tt Y$ at the questions mark, we have an aligned column. Then we can move to the "next" by considering the column to the left. 
    
    Maybe we can do this by `i -= 1, j -= 1`, now we are here:
    
    $$
    \begin{matrix}
    &&&&\Downarrow\\
    A:&\tt{F} & \tt{O}&\tt{O}&\tt{\red D}&\tt{\green Y}\\
    B:&\tt{M} & \tt{O}&\tt{N}&\tt{\red E} &\tt{\green Y} 
    \end{matrix}
    $$
    
    But notice that the question mark technically doesn’t exist because $A$ is shorter. 
    
    $$
    \begin{matrix}
    &&&&\overset{i}{\Downarrow}\\
    A:&\tt{F} & \tt{O}&\tt{O}&\tt{\red D}&\\
    B:&\tt{M} & \tt{O}&\tt{N}&\tt{E} &\tt{\red Y} \\
    &&&&&\underset{j}{\Uparrow}\\
    \end{matrix}
    $$
    
    So $i$ is already at the position after insertion. All we need to do is move $j$ to $j-1$.
    
    $$
    \begin{matrix}
    &&&&\overset{i}{\Downarrow}\\
    A:&\tt{F} & \tt{O}&\tt{O}&\tt{\red D}&\tt{\green Y}\\
    B:&\tt{M} & \tt{O}&\tt{N}&\tt{\red E} &\tt{\green Y} \\
    &&&&\underset{j}{\Uparrow}\\
    \end{matrix}
    $$
    
    Therefore the recursive call for insert is:
    
    $$
    \text{EditDist}(i, j-1) + 1
    $$
    
    +1 for the cost of insertion
    
2. Delete
    
    Delete is more straightforward. We simply don’t consider the $i$–th character in $A$. 
    
    We can do this by moving $i$ to $i - 1$.
    
    Therefore the recursive call for delete is:
    
    $$
    \text{EditDist}(i-1, j) + 1
    $$
    
    +1 for the cost of deletion
    
3. Replace
    
    To replace a character, we move both $i$ and $j$.
    
    `@EX` $A=\texttt{"FOOD"}, B=\texttt{"MONEY"}$
    
    $$
    \begin{array}{c:}
    \text{\blue {Index}}&\blue1 & \blue2 & \blue 3 & \blue4  & \blue 5 \\
    &&&&\Downarrow\\
    A:&\tt{F} & \tt{O}&\tt{O}&\tt{\red D}&\tt{Y}\\
    B:&\tt{M} & \tt{O}&\tt{N}&\tt{\red E} &\tt{Y} 
    \end{array}\hspace{1em}\xrightarrow{\text{replace(cost 1) at index 4}}\hspace{1em}\begin{matrix}
    \blue1 & \blue2 & \blue 3 & \blue4  & \blue 5 \\
    &&\Downarrow\\
    \tt{F} & \tt{O}&\tt{\red O}&\tt{\green E}&\tt{Y}\\
    \tt{M} & \tt{O}&\tt{\red N}&\tt{\green E} &\tt{Y} 
    \end{matrix}
    $$
    
    $$
    \begin{array}{c:}
    \text{\blue {Index}}&\blue1 & \blue2 & \blue 3 & \blue4  & \blue 5 \\
    &&\Downarrow\\
    A:&\tt{F} & \tt{\purple O}&\tt{N}&\tt{E}&\tt{Y}\\
    B:&\tt{M} & \tt{\purple O}&\tt{N}&\tt{E} &\tt{Y} 
    \end{array}\hspace{1em}\xrightarrow{\text{replace(free) at index 2}}\hspace{1em}\begin{matrix}
    \blue1 & \blue2 & \blue 3 & \blue4  & \blue 5 \\\Downarrow\\
    \tt{\red F} & \tt{\green O}&\tt{ N}&\tt{E}&\tt{Y}\\
    \tt{\red M} & \tt{\green O}&\tt{N}&\tt{E} &\tt{Y} 
    \end{matrix}
    $$
    
    To find the cost, we check if the characters $A[i]$ and $B[j]$ are the same.
    
    1. If $A[i] = B[j]$ then the replacement is free
    2. If $A[i] \ne B[j]$ then the cost is $1$
    
    Therefore the recursive call for replace is:
    
    $$
    \text{EditDist}(i-1, j-1) + \begin{cases}1 & \text{if }A[i]\ne B[j]\\
    0 & \text{if }A[i]= B[j]
    \end{cases}
    $$
    

Since we want the shortest distance, this falls under the [best solution](https://www.notion.so/Recursive-Backtracking-Summary-03fc781b681940b290ae3733330dcfde?pvs=21) case and we will need a `min(…)` call.

### 2.4 Forming the recurrence

Now we combine 2.2.2 and 2.2.3,

$$
{\text{EditDist}(i, j)} = \begin{cases}
i & \text{if } j = 0\\j & \text{if } i = 0\\
\min \left.\begin{cases}

\text{Insert}\\
\text{Delete}\\
\text{Replace}\\

\end{cases}\right\} & \text{Otherwise}
\end{cases}
$$

Convert to expressions:

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

### 2.5 Python: Backtracking Edit Distance

Implements the above recurrence. [!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/backtracking-edit-distance.py)

!!! **Sanity Check**

1. We still make only 1 choice at each recursion level, the choices are:
    1. Insert
    2. Delete
    3. Replace
2. All choices are valid when the 2 strings are not empty, so we try all 3 of them and see which one is the best.
3. Cost is always 1 for both insert and delete, 0 for replace if the characters are the same.

!!!

## 3. Convert Backtracking to Dynamic Programming

### 3.1 Data Structure

The `EditDistance(i, j)` call requires 2 arguments, so we can use a 2D array.

:::center
`EditDist: List<List<int>>`
:::

We can access the result of a previous function call by `EditDist[i, j]`

The final result will be in `EditDist[n, m]`

### 3.2 Order of Evaluation

$\text{EditDist}(i, j)$ needs:

- $\text{EditDist}(i, j - 1)$
- $\text{EditDist}(i-1, j)$
- $\text{EditDist}(i-1, j-1)$

So both the loop for $i$ and $j$ need to go in **ascending** order. 

!!!info
Similar to LCS, if we scan the 2 strings backwards, then the order of evaluation will also be flipped. In that case $i$ and $j$ goes in descending order. The final call will be in `EditDist[1, 1]`
!!!


## 4.2 Python: DP Edit Distance

We can also save the recursion results by using the library `@cache` decorator. See [LRU Cache](https://docs.python.org/3/library/functools.html).

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/DP-edit-distance.py)

This file has DP and `@cache` brute force.
