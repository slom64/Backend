- generate sorted array while pushing to it.
- It is based on heap
- if you want to constrauct it from array you can do it in O(N)
- single push O(log n).
- top O(1). it gets the item with higher priority. **read only** it doesn't remove it or fetch it. so it doesn't call `poll()`.
	- Thats why sorting using Priority Queue takes O(n log n), to see the next MAX number we should call `poll()` which takes O(log n)

| **Operation**                    | **Complexity**  | **Note**                                     |
| -------------------------------- | --------------- | -------------------------------------------- |
| **New PQ from Collection**       | **$O(N)$**      | Uses the optimized `heapify` process.        |
| **$N$ individual `add()` calls** | $O(N \log N)$   | If you build the tree manually using `add()` |
| **Single `poll()` / `add()`**    | **$O(\log N)$** | Standard heap maintenance.                   |
| **`peek()`**                     | **$O(1)$**      | Just looks at the root of the heap.          |

### In general

| Operation   | Complexity  |
| ----------- | ----------- |
| add         | $O(\log N)$ |
| Extract Min | $O(\log N)$ |
| Update      | $O(\log N)$ |


### Why use this over TreeSet?
While both have O(logN) for insertion, the **constant factors** are very different.
- **PriorityQueue (The Array King):** It is a "Complete Binary Tree" stored inside a **flat array**.
    - **Cache Locality:** Because it's an array, the CPU can cache it very efficiently.
    - **Memory Overhead:** It only stores the data itself. There are no "Node" objects or pointers to "Left/Right/Parent" children.
- **TreeSet (The Pointer Web):** It is a **Red-Black Tree**.
    - **Memory Bloat:** Every single item you add is wrapped in a `TreeMap.Entry` object. Each entry has four pointers (Left, Right, Parent, Key) and a boolean for color. On a 64-bit Linux system, this consumes significantly more RAM.
    - **Cache Misses:** To find an element, the CPU has to "follow" pointers to random memory addresses. This is much slower than jumping through an array.
- **Construction Speed (O(N) vs O(NlogN))**
	- `new PriorityQueue(list)` takes **O(N)**.
	- `new TreeSet(list)` takes **O(NlogN)**. If you are initializing a large data structure, the `PriorityQueue` will be significantly faster.

| **Use Case**             | **Use PriorityQueue** | **Use TreeSet**            |
| ------------------------ | --------------------- | -------------------------- |
| **Top K Elements**       | ✅ (The best choice)   | ❌ (Too much overhead)      |
| **Dijkstra's Algorithm** | ✅ (Standard choice)   | ❌ (Slow and no duplicates) |
| **Keeping data unique**  | ❌ (Allows duplicates) | ✅ (Guarantees uniqueness)  |
| **Search/Contains**      | ❌ ($O(N)$ - Slow)     | ✅ ($O(\log N)$ - Fast)     |
| **Range Queries**        | ❌ (Impossible)        | ✅ (e.g., `subSet(10, 50)`) |
