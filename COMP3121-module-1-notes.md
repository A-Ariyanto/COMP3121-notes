# COMP3121 Module 1 — Foundations: Exam‑Ready Notes

> Term 2, 2025 • Based on “Module 1: Foundations” slides (Paul Hunter)

---

## Quick Overview

* **Focus:** Worst‑case time complexity, core DS\&A refresh (heaps, BSTs, hashing), proof tools (logic, induction, contradiction), and **Gale–Shapley** for Stable Matching. Ends with a classic **gas‑station** puzzle.
* **What to master:** Asymptotics (O, Ω, Θ), when to ignore constants/lower‑order terms, how to justify correctness & bounds, and how the stable matching proof works (termination, perfectness, stability).

---

## 1) Time Complexity & Asymptotics

### 1.1 Big Ideas

* **Worst vs Best vs Average case**

  * Course prioritises **worst‑case** to be robust to adversarial/unlucky inputs.
  * Average/expected analysis (e.g., hashing, quicksort) uses probability and is largely assumed from prior courses.
* **Asymptotic viewpoint:** Compare **growth rates** as *n → ∞*; constants and small values of *n* don’t change the classification.

### 1.2 Definitions (informal but exam‑ready)

* **Big‑Oh:** `f(n) = O(g(n))` ⇢ for large enough *n*, `f(n)` ≤ `c·g(n)` (upper bound). “No worse than.”
* **Big‑Omega:** `f(n) = Ω(g(n))` ⇢ for large enough *n*, `f(n)` ≥ `c·g(n)` (lower bound). “No faster than.”
* **Big‑Theta:** `f(n) = Θ(g(n))` ⇢ both `O` and `Ω` hold. “Same order.”

> **Note:** Writing “Bubble sort runs in O(n²)” is correct but weaker than “Θ(n²)”. The latter conveys the **tight** bound.

### 1.3 Examples (from slides)

* `100n = O(n)`
* `2n + 7 = O(n)` and also `O(n²)` (but the tight one is `Θ(n)`).
* `0.001n³ ≠ O(n²)` (eventually exceeds any constant·n²).

### 1.4 Laws you’ll repeatedly use

* **Sum rule:** If `f1 = O(g1)` and `f2 = O(g2)`, then `f1 + f2 = O(g1 + g2) = O(max{g1, g2})`.

  * *Interpretation:* Sequential stages → dominated by the slower one.
* **Product rule:** If `f1 = O(g1)` and `f2 = O(g2)`, then `f1·f2 = O(g1·g2)`.

  * *Interpretation:* Nested loops/parts → multiply the bounds.

### 1.5 Practical cautions

* Asymptotics ignore constants, but **real input limits** and **hidden constants** matter in practice.
* “Large enough n” means **eventual** behaviour; don’t over‑interpret behaviour at n ≤ 10⁶ if problems never reach that size.

---

## 2) DS\&A Refresher (what you need at your fingertips)

### 2.1 Heaps (binary)

* **Structure:** Complete binary tree; **max‑heap** has parent ≥ children (swap ≤ for min‑heap).
* **Use:** Priority queue implementation.
* **Ops:** Build `O(n)`, Find‑max `O(1)`, Delete‑max `O(log n)`, Insert `O(log n)`.

### 2.2 Binary Search Trees (BSTs)

* **Invariant:** Node key > all in left subtree and < all in right subtree.
* **Let h be height:** Search/Insert/Delete `O(h)`.

  * Best case `h ≈ log n` (balanced), worst case `h ≈ n` (e.g., sorted insert order → a chain).

### 2.3 Self‑Balancing BSTs

* **Goal:** Maintain `h = O(log n)` by rotations & invariants (e.g., AVL, Red‑Black, B‑trees).
* **Result:** All ops become `O(log n)` guaranteed.

### 2.4 Frequency Tables

* **Pattern:** Map items to integers `1..m`, keep an array of counts.
* **Example problem:** Array of size `n−1` contains all numbers `1..n` except one missing.

  * **Linear‑time, constant‑space idea:** XOR trick (`1 ⊕ 2 ⊕ … ⊕ n ⊕ A[1] ⊕ … ⊕ A[n−1]` yields the missing), or arithmetic sum with overflow care.

### 2.5 Hash Tables

* **Concept:** Hash function maps keys → indices in a fixed‑size table; collisions are inevitable.
* **Separate chaining:** List at each bucket.
* **Complexity:** Expected `O(1)` for search/update/insert/delete; **worst‑case** `O(n)` if everything collides.

---

## 3) Proof Tools & Reasoning About Algorithms

