- While BFS is the king of **shortest paths**, DFS (Depth-First Search) is the king of **structure and connectivity**.
- **Backward edge** its the reason of **cycles**, which and vertics has a neighbor that is "gray" not fully discovered. 
- if one of node neighbors is **black**:
	- **And** the discover time of node is smaller than discover time of black neighbor. this is **forward edge**.
![[Z Assets/Images/Pasted image 20260204182709.jpeg]]

- And the discover time of node is **bigger** than discover time of black neighbor. this is **corss edge**. which may happen between 2 distinct trees, or between 2 leaf trees from the same tree.
![[Z Assets/Images/Pasted image 20260204183219.jpeg]]

### What does it gives us?
- Discovery and Finish Times (The Timeline)
- **Back Edges (Cycle Detection)**
	- This is useful before trying to do topological sorting, because if there is cycle we won't be able to do topological sort.
- Topological Sorting


---

## Topological sort
- Input: It has a limitation that the input graph should be `DAG` directed acyclic graph
- Output: It give us a tree that all of its vertices are in one direction, which is the order of tasks without violation dependencies.
- Run `DFS` and based on finish time, last one finish it is the first one we should start with.


---

## DAG relaxation
- Shortest path from a single source to all other vertices
- Input: Weighted Directed Acyclic Graph (`wDAG`)
### Steps
1. Do normal DFS, and get the `topologicacl sort`.
2. Start from the required point
3. For every neighbor of each vertex update the cost   `Min(cost[neighbor], cost[vertex] + edge_weight) `

![[Z Assets/Images/Pasted image 20260204213302.jpeg]]


> [!tips]
> You can put negative wieghts to get the opposite of min, to get MAX
