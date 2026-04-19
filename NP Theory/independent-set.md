# Independent Set

For an undirected graph $G = (V,E)$, an independent set $S \sub V$ satisfies $\forall u,v \in S, uv\notin E$

## Independent set is NP complete

For an input $G$, solution $S$, we need to show that S can be verified quickly, but cannot be found in poly time.

Checking if $S$ is a solution is poly time. For each pair $x, y\in S$, check if $xy \in E$. This is worst case $O(E^2)$

Now we will reduce 3SAT to Independent Set.

