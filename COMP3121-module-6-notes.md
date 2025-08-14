# COMP3121 Module 6 Notes — Intractable Problems: Exam-Ready Notes

## **Module 6 – Intractable Problems**

### 1. **What is an Intractable Problem?**

* A problem is **intractable** if no polynomial-time algorithm is known for it.
* These problems typically require **super-polynomial** time in the worst case (e.g., $O(2^n)$, $O(n!)$).
* The focus is on **decision problems** (yes/no answers) because they are easier to classify.

---

### 2. **Decision, Search, and Optimization Problems**

* **Decision problem**: Answer is YES/NO.
* **Search problem**: Find an example (e.g., a solution).
* **Optimization problem**: Find the *best* example according to some metric.
* **Why decision problems?**
  If a decision version is intractable, so is the corresponding search/optimization version.

---

### 3. **Complexity Classes**

1. **P** – problems solvable in polynomial time.
2. **NP** – problems whose solutions can be verified in polynomial time (nondeterministic polynomial time).
3. **NP-Hard** – at least as hard as any NP problem; not necessarily in NP.
4. **NP-Complete** – in NP and NP-Hard.

**Relationship:**

$$
P \subseteq NP \subseteq \text{NP-Hard}
$$

If $P = NP$, then **all NP problems are solvable in polynomial time** (unknown).

---

### 4. **Polynomial-Time Reduction**

* A **reduction** from problem $A$ to problem $B$ means:

  * Any instance of $A$ can be *converted* to an instance of $B$ in polynomial time.
  * If $B$ can be solved in polynomial time, so can $A$.
* **Notation**: $A \le_p B$ (A reduces to B in polynomial time).
* **Why reductions?**
  They let us prove that if one problem is hard, another is also hard.

---

### 5. **Reduction Example – Hitting Set → Set Cover**

*(Expanded step-by-step explanation)*

#### **Definitions**

* **Hitting Set Problem**:

  * Input: Collection $C$ of subsets of a set $U$, integer $k$.
  * Question: Is there a subset $H \subseteq U$ with $|H| \le k$ that intersects every set in $C$?
    (“Hits” every set at least once.)
* **Set Cover Problem**:

  * Input: Collection $S$ of subsets of a set $X$, integer $k$.
  * Question: Is there a subcollection of $S$ with $|S'| \le k$ whose union is $X$?
    (Covers all elements.)

#### **Reduction Intuition**

We want to **turn** an instance of *Hitting Set* into an instance of *Set Cover* so that solving Set Cover solves Hitting Set.

#### **Step-by-Step Reduction**

1. **Given**:

   * Universe $U$ (elements)
   * Collection $C = \{C_1, C_2, ..., C_m\}$
   * Target size $k$.

2. **Create Set Cover Instance**:

   * New universe $X$ will represent the **sets** in $C$.
     → Let $X = \{x_1, x_2, ..., x_m\}$ where each $x_i$ corresponds to $C_i$ in hitting set.
   * For each element $u \in U$:

     * Create a subset $S_u \subseteq X$ consisting of all $x_i$ such that $u \in C_i$.
     * This means: “if we pick $u$, we cover all the sets in $C$ that contain $u$”.

3. **Interpretation**:

   * Picking element $u$ in hitting set = choosing subset $S_u$ in set cover.
   * If the chosen $S_u$ cover all of $X$, then the chosen $u$ hit all sets in $C$.

4. **Preserve Size Constraint**:

   * We keep the same $k$ for set cover.
   * If we can find ≤ $k$ subsets $S_u$ covering all of $X$, the corresponding $u$’s form a hitting set of size ≤ $k$.

5. **Correctness Proof**:

   * **If** there’s a hitting set $H$ of size ≤ $k$:

     * The corresponding $S_u$ cover every $x_i$ because $H$ hits every set $C_i$.
   * **If** there’s a set cover of size ≤ $k$:

     * The corresponding $u$ form a hitting set since each $C_i$ (represented by $x_i$) is covered.

6. **Complexity**:

   * Transforming the instance is polynomial:
     For each $u \in U$, check membership in each $C_i$.

---

### 6. **Why This Matters**

* If we know **Set Cover is NP-Complete** and we reduce Hitting Set → Set Cover in polynomial time, then **Hitting Set is also NP-Complete** (since we can verify solutions in polynomial time).
* This method works for proving hardness of many problems.

---

### 7. **Typical NP-Complete Problems in Module 6**

* SAT (Boolean satisfiability)
* 3-SAT
* Clique
* Vertex Cover
* Hamiltonian Cycle
* Travelling Salesman (decision version)
* Knapsack (decision version)

---

### 8. **Proving NP-Completeness (Cook–Karp–Levin style)**

1. Show the problem is in NP (solutions verifiable in polytime).
2. Choose a known NP-Complete problem $A$.
3. Reduce $A$ to your problem $B$ in polynomial time.
4. Conclude: $B$ is NP-Complete.

---

