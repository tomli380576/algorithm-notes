# Quick Sort (122A)


## Problem Statement

`@Params` $A:\texttt{\gray{\blue{Array}<\blue{number}>}}$, the array to sort

The idea of quick sort is:

```c
function QuickSort(A[1 … n]):
	pivotValue = ChoosePivot()

	partitionLeft = from A select value where value < pivot value
	partitionMid = from A select value where value == pivot value
	partitionRight = from A select value where value > pivot value
	
	Recursively sort partitionLeft           // *Recursion Magic*
	Recursively sort partitionRight          // *Recursion Magic*
```

## Base Case

When $A$ has 1 or 0 elements, we do nothing.

## Partition

```c
partitionLeft = from A select value where value < pivot value
```

↑ This partitioning step is doing most of the work. 

Unlike merge sort, after left & right partitions are sorted, the merging step is already done.

Similar to how we considered `merge()`, it’s easier to think about if we use extra space.

```c
function Partition(A[1 … n], pivotValue) -> pair of Arrays:
	left = []
	right = []

	for i = 1 to n:
		if A[i]	< pivotValue:
			push A[i] into left
		else if A[i] == pivotValue: 
			if not first time seeing pivotValue:
				push A[i] into left
		else:
			push A[i] into right

	return left, right
```

(Change $ < $ to other predicates depending on the sorting order)

This is also a linear scan, so the runtime is $O(n)$.


## Choosing the Pivot

There are a lot of ways for choosing pivots:

1. Always pick first
2. Always pick last
3. Choose randomly
4. Somehow pick out the true median of $A$ (important for quick select)

[Source](https://www.geeksforgeeks.org/quick-sort/)

## Pseudocode

```c
function QuickSort(A[1 … n]) -> Array:
	if n > 1: 
		pivot_value = ChoosePivot()
		left, right = Partition(A[1 … n], pivot_value)
		return flatten([QuickSort(left), [pivot_value], QuickSort(right)])
	else:
		return A

function Partition(A[1 … n], pivot_value) -> pair of Arrays:
	left = []
	right = []

	for i = 1 to n:
		if A[i]	< pivot_value:
			push A[i] into left
		else if A[i] == pivot: 
			if not first time seeing pivot:
				push A[i] into left
		else:
			push A[i] into right

	return left, right

function ChoosePivot() -> int:
	// depends on implementation, assume this is O(1)
	return pivot
```

## Python: Quick Sort

[!badge variant="dark" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/quick-sort.py)