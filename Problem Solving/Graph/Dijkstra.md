- SSSP, single source shortest
- Input
	- **Weighted** `Non-Negative`
	- A source vertex
- Output
	- shortest path from source to all other vertices.
	- Find minimum weight.


---
### How it works
- At each step, Dijkstra’s algorithm selects a node that has not been processed yet and whose distance is as small as possible.
- When a node is selected, the algorithm goes through all edges that start at the node and reduces the distances using them.
- The next node would be the smallest edge that is connected to one of the visited nodes.