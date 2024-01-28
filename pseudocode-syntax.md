---
icon: note
---

# Pseudocode Syntax Reference

## Basics

Math operators are used the same as they are in formal mathematical writing.

`->` **indicates a return type**. Only used for function declarations.
- If nothing is returned, this symbol is unused.

Any kind of indexing, range, interval is **inclusive** unless explicitly stated.
- Example: `A[1...N]: List<number>` means the list `A` has `N` elements and can be indexed by `1, 2, 3, ... N`.

Angle brackets `<T>` indicate the use of type parameters. For example, `List<Vertex>` means elements of this list are vertices. 

### Examples

```c
function MergeSort(A[1...n]):
	if n > 1:
		mid = floor(n / 2)
		MergeSort(A[1 ... mid])
		MergeSort(A[mid + 1 ... n])
		Merge(A, mid)
```

In this pseudocode:

- Array `A` can be indexed by integers in $[1, n]$
- `mid` was declared and assigned the value `floor(n / 2)`
- `floor`, `Merge` are function calls
- Returns nothing

```c
const prices = [...]

// Backtracking
function CutRod(rod_len) -> number:
	if rod_len == 0:
		return 0
	if rod_len == 1:
		return prices[1]
	
	best = 0
	for cut_len = 1 to rod_len:
		cut_profit = CutRod(rod_len - cut_len) + prices[cut_len]
		if cut_profit > best:
			best = cut_profit

	return best
```

Here we have an global constant `prices = [...]`. The function returns a number. `int` or `float` are explicitly stated when it is important (e.g. if the function returns an index, it must be an `int`).

## Data Structure Initialization

Any object will be created on the heap through its constructor and are passed around by reference. (anything other than numbers, booleans, and strings)

```c
queue = Queue()
```

### Multi-dimensional Arrays

Let’s use this syntax to state each dimension's size of the array:

```c
Array(shape=(dimension1, dimension2, dimension3, ...))
```

For example:

```c
B = Array(shape=(5, 6))
```

This means $B$ can be indexed by:

- 1 through 5 on the 1st axis
- 1 through 6 on the 2nd axis.