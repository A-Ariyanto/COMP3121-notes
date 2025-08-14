# COMP3121 Module 5 Notes — Dynamic Programming: Exam‑Ready Notes

## **Dynamic Programming (DP) Essentials**

### 1. **Core Idea**

DP solves problems by breaking them into **overlapping subproblems** with **optimal substructure**, storing results to avoid recomputation.

* **Optimal Substructure:** Solution to a problem can be built from optimal solutions to its subproblems.
* **Overlapping Subproblems:** The same subproblem occurs multiple times during recursion.

---

### 2. **General Steps to Design a DP Algorithm**

1. **Define subproblems** – minimal parameters needed to answer the full problem.
2. **Form a recurrence** – relate subproblem solutions.
3. **Set base cases** – handle trivial instances directly.
4. **Choose computation order** – ensure all dependencies are solved first.
5. **Store solutions** – usually in an array/table.
6. **Extract the final answer** – may be one subproblem or combination.
7. **Analyse time complexity** – `#subproblems × time/subproblem`.

---

### 3. **Implementation Styles**

* **Top-Down (Memoization):** Recursion + cache results.
* **Bottom-Up (Tabulation):** Iteratively fill table from base cases up.

---

### 4. **Common DP Patterns & Examples**

#### **A. One Parameter**

* **Constant Recurrence:**
  Example: *Tower of Hanoi* – `moves(n) = 2*moves(n-1) + 1`, base `moves(1) = 1`.
  Time: Θ(n) for computing sequentially.

* **Linear Recurrence:**
  Example: *Hopscotch* – `num(i) = num(i-1) + num(i-2)`.

---

#### **B. Two Parameters**

* **Constant Recurrence:**
  Example: *Longest Common Subsequence (LCS)* –

  ```
  if a[i] == b[j]:
      opt(i,j) = 1 + opt(i-1,j-1)
  else:
      opt(i,j) = max(opt(i-1,j), opt(i,j-1))
  ```

  Base: `opt(i,0) = opt(0,j) = 0`.

* **Linear Recurrence:**
  Example: *0–1 Knapsack* –

  ```
  opt(i,k) = max(
      v[k] + opt(i-w[k], k-1),
      opt(i, k-1)
  )
  ```

  Base: `opt(0,k) = opt(i,0) = 0`.

---

#### **C. DP on Graphs**

* **Shortest Path in DAG:** Topological sort, then
  `opt(t) = min_{(v,t) in E} opt(v) + w(v,t)`.
  Time: O(n+m).

* **Bellman-Ford:** For negative edges,
  `opt(i,t) = min_{(v,t) in E} opt(i-1,v) + w(v,t)`.
  Time: O(nm).

* **Floyd-Warshall:** All-pairs shortest paths,
  `opt(i,j,k) = min(opt(i,j,k-1), opt(i,k,k-1) + opt(k,j,k-1))`.
  Time: O(n³).

---

#### **D. String Matching / Sequence Alignment**

* **Edit Distance:**

  ```
  opt(i,j) = min(
      cD + opt(i-1,j),      # delete
      cI + opt(i,j-1),      # insert
      (opt(i-1,j-1) if A[i]==B[j] else cR + opt(i-1,j-1))
  )
  ```

* **LCS / SCS:** Variations on two-parameter table filling.

---

#### **E. Classic DP Problems from Module**

* **Making Change:**
  `opt(i) = 1 + min_{coin ≤ i} opt(i - coin)` (unlimited coins).
* **Integer Knapsack:** Similar to making change but maximising value.
* **Balanced Partition:** Reduction to 0–1 Knapsack with capacity T/2.
* **Matrix Chain Multiplication:**
  `opt(i,j) = min_{i<k<j} (s[i]*s[k]*s[j] + opt(i,k) + opt(k,j))`.
* **Assembly Line Scheduling:** Transition between lines with transfer times.

---

### 5. **Correctness Proof Approach**

* **Base cases correct** (trivial instances handled exactly).
* **Recurrence correctness:** Inductive step – optimal solution for a subproblem must come from optimal solutions to smaller subproblems (contradiction proof).
* **Final solution correctness:** Obtained by combining/choosing among subproblems as per problem definition.

---

### 6. **Time Complexity Tips**

* Count **number of subproblems**.
* Measure **time to solve each subproblem**.
* Multiply for total complexity.
* Many DP algorithms are **O(n²)** or **O(n³)** depending on parameter count, except when parameters take large numeric values → **pseudopolynomial** time.

---

