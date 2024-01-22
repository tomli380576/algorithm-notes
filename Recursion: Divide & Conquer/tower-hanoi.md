# Tower of Hanoi (36B)

## Problem Statement

> **Question.** Given 3 pegs and $n$ disks, how can we move all the disks from $\tt{src}$ to $\tt{dest}$ such that larger disks cannot stack on top of smaller disks?

`n: int`
:	number of disks. This implies we have disk from radius 1 to $n$

`src, dest, temp: Array<int>`
:	3 pegs that we can puts disks on.
  `src` initially has the disks $[1\dots n]$ on it.
  `dest, temp` will be empty initially.

## Reducing the problem

![Source: Erickson Text](/assets/hanoi-idea.png){class="image-m"}

We can observe that:

- The largest disk has to go on the $\tt{dest}$ peg first before everything else.

To do so, we need to move everything else to the $\tt{temp}$ peg first. Now $\tt{src}$ only has the largest disk and $\tt{dest}$ is empty ⇒ so we can directly move.

Let’s consider a simple case with only 2 disks.

![](/assets/simple-hanoi.png)

We do this 4-step process for any number of disks, except that 2 is the $n$-th disk and disk $1\sim n$ is in the position of 1.

## Pseudocode

```c
function SolveHanoi(n, src, dest, temp):
	// do nothing if there's no disk
	if n > 0:
		// move disks 1…n-1 to temp
		SolveHanoi(n - 1, src, temp, dest)
		put disk n on dest
		// move disks 1…n-1 from temp to dest
		SolveHanoi(n - 1, temp, dest, src)
```

### :icon-code: Python: Tower of Hanoi

[!badge variant="dark" size='l' icon="mark-github" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/tower_of_hanoi.py)