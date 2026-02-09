### What data does BFS give us?
When you run BFS starting at node A, you generate three critical pieces of information for every other reachable node V:
1. **Distance:** The minimum number of edges to get from A to V.
2. **Predecessor (Parent):** The node that "discovered" V. This allows you to reconstruct the actual path.
3. **Connectivity:** Whether V is even reachable from A.

### How to store this additional data
Instead of just printing the nodes, you store these results in simple arrays (since nodes are usually 0 to N−1).
- `int[] dist`: `dist[v]` stores the shortest distance from the start.
- `int[] parent`: `parent[v]` stores which node came before v.