## **1. Flow Networks**

* **Definition:** A directed graph $G = (V, E)$ with:

  * A **source** $s$ and a **sink** $t$.
  * Each edge $(u, v) \in E$ has a **positive integer capacity** $c(u, v) > 0$.
  * No edge enters $s$, and no edge leaves $t$.

* **Flow:** A function $f: E \to [0, \infty)$ satisfying:

  * **Capacity constraint:** $f(u, v) \leq c(u, v)$.
  * **Flow conservation:** $\sum f(\text{in}) = \sum f(\text{out})$ for $v \ne s, t$.

* **Value of flow:** $|f| = \sum f(s, v) = \sum f(v, t)$.

* **Integrality Theorem:** If capacities are integers, there exists a max flow where all $f(u, v)$ are integers.

---

## **2. Solving the Maximum Flow Problem**

### **2.1 Ford-Fulkerson Algorithm (FFA)**

* **Idea:** Repeatedly augment flow along **augmenting paths** in the **residual network** until none exist.
* **Residual Network:**

  * For each edge $(u, v)$ with flow $f$, add:

    * $(u, v)$ with capacity $c(u, v) - f(u, v)$
    * $(v, u)$ with capacity $f(u, v)$
* **Augmenting Path:** A path from $s$ to $t$ in the residual graph.
* **Update Flow:** Send flow equal to the **minimum residual capacity** (bottleneck) along the path.

### **2.2 Cuts in a Flow Network**

* **Cut (S, T):**

  * $S \cup T = V, S \cap T = \emptyset, s \in S, t \in T$
* **Capacity of Cut:** $c(S, T) = \sum c(u, v)$ where $u \in S, v \in T$
* **Flow through Cut:** $f(S, T) = \sum f(u, v) - \sum f(v, u)$
* **Property:** $|f| = f(S, T) \leq c(S, T)$

### **2.3 Max-Flow Min-Cut Theorem**

* **Statement:** Maximum flow = Minimum cut capacity.
* **Proof:** After FFA terminates, construct $S$ as the set of nodes reachable from $s$ in residual graph; edges crossing to $T$ are saturated.

### **2.4 Time Complexity**

* **Ford-Fulkerson:** $O(E \cdot |f|)$

  * Can be exponential if capacities are large.
* **Edmonds-Karp (BFS version):** $O(V \cdot E^2)$

  * Always picks shortest path (in terms of edges).

### **2.5 Alternatives**

* **Edmonds-Karp:** BFS for shortest augmenting paths.
* **Dinic’s Algorithm:** $O(V^2 \cdot E)$
* **Preflow-Push:** $O(V^3)$

---

## **3. Applications of Flow Networks**

### **3.1 General Modelling Tricks**

* **Multiple sources/sinks:** Add **super-source** and **super-sink**.
* **Vertex capacities:** Split each vertex $v$ into $v_{in}$ and $v_{out}$, connect with capacity $C(v)$.

---

### **3.2 Movie Rental Problem**

* Each customer $u_i$, each movie $v_j$
* Build network:

  * $s \to u_i$ with cap 5
  * $u_i \to v_j$ if customer wants movie $j$, cap 1
  * $v_j \to t$ with cap $m_j$
* **Time complexity:** $O(n^2k)$

---

### **3.3 Cargo Allocation**

* Grid with cell capacities, row/column limits
* Build network:

  * $s \to r_i$ with cap $Cr(i)$
  * $r_i \to c_j$ with cap $C(i, j)$
  * $c_j \to t$ with cap $Cc(j)$
* **Time complexity:** $O((n + m)(nm)^2)$

---

### **3.4 Vertex-Disjoint Paths**

* Red (source), Blue (sink), Black (intermediate)
* Use:

  * Super-source and sink
  * Vertex-splitting for blacks (in/out pair)
* All edges capacity 1
* **Time complexity:** $O(n(n + m))$

---

### **3.5 Maximum Bipartite Matching**

* Bipartite graph $(A, B)$
* Flow network:

  * $s \to a_i$, $b_j \to t$
  * Directed edge $a_i \to b_j$ for original edge
* All capacities = 1
* Max flow = max matching

---

### **3.6 Job Centre**

* Workers with qualifications, jobs with requirements
* Edge $a_i \to b_j$ if worker qualifies for job
* Construct bipartite graph and use max matching
* **Time complexity:** $O(nm(k + \min(n, m)))$

---

## **4. Puzzle**

* Drop 4 indistinguishable pills (2 A, 2 B).
* Split each pill in half. Eat **half of each pill** each day.
* This guarantees one A and one B per day over 2 days.
