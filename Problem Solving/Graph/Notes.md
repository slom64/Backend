Questions
- Check if  a network is connected.
	- Undirected `BFS`.
	- directed `DFS`.
- Compute the connected/disconnected graph pieces
	- Undirected `BFS`.
	- Directed `DFS`
- Check order of doing dependent tasks
	- `DFS` DAG
- Efficient rounting
	- `BFS`: shortest path based on edges only.
	- `Dijkstra`
- Efficient network design. "min totoal cost"
	- `MST` "Kruskal"
- Maximize the flow in a network, max # finished tasks by a given set of persons.



| Algo                    | Input                                                                                     | Output                                                   |
| ----------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| `DAG relaxation` `DFS`  | 1. if we have already `wDAG`<br> (Weighted Directed Acyclic Graph)<br> 2. Starting point. | Shortest path from a single source to all other vertices |
| `Minimal Spaining Tree` | Given `Connected`, `Undirected` Graph                                                     | Minimal cost to connect the whole network                |


| Algo             | Graph   | Weight       | Order             |
| ---------------- | ------- | ------------ | ----------------- |
| `BFS`            | General | Unweighted   | `O(V + E)`        |
| `DAG Relaxation` | DAG     | Any          | `O(V + E)`        |
| `Dijkstra`       | General | Non-Negative | `O(V Log(V) + E)` |
| `Bellman-Ford`   | General | Any          | `O(V * E)`        |
| `Kruskal MST`    | General | Any          | `O(E log E)`      |
