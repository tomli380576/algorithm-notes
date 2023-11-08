# Merge Sort (122A)

$A:\texttt{\blue{number}\gray{[]}}$
:	the array to sort

The idea of merge sort is:

```c
function MergeSort(A[1 … n]):
	sorted_left = sort left half     // *Recursion Magic*
	sorted_right = sort right half    // *Recursion Magic*
	Merge sorted_left and sorted_right
```

## Dividing the Problem

We need to split the array in half. Let `low` and `high` denote the current starting and ending indices of the array (inclusive). Then we can find the middle index `mid` by:

```c
mid = floor((low + high) / 2)
```

!!!warning
Note that this could cause integer overflow when `low` and `high` are large. A better way would be:

```c
mid = floor(low + ((high - low) / 2))
```

!!!

And the recursive calls are:

```c
MergeSort(A[1 … mid])
MergeSort(A[mid + 1 … high])
```

## Base Cases

This is where we actually do the comparison. We have 2 base cases:

1. The array is empty or has 1 element, then we do nothing.
2. The array has exactly 2 elements, then we directly compare.
   - This case could be implicitly handled by merging two 1–element arrays, so no need to explicitly implement this case.

## Merge 2 sorted Arrays

Now that the recursion has sorted the left and right array for us, we need to merge them together. We can use the **2 pointer** approach here.

Let `ptr_1` and `ptr_2` be the traversing pointers in the left & right sorted arrays respectively.

It’s probably easier to consider merging with 2 separate arrays first.

```c
function Merge2SortedArrays(L[1 … n], R[1 … m]) -> Array<int>:
	ptr1 = 0  // traverses L array
	ptr2 = 0  // traverses R array
	output = []

	while neither pointer has reached the end:
		if L[ptr_1] should come before R[ptr_2]: // call the predicate here
			push L[ptr1] into output
			ptr1 ++
		else:
			push R[ptr2] into output
			ptr2 ++

	nonEmptyArray = L if ptr_1 not at the end, R if ptr_2 not at the end
	push all remaining elements from nonEmptyArray into output

	return output
```

Translate into expressions: (Assuming we are sorting in ascending order. Flip < to > if reversed.)

```c
function Merge2SortedArrays(L[1 … n], R[1 … m]) -> Array<int>:
	ptr1 = 0
	ptr2 = 0
	output = []

	while ptr1 != n and ptr2 != m:
		if L[ptr1] < R[ptr2]:
			push L[ptr1] into output
			ptr_1 ++
		else:
			push R[ptr2] into output
			ptr_2 ++

	while ptr1 != n:
		push L[ptr1] into output
		ptr_1 ++

	while ptr2 != m:
		push R[ptr2] into output
		ptr_2 ++

	return output
```

The 2nd and 3rd `while` loops are safe because at most 1 of them will run, and the loop that runs corresponds to the array that has all its remaining elements $\geqslant$ the last element in `output`. 

We can also see that this is a linear scan, so the runtime is $O(n)$.

#### Implementation Details

In actual implementation, don’t actually slice the main array into $L$ and $R$ because passing arrays on the stack is expensive.

Instead, pass in an extra parameter `mid` to indicate where the left half ends, then merge with the same 2 pointer approach.

At the end, instead of returning output, replace all elements in the parameter array with `output`.

This requires the main array to be passed as a reference so it may be different depending on the language.

## Python: Merge Sort

```c
//pseudocode
function MergeSort(A[1 … n]):
	if n > 1:
		mid = floor(n / 2)
		MergeSort(A[1 … mid])
		MergeSort(A[mid + 1 … n])
		Merge(A, mid)
```

[!badge variant="dark" target="blank" text="Github"](https://github.com/tomli380576/ECS122A-Algorithms-python-implementation/blob/main/Implementations/merge-sort.py) This implementation is not sorting directly inside the array. So it’s more expensive memory wise.

