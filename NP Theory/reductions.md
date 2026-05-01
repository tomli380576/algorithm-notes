# Reduction

Example: Coloring $\to$ SAT

Support there's a poly-time algorithm for SAT, we want to use it to construct a algo for coloring. Let $I$ be the input for coloring, we need

I -> convert to input of SAT, f(I) -> run SAT -> convert to output of coloring, h(S)

We want to define $f$ and $h$ and show they run in poly-time.

- $f$ takes the input of coloring, returns the input for SAT
- $h$ takes the output of SAT, returns the output of colorings

## NP-completeness proof

Example: independent sets (IS)

Need to show:

- $IS\in NP$
- For all problem $A\in NP$, we can convert A to IS

We know SAT is NP-complete, then we automatically have $A$ to SAT and we only need to show SAT $\to$ IS reduction.
