# Minimal Spaning Tree
- Given `Connected`, `Undirected` Graph
- Output:
	- froms tree "No cycles"
	- includes every vertex V
	- has minimum possible cost.
	- Number of edges = `|V| - 1`, less than this will be not connected, more than this will make cycles.
- Algorithms
	- Kruskal algo
	- Prim's algo


---
## Kruskal
- Greedy algo based on choosing the minmum edge everytime
- $\begin{matrix} O(E\; log(E)) \end{matrix}$

### Steps
1. Sort edges based on cost from minimum to maximum
2. Everytime we choose the minimum, we check the source,distination nodes if they are in the **same set** we **reject** the edge, but if they are in **different** sets we **accept** them.
3. If we accept the edge we do union between sets.