Alright — here’s your **COMP3121 Algorithm Cheat Sheet**
(I’ve kept each entry short enough for fast revision, but still with the core idea you’ll need in the exam).

---

## **Module 1 – Fundamentals**

| Algorithm           | Problem              | How it Works                                                       | Time Complexity                                   |
| ------------------- | -------------------- | ------------------------------------------------------------------ | ------------------------------------------------- |
| **Mergesort**       | Sort array           | Divide into halves, recursively sort, merge sorted halves          | $O(n \log n)$                                     |
| **Binary Heap Ops** | Priority queue       | Tree stored in array, maintain heap property via sift-up/down      | Insert: $O(\log n)$, Extract-min/max: $O(\log n)$ |
| **BST Ops**         | Search/insert/delete | Maintain binary search order; balance for efficiency               | Avg: $O(\log n)$, worst: $O(n)$                   |
| **Hash Table**      | Fast lookup          | Compute hash, resolve collisions (chaining or probing)             | Avg: $O(1)$                                       |
| **Gale–Shapley**    | Stable matching      | Repeated proposals from one side, accept tentatively, reject worse | $O(n^2)$                                          |

---

## **Module 2 – Graph Algorithms**

| Algorithm            | Problem                             | How it Works                                       | Time Complexity    |
| -------------------- | ----------------------------------- | -------------------------------------------------- | ------------------ |
| **BFS**              | Shortest path in unweighted graph   | Explore level by level using queue                 | $O(V+E)$           |
| **DFS**              | Graph traversal, cycle detection    | Recursively explore unvisited neighbours           | $O(V+E)$           |
| **Topological Sort** | Order DAG nodes                     | DFS post-order reverse or Kahn’s algorithm         | $O(V+E)$           |
| **Dijkstra**         | SSSP (positive weights)             | Greedy: pick smallest distance node, relax edges   | $O((V+E)\log V)$   |
| **Bellman–Ford**     | SSSP (neg. weights, no neg. cycles) | Relax all edges $V-1$ times                        | $O(VE)$            |
| **Floyd–Warshall**   | APSP (neg. allowed)                 | DP on intermediates $k$: opt(i,j,k)                | $O(V^3)$           |
| **Union–Find**       | Disjoint sets                       | Parent pointers + path compression + union by rank | \~$O(1)$ amortized |

---

## **Module 3 – Greedy Algorithms**

| Algorithm                | Problem                          | How it Works                                          | Time Complexity |
| ------------------------ | -------------------------------- | ----------------------------------------------------- | --------------- |
| **Fractional Knapsack**  | Maximise value with weight limit | Sort by value/weight, take fractions                  | $O(n \log n)$   |
| **Activity Selection**   | Max activities without overlap   | Sort by finish time, pick earliest finishing          | $O(n \log n)$   |
| **Huffman Coding**       | Optimal prefix code              | Combine 2 least freq symbols repeatedly (PQ)          | $O(n \log n)$   |
| **Kruskal’s MST**        | Min spanning tree                | Sort edges, union–find to avoid cycles                | $O(E \log E)$   |
| **Prim’s MST**           | Min spanning tree                | Grow tree from start node, add min edge to new vertex | $O(E \log V)$   |
| **Shortest Path in DAG** | SSSP in DAG                      | Topo sort, relax edges in order                       | $O(V+E)$        |

---

## **Module 4 – Network Flow**

| Algorithm              | Problem               | How it Works                                         | Time Complexity               |
| ---------------------- | --------------------- | ---------------------------------------------------- | ----------------------------- |
| **Ford–Fulkerson**     | Max flow              | Augment along any path until none remain             | $O(E \cdot \text{max flow})$  |
| **Edmonds–Karp**       | Max flow (polynomial) | Ford–Fulkerson with BFS for shortest augmenting path | $O(VE^2)$                     |
| **Bipartite Matching** | Max match             | Build flow network, run max flow                     | Depends on max flow algorithm |

---

## **Module 5 – Dynamic Programming**

| Algorithm                        | Problem                        | How it Works                                              | Time Complexity           |
| -------------------------------- | ------------------------------ | --------------------------------------------------------- | ------------------------- |
| **Rod Cutting**                  | Max profit from cuts           | DP: best length = max over first cut + remainder          | $O(n^2)$                  |
| **Matrix Chain Mult.**           | Min multiplication cost        | DP over chain length, split at k                          | $O(n^3)$                  |
| **LCS**                          | Longest subsequence            | DP table: if match, +1 from diag; else max(top,left)      | $O(mn)$                   |
| **LIS**                          | Longest increasing subsequence | DP: LIS\[i] = 1 + max(LIS\[j] for j\<i, arr\[j]\<arr\[i]) | $O(n^2)$ or $O(n \log n)$ |
| **0–1 Knapsack (DP)**            | Max value within weight        | DP table over items and weight capacity                   | $O(nW)$                   |
| **Weighted Interval Scheduling** | Max value non-overlap          | Sort by finish time, DP with p(i) for last non-overlap    | $O(n \log n)$             |

---

## **Module 6 – Intractable Problems**

| Algorithm                   | Problem           | How it Works                           | Time Complexity                   |
| --------------------------- | ----------------- | -------------------------------------- | --------------------------------- |
| **Reductions**              | Show NP-hardness  | Transform known hard problem to target | –                                 |
| **2-Approx Vertex Cover**   | Approximation     | Pick any edge, add both endpoints      | $O(E)$                            |
| **Exact VC O(1.1970ⁿ)**     | Small graph exact | Branch on including vertex or not      | Super-polynomial                  |
| **LP (Simplex, Ellipsoid)** | Optimisation      | Solve constraints in polytope          | Simplex: usually fast, worst exp. |

---

If you want, I can make this into a **double-sided A4 “exam cram sheet”** with **one-line pseudocode for each algorithm** so you can instantly recall the mechanics during the final.
Do you want me to prepare that next?
