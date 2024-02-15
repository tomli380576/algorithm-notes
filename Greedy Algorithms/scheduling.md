# Scheduling Classes / Activity Selection (122A)

>  **Question.** Given a list of class schedules consisting of start times $S$ and end times $F$, what’s the maximum amount of classes we can take?

<!-- - $S:\texttt{\blue{Map}\gray{<\blue{string}, \blue{Time}>}}$,  -->

`s: Map<string, Time>`    
:   a map of start times of classes, where the key is the class name and the value is the start time.

`f: Map<string, Time>`
:   a map of finish times of classes, where the key is the class name and the value is the finish time.

Constraint: All the times `s[i]` and `f[i]` are both less than some maximum time $T$.

We want to find `List<string>`, the longest list of classes we can take without schedule conflicts.

## 1. No Recursive Backtracking Here

Well no but actually yes because this problem can be solved recursively.

![](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99ea6da8-3fc7-40a2-a30c-613ff9282305/Untitled.png)

We either take a class or skip. Since we want the maximum amount of classes, this problem falls under the [best solution]() case.

Let $curr$ be the current class we are considering and $prev$ be the previous class we chose to take. The recurrence is:

$$
\text{Schedule}(curr, prev) = \begin{cases}
0 & \text{if no more classes}\\
\text{skip}(curr) & \text{if not compatible}
\\
\max\left.\begin{cases}
\text{take}(curr)\\
\text{skip}(curr)
\end{cases}\right\} & \text{otherwise}

\end{cases}
$$

Convert to expressions:

$$
\text{Schedule}(curr, prev) = \begin{cases}
0 & \text{if $S$ is empty}\\
\text{Schedule}(curr.\text{next}, prev) & \text{if $s[curr]> f[prev]$}
\\
\max\left.\begin{cases}
\text{Schedule}(curr.\text{next}, curr) + 1\\
\text{Schedule}(curr.\text{next}, prev)
\end{cases}\right\} & \text{otherwise}

\end{cases}
$$

where $curr.\text{next}$ depends on the implementation to decide what the “next” class is.

This is the recurrence for computing the **number of classes**. Replace the $+1$ with the literal class name and replace 0 with empty array if we are finding the actual schedule.

### 1.1 Python. Scheduling with Backtracking

This directly implements the recurrence above.

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/backtracking-class-scheduling.py)

### 1.2 Python. Scheduling with DP

Memoizes the recurrence.

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/DP-class-scheduling.py)

---

## 2. Take the Local Best

Backtracking takes $O(2^n)$ with $O(1)$ space, DP takes $O(n^2)$ with $O(n^2)$ space.

But we can do better. Since we want to take as many classes as possible, we want the current choice to be **finish as early as possible** to leave room for other classes. This is the greedy choice.

Then our local best is the class that finishes first, and is compatible with the previous choice.

```c
function GreedySchedule(s[1...n], f[1...n]) -> List<string>:
	schedule = []

	sort f and permute s to match
	put f[1] in schedule

	for each class in f[2...n]:
		if class is compatible:
			add class to schedule

	return schedule
```

If we use merge/quick sort, this algorithm is $O(n\log n  + n) = O(n\log n)$.

Note that this greedy strategy won’t work if the classes are weighted.

### 2.1 Python. Greedy Scheduling

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/greedy-class-scheduling.py)

### 2.2 Proof. Greedy Exchange Argument

[Reference](http://www.cs.cornell.edu/courses/cs482/2007su/exchange.pdf)

Suppose there’s an optimal solution to the problem $\text{OPT}=\{o_1 ,o_2, \dots, o_k\}$. Then the maximum number of classes we can take is $k$. Now given a greedy solution $A= \{a_1, a_2, \dots\}$, we can replace the first different choice of $\text{OPT}$ with a greedy choice and still remain optimal.

We are trying to show the new set $A'$ is optimal:

$$
A'=\text{OPT} - \underbrace{\{o_i\}}_\text{first different choice} + \underbrace{\{a_i\}}_\text{greedy choice}
$$

- $|A'| = |\text{OPT}|$, so $A'$ still has the maximum size
- Every class in $A'$ is compatible with each other

Now we show that the greedy choice is compatible. Assume without loss of generality that the first difference is the first choice ($i=1$).

- By greedy choice, $f[a_1]\leqslant f[o_1]$ (Greedy stays ahead)
- And we know $f[o_1]\leqslant s[o_2]$ (from given, optimal solutions are always compatible)
- Then $f[a_1]\leqslant s[o_2]$ by transitivity of $\leqslant$.

Therefore $a_1$ is compatible with $o_2$, which means we can **exchange $a_1$ with $o_1$ without making the solution worse**. So $\text{OPT}-\{o_1\}+\{a_1\}$ is optimal. $A$ is optimal by induction because we can swap everything inside $\text{OPT}$ with $A$.
