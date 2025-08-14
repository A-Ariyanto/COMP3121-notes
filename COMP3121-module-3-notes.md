# COMP3121 Module 3 — Greedy Algorithms: Exam‑Ready Notes

> Term 2, 2025 • Based on “Module 3: Greedy Algorithms” (Paul Hunter)

---

## 0) Greedy at a glance

* **Greedy idea:** Build the answer in stages by taking the locally best (myopic) choice.
* **When it works:** Problem admits a structure that lets local choices be globally optimal.
* **How to prove correctness:**

  1. **Greedy stays ahead** (prefix‑optimality / induction style): After every step, greedy is at least as good as any other partial solution.
  2. **Exchange argument** (local swaps / contradiction style): Any optimal solution can be transformed to the greedy one without hurting the objective.
* **Caveat:** Greedy ≠ universal. E.g., 0–1 knapsack is **not** solved by ratio greediness, but **fractional** knapsack is.

---

## 1) Optimal Selection

### 1.1 Activity Selection (max # of non‑overlapping intervals)

**Greedy rule:** Sort by **earliest finishing time**; scan once, picking the next activity whose start ≥ last chosen finish.

* **Correctness (exchange):** Replace the first non‑greedy pick with the greedy finishing‑earliest interval; no conflicts introduced, count preserved → keep swapping until solutions coincide.
* **Running time:** Sort `O(n log n)` + scan `O(n)` ⇒ `O(n log n)` (sorting dominates).
* **Same‑length intervals variant:** Maximising total duration ≡ maximising count ⇒ same algorithm.
* **Counterexample awareness:** Greedy by **shortest duration** or **fewest conflicts** fails.

### 1.2 Cell‑Tower Placement on a Line (range = 5 km)

**Greedy rule:** Sweep west→east. Place a tower **5 km east** of the first uncovered house; skip all covered houses; repeat.

* **Correctness (stays‑ahead):** The `k`‑th greedy tower is always at least as far east as the `k`‑th tower of any solution ⇒ no one can cover with fewer towers.
* **Running time:** `O(n)` if houses are given in order; otherwise sort + sweep ⇒ `O(n log n)`.

---

## 2) Optimal Ordering

### 2.1 Tape Storage (equal request probabilities)

**Goal:** Minimise average retrieval time ≍ minimise
`n·L1 + (n−1)·L2 + ··· + 2·L_{n−1} + L_n`.
**Greedy rule:** Sort files **increasing by length**.

* **Proof (adjacent exchange):** If `Li > L_{i+1}`, swapping reduces objective by `Li − L_{i+1} > 0`.
* **Time:** `O(n log n)` for sorting.

### 2.2 Tape Storage II (probabilities `p_i` known)

**Objective:** `E = Σ_{i} p_i · (sum of lengths up to i)`.
**Greedy rule:** Sort by **decreasing `p_i / L_i`**.

* **Proof (adjacent exchange):** If `p_k/L_k < p_{k+1}/L_{k+1}`, swapping `k, k+1` reduces `E` by `L_k p_{k+1} − L_{k+1} p_k > 0`.
* **Time:** `O(n log n)`.

### 2.3 Minimising Maximum Lateness (all jobs must be done)

**Greedy rule (EDD):** Order jobs by **Earliest Due Date** (ascending deadlines `due_i`). Durations do **not** affect the optimal order for max‑lateness.

* **Proof (adjacent inversion swap):** Swapping a pair scheduled out of due‑order cannot increase the max lateness of the pair; repeatedly fix inversions.
* **Time:** `O(n log n)`.

---

## 3) Optimal Merging — Huffman Coding

* **Prefix code:** No codeword is a prefix of another ⇒ unique decoding; codewords correspond to leaves of a binary tree.
* **Huffman algorithm:** Repeatedly combine the two **least‑frequent** symbols into a new node with weight `f_x + f_y`; use a min‑priority queue.
* **Yields:** An **optimal** prefix code (minimum expected codeword length `Σ f_i · depth_i`).
* **Time:** `O(n log n)` with a binary heap.
* **Why optimal (sketch):** (i) Some optimal tree is full; (ii) Two least‑frequent symbols are siblings at max depth; (iii) Induction on `n` by contracting that pair.

---

## 4) Greedy on Graphs

### 4.1 Strongly Connected Components (SCCs) & Condensation

* **SCC:** Maximal set where every vertex can reach every other.
* **Algorithms:** Tarjan’s (single DFS with stack) or Kosaraju’s (two DFS) ⇒ both `Θ(|V|+|E|)`.
* **Condensation graph:** Contract each SCC to a node ⇒ a **DAG**.
* **Application (Tsunami sensors):** Place a sensor on every **source SCC** (in‑degree 0 in the condensation DAG). Those must be triggered, and they reach all others along DAG edges.

### 4.2 Topological Sort (for DAGs)

* **Kahn’s algorithm:** Repeatedly remove a zero‑in‑degree vertex, recording the order; decrement in‑degrees of its out‑neighbours. `Θ(|V|+|E|)`.
* **Uses:** Natural processing order for many DP/greedy routines on DAGs.

### 4.3 Single‑Source Shortest Paths (non‑negative weights) — Dijkstra

* **Invariant:** Maintain tentative distances `d[v]`; at each step add the outside vertex with **smallest `d`** to the settled set `S`, relax its outgoing edges.
* **Correctness (stays‑ahead):** The chosen `v` already has the true shortest distance.
* **Data structures & time:**

  * **Array scan:** `O(n^2 + m)` ⇒ `O(n^2)` for simple graphs; fine for dense graphs.
  * **Augmented min‑heap with position map:** `O((n + m) log n)`; often simplified to `O(m log n)` since `m ≥ n−1` in connected digraphs.
  * (Fibonacci heap `O(m + n log n)` is out of scope unless fully developed.)

### 4.4 Minimum Spanning Trees (MSTs)

* **Cut property (distinct weights):** For any nonempty proper `S ⊂ V`, the lightest edge crossing `(S, V\S)` is in **every** MST.
* **Kruskal’s algorithm:** Sort edges by weight; add an edge iff it does **not** create a cycle; stop after `n−1` edges. Correct by the cut property.
* **Time:** `O(m log m)` for sorting (+ near‑linear cycle checks with DSU/Union‑Find if implemented).
* **(Prim’s algorithm exists; akin to Dijkstra but growing a tree.)**

---

## 5) Patterns to cite in proofs

* **Exchange:** Identify a first inversion vs greedy order; show swapping reduces (or doesn’t worsen) the objective; iterate.
* **Stays‑ahead:** Define a per‑prefix metric (e.g., k‑th finish time / k‑th tower position / settled distances) and prove greedy’s prefix is never worse.

---

## 6) Micro‑drills (self‑check)

1. Give a counterexample where “shortest interval first” fails for activity selection.
2. Prove the cell‑tower stays‑ahead claim rigorously for the inductive step.
3. Derive the objective for equal‑probability tape storage and justify the shortest‑first order.
4. Show that ordering by decreasing `p_i/L_i` minimises expected tape time via an adjacent swap.
5. For the tsunami problem, sketch Tarjan’s SCC routine and argue why the condensation graph is a DAG.
6. Prove Dijkstra’s correctness using the “first edge leaving S” argument.
7. State the MST cut property and use it to justify one Kruskal inclusion step.
