# COMP3121 Module 2 — Divide & Conquer: Exam‑Ready Notes

> Term 2, 2025 • Based on “Module 2: Divide and Conquer” (Paul Hunter)

---

## 0) What “Divide & Conquer” (D\&C) really means

* **Divide:** Split instance of size *n* into *a* subproblems of size *n/b*.
* **Conquer:** Solve subproblems (recursively).
* **Combine:** Merge sub‑solutions in cost *f(n)*.
* **Generic recurrence:** `T(n) = a·T(n/b) + f(n)`.

**Correctness schema (strong induction):**

1. Base cases correct. 2) Assume all sizes < *n* correct. 3) Show combine step yields correct solution for size *n*.

---

## 1) Warm‑ups & classic examples

### 1.1 Coin puzzle (27 coins, 1 light counterfeit, 3 weighings)

* Split into 3 piles of 9; weigh A vs B.

  * Heavier/lighter side contains fake; if equal, it’s in C → recurse with 9, then 3, then 1 ⇒ `3` weighings: `log₃ 27`.

### 1.2 Binary Search (and variants)

* **Steps:** Probe midpoint (Θ(1)) → recurse on one half → return. Depth `⌊log₂ n⌋`. Cost per level Θ(1) ⇒ `Θ(log n)`.
* **Lower/Upper bound:** first `i` with `A[i] ≥ x` / last `i` with `A[i] ≤ x`.
* **Equal range:** first & last occurrence; typical interview staple.

### 1.3 Merge Sort

* Split into halves, sort recursively, **merge in Θ(n)**.
* Depth `log₂ n`, work/level Θ(n) ⇒ `Θ(n log n)`.

### 1.4 Quicksort

* Partition Θ(n), recurse on two sides. **Average `Θ(n log n)`**, **worst `Θ(n²)`** (e.g., bad pivots). Randomized/median‑of‑3 mitigates.

### 1.5 Discrete Binary Search (DBS) on answers

* Turn an optimisation into a **monotone decision**: `feasible(t)?`
* **Max Median** problem: given sorted `A` of size `2n−1` and `k` increments, largest achievable median.

  * Decision for a target `t`: sum `Σ_{i=n}^{2n-1} max(0, t − A[i]) ≤ k?` in `O(n)`.
  * **Search space:** `[A[n], A[n]+k]` (integers). **Binary search** this with `O(log k)` decisions ⇒ `O(n log k)`.

---

## 2) Recurrences & the Master Theorem

### 2.1 Recurrence template

`T(n) = a·T(n/b) + f(n)` with `a ≥ 1`, `b > 1`, non‑decreasing `f`.

* **Leaves:** `n^{log_b a}` many; **critical exponent** `c* = log_b a` and **critical polynomial** `n^{c*}`.

### 2.2 The three cases (remember the intuition)

1. **Subcritical combine** (`f(n) = O(n^{c*−ε})`): leaves dominate ⇒ `T(n) = Θ(n^{c*})`.
2. **Balanced** (`f(n) = Θ(n^{c*})`): equal per level ⇒ `T(n) = Θ(n^{c*} log n)`.
3. **Supercritical combine** (`f(n) = Ω(n^{c*+ε})` **and** regularity `a f(n/b) ≤ k f(n)` for some `k < 1`): root dominates ⇒ `T(n) = Θ(f(n))`.

### 2.3 Worked examples

* `T(n) = 4T(n/2) + n` → `c* = 2`, `f = n = O(n^{2−0.1})` ⇒ **Case 1:** `Θ(n²)`.
* `T(n) = 2T(n/2) + 5n` → `c* = 1`, `f = Θ(n)` ⇒ **Case 2:** `Θ(n log n)`.
* `T(n) = 3T(n/4) + n` → `c* = log₄3 ≈ 0.7925`, `f = n = Ω(n^{c*+0.1})` and regularity holds ⇒ **Case 3:** `Θ(n)`.
* **Limitations:** e.g., `2T(n/2) + n·log₂ log₂ n` → between cases; classic Master Theorem doesn’t apply.

**Exam template for MT:**

1. Identify `a,b,f`. 2) Compute `c* = log_b a` and `n^{c*}`. 3) Compare `f` to `n^{c*}` (O/Θ/Ω). 4) Check regularity if Case 3. 5) Conclude `Θ(·)`.