### 3.1 Logic essentials

* Connectives: ¬ (not), ∧ (and), ∨ (or), → (implication). Quantifiers: ∀, ∃.

### 3.2 Induction (including strong induction)

* **Template:** Base case(s) + inductive step (assume up to `k`, prove `k+1`).
* **Metaphor:** Dominoes: knock first; each knocks the next.

### 3.3 Proof by Contradiction

* Assume ¬P, derive an impossibility (Q ∧ ¬Q), conclude P.
* **Tip:** Often pairs naturally with “minimal counterexample” arguments.

### 3.4 Example: Mergesort (correctness & `O(n log n)`)

* **Divide:** Recursively sort `A[ℓ..m]` and `A[m+1..r]`.
* **Conquer:** Merge two sorted halves in `O(n)`.
* **Depth:** `log₂ n` levels; **work/level:** `O(n)` ⇒ **total:** `O(n log n)`.
* **Correctness:** By induction on subarray size; merging preserves sorted order.

> **Exam cue:** Be ready to state the recurrence intuition without heavy notation: “`log` levels × linear merge per level ⇒ `n log n`.”

---

## 4) Stable Matching (Gale–Shapley)

### 4.1 Core definitions

* **Matching:** No person is in two pairs.
* **Perfect matching:** Everyone is paired.
* **Stable matching:** No unmatched pair `(f, b)` where *both* prefer each other over their assigned partners.

### 4.2 Gale–Shapley (frontend proposes; backend responds)

**Algorithm (high‑level):**

1. All frontend engineers start **solo**.
2. While a solo frontend exists who hasn’t proposed to everyone:

   * Propose to the most‑preferred backend not yet proposed to.
   * Backend accepts if free; else, keeps the better of current vs proposer (rejects the worse).

**Running time:** Each frontend proposes to each backend at most once ⇒ ≤ `n²` proposals/rounds.

### 4.3 Why it works (proof sketch triad)

* **Termination:** At most `n²` proposals.
* **Perfectness:** If algorithm stopped with some solo frontend, every backend must have been proposed to; but then every backend is paired and so must every frontend ⇒ contradiction.
* **Stability:** Backends only “trade up”; frontends propose in decreasing preference order. If a blocking pair existed `(F, B)` where both prefer each other, then `F` would have proposed to `B` before settling, and `B` would have either accepted and never traded down, or rejected only for someone preferred to `F`. Either way, they can’t end up in a blocking situation ⇒ contradiction.

> **Takeaway facts:** Existence is guaranteed; proposer side obtains a **best feasible** stable outcome for themselves (and dually, worst for the other side).

---

## 5) Puzzle: Gas‑Station (Circular Tour) — Existence Proof

**Claim:** On a circle with `n` stations of uneven fuel, where total fuel equals the fuel needed to complete one loop, there exists a station from which a full loop is possible (taking all fuel at each stop).

**Proof sketch (prefix‑sum minimum):**

* Let `Δi = fuel_i − cost_i_to_next`. Sum `Δi` over the circle is `0` (given).
* Consider cumulative sums `S(k) = Δ1 + … + Δk`. Let `t` be an index where `S(t)` is **minimal**.
* Start at station `t+1`: Every subsequent partial sum `S(k) − S(t) ≥ 0`, so you never dip below zero fuel along the way, completing the loop.

**Algorithmic viewpoint:** If starting at `i` fails somewhere before `j`, then **no** station between `i` and `j` can succeed; jump start to `j+1`. One pass yields a valid start.

---

## 6) Exam Pointers & Pitfalls

* State **model & assumptions** (worst‑case? input size? data structure invariant?).
* For complexity, explicitly identify **dominant stage** (sum rule) or **nesting** (product rule).
* For proofs, name the **technique** (induction/contradiction) and the key invariant/ordering.
* Stable matching: remember the **≤ n²** bound and the **no‑blocking‑pair** contradiction.
* Hashing: distinguish **expected O(1)** vs **worst‑case O(n)**; mention collision handling.
* Heaps: recall **Build‑Heap O(n)** (not `n log n`).

---

## 7) Micro‑Drills (self‑check)

1. Give a tight asymptotic bound for `3n² + 100n log n + 7` and justify via sum rule.
2. Why is Build‑Heap `O(n)`? Outline the level‑wise work argument.
3. Provide a strong‑induction proof template you could adapt in an exam.
4. Describe Gale–Shapley in 3 bullets, and prove termination in one sentence.
5. Prove the gas‑station claim using the minimal‑prefix argument in ≤ 5 lines.
