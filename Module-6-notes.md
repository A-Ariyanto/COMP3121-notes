**Module 6: Intractable Problems – Notes**

---

## 1. Polynomial-Time Computability

* **Polynomial-time algorithm**: An algorithm running in $O(n^k)$ steps for some constant $k$, where $n$ is the length of the input encoding .
* **Input length**:

  * For an integer $x$, $|x|\approx\lceil\log_2 x\rceil$ bits .
  * For an array of $n$ integers each $\le M$, total length is $n\log_2 M$.

---

## 2. Classes P and NP

* **P**: Decision problems solvable in polynomial time.
* **NP**: Decision problems where “YES” instances admit a polynomial-size *certificate* checkable in polynomial time by a *certifier* .
* **Example**:

  * **Primality** (“Is $x$ prime?”) ∈ NP (certificate: a nontrivial divisor) and ∈ P (AKS algorithm, deterministic poly-time) .
  * **SAT** is in NP (check an assignment in $O(n)$ time) but unknown if in P .

---

## 3. Polynomial Reductions & NP-Completeness

* **Polynomial reduction**: $U\le_p V$ if $\exists$ polynomial-time computable $f$ mapping instances of $U$ to $V$ preserving YES/NO answers .
* **Implication**: If $U\le_p V$ and $V\in P$, then $U\in P$.
* **NP-hard**: A problem $V$ to which every problem in NP reduces; may not itself be in NP.
* **NP-complete**: Problems in NP that are NP-hard.
* **Cook’s Theorem**: SAT is NP-complete .
* **Example reductions**:

  * SAT $\rightarrow$ 3SAT by clause‐chaining (introduce new variables to enforce 3‐literals per clause) .
  * 3SAT $\rightarrow$ Vertex Cover and Clique (construct gadgets for variables and clauses) .

---

## 4. Intractable Optimisation Problems

* **Optimisation vs. decision**: The optimisation version (e.g., TSP minimisation) is at least as hard as its decision counterpart because one can compare the optimum value to a threshold in constant extra time .
* **Integer Linear Programming (ILP)** is NP-hard despite LP being in P .
* **Examples**:

  * Travelling Salesperson Problem (TSP): no known poly-time solution for general case .
  * Subset Sum: pseudopolynomial DP runs in $O(nS)$, not polynomial in input length $\,n\log S$ .

---

## 5. Superpolynomial Algorithms

* **Meet-in-the-middle (Subset Sum)**: Runs in $O(2^{n/2}n)$ time and $O(2^{n/2})$ space, improving over $O(2^n n)$ brute force by effectively “square‐rooting” the exponent .
* **TSP via DP (Held–Karp)**: Runs in $O(2^n n^2)$ time, using a table of size $2^n \times n$, far faster than $O(n!n)$ brute force for moderate $n$ .

---

## 6. Approximation Algorithms

* **Vertex Cover**: A simple 2-approximation by repeatedly selecting both endpoints of any remaining edge .
* **Metric TSP**:

  1. Compute MST of graph (weight $\le$ optimal tour).
  2. Traverse and shortcut repeated visits (triangle inequality ensures no increase).
  3. Yields a tour of length $\le 2\,\text{OPT}$ .
* **Inapproximability of general TSP**: Unless P = NP, no constant-factor approximation exists; reduction from Hamiltonian Cycle via edge weights $1$ vs. $kn$ distinguishes YES/NO by length threshold .

---

## 7. Coping Strategies for NP-Hard Problems

1. **Approximation** (constant/Polynomial-time Approximation Schemes).
2. **Exact superpolynomial** (e.g., $O(1.1970^n)$ for Vertex Cover).
3. **Fixed-parameter tractability** (e.g., $O(f(k)\,n)$ algorithms).
4. **Heuristics** (e.g., COVer Edges Randomly).
5. **Restricted instances** (trees, bipartite graphs, interval graphs).
6. **Quantum speed-ups** (mild improvements, not poly-time unless P = NP) .

---

*End of Notes*