---

## 3) Fast Integer Multiplication

### 3.1 Schoolbook & plain D\&C

* **Grade‑school:** `O(n²)` bit ops for two `n`‑bit numbers.
* **Naïve D\&C:** split `A = A₁·2^{n/2} + A₀`, `B = B₁·2^{n/2} + B₀`:

  * Compute `A₀B₀, A₀B₁, A₁B₀, A₁B₁` (4 submultiplies) + linear shifts/adds ⇒ `T(n)=4T(n/2)+Θ(n)` ⇒ `Θ(n²)`.

### 3.2 Karatsuba’s trick

* Use identity: `(A₁+A₀)(B₁+B₀) − A₁B₁ − A₀B₀ = A₁B₀ + A₀B₁`.
* Only **3** submultiplies: `X=A₀B₀`, `W=A₁B₁`, `V=(A₁+A₀)(B₁+B₀)`; combine as `W·2^{n} + (V−W−X)·2^{n/2} + X`.
* Recurrence `T(n)=3T(n/2)+Θ(n)` ⇒ `Θ(n^{log₂3}) ≈ Θ(n^{1.585})`.
* **Extensions:** Toom–Cook & friends push toward `n^{1+ε}`; state‑of‑the‑art reaches `O(n log n)` (enormous constants; only for astronomically large *n*).

---

## 4) Convolution, Polynomials & the FFT

### 4.1 Convolution = coefficient view of multiplication

* For `PA(x)=Σ A_i x^i`, `PB(x)=Σ B_j x^j`, product `PC(x)` has coefficients `C_t = Σ_{i+j=t} A_i B_j` = **convolution** `C = A ⋆ B`.
* Direct coefficient method is **Θ(n²)**.

### 4.2 Value representation & plan

* Evaluate both polynomials at **m points**, multiply pointwise, then **interpolate**.
* Choose points as **m‑th complex roots of unity** `ω_m^k = e^{2πik/m}`; DFT maps coefficients → values; IDFT inverts.
* With **divide & conquer FFT**, `DFT/IDFT` each in `Θ(m log m)`.

### 4.3 FFT recursion idea (Cooley–Tukey)

* Split `PA(x)` into **even** and **odd** parts: `PA(x)=P_even(x²)+x·P_odd(x²)`.
* Evaluate at `x=ω_m^k`; then `x²=ω_{m/2}^k` by the **cancellation lemma**.
* Recurse on size `m/2` for both even/odd coefficient lists; combine in linear time ⇒ `T(m)=2T(m/2)+Θ(m)` ⇒ `Θ(m log m)`.
* Zero‑pad to next power of 2 so recursion is clean.

### 4.4 Application: “Fishing Net” problem

* Shore fish counts `A[1..100L]`, binary net `N[1..L]` with holes; best placement to maximize catch.
* Reverse net: `N'` and compute convolution `A ⋆ N'` (with zero‑padding). The **t‑th** convolution term equals total fish caught ending at position `t`.
* Use FFT to compute convolution in `Θ(L log L)` (constants suppressed by problem statement).

---

## 5) Putting it together — proof & analysis moves

* For D\&C correctness: cite **induction on size** and name the invariant of the **combine** step (e.g., “merge preserves sortedness”, “cross inversions counted when taking from right run”).
* For complexity: (i) Write the recurrence, (ii) apply Master Theorem (with regularity check), (iii) justify any padding/assumptions.
* For DBS: argue **monotonicity** (`feasible(t)` implies `feasible(t−1)` / or dual), define integer search range, give decision `O(g(n))` and final `O(g(n) log k)`.

---

## 6) Micro‑drills (self‑check)

1. Prove correctness of mergesort via strong induction (3–5 lines).
2. Show how to count inversions in `Θ(n log n)` by modifying merge (explain when you add `|left_remaining|`).
3. Apply Master Theorem to `T(n)=8T(n/4)+n²` and `T(n)=3T(n/3)+n log n`.
4. Write the Karatsuba combine formula from `X, W, V` without re‑deriving.
5. State the FFT even/odd decomposition and the cancellation lemma.
6. Max Median: write the exact decision sum and the DBS range.