# COMP3121 Module 4 — Flow Networks: Exam‑Ready Notes

> Term 2, 2025 • Based on “Module 4: Flow Networks” (Paul Hunter)

---

## 0) Core objects & properties

* **Flow network:** directed graph `G=(V,E)` with **capacities** `c(u,v) > 0` on edges, **source** `s`, **sink** `t`. No edge enters `s`, none leaves `t`.
* **Flow `f`:** nonnegative on edges, obeys (i) **capacity** `0 ≤ f(u,v) ≤ c(u,v)` and (ii) **conservation** at all `v ≠ s,t`: inflow = outflow.
* **Value `|f|`:** total outflow of `s` (equivalently inflow to `t`).
* **Integrality theorem:** with **integer** capacities, there exists an **integral** maximum flow.

---

## 1) Residual networks & augmenting paths

* **Residual graph `G_f`:** for each original edge `(u,v)` with capacity `c` and current flow `f`, add:

  * **forward** edge `(u→v)` of capacity `c−f` (extra we can still push forward),
  * **backward** edge `(v→u)` of capacity `f` (how much of earlier flow we can cancel).
  * If both directions existed in the original, residual **adds** (forward spare + cancel room) accordingly.
* **Augmenting path:** any `s→t` path in `G_f`. Its **bottleneck** is the min residual capacity on the path; push that much along the path (cancelling where we traverse backward edges).
* **Key loop:** while an augmenting path exists, augment → recompute residual → repeat.

**Intuition:** Residual edges let us “undo” earlier bad choices by sending flow back and re‑routing.

---

## 2) Ford–Fulkerson (FF)

**Algorithm (template):**

1. Start with zero flow.
2. Build `G_f`. If there is an `s→t` path, push its bottleneck; update `G_f`.
3. Repeat until no augmenting path exists.

**Termination:** With integer capacities, each augmentation increases `|f|` by **≥ 1**, so process stops (total flow is finite).

**Correctness idea (via min‑cut):** When no augmenting path remains, let `S` be vertices **reachable** from `s` in `G_f` and `T=V\S`.

* Every edge **`S→T` is saturated** (else `T` vertex would be reachable), and every edge **`T→S` carries zero flow** (else its reverse residual edge would make `T` vertex reachable).
* Therefore the **flow across `(S,T)` equals its capacity**, i.e. `f(S,T)=c(S,T)`. Since any flow ≤ any cut capacity, this flow is **maximum** and `(S,T)` is a **minimum cut**.

**Complexity:** Each augmentation path found in `O(|E|)` (e.g., DFS/BFS). Total time `O(|E|·|f*|)`. This can be **pseudo‑polynomial** and **exponential in input bits** (capacities written in binary) in worst cases.

---

## 3) Cuts & Max‑Flow Min‑Cut Theorem

* **Cut `(S,T)`:** partition with `s∈S`, `t∈T`. **Capacity** `c(S,T) = Σ_{u∈S,v∈T} c(u,v)` (ignore edges from `T→S`).
* For any flow `f`, **flow across the cut** `f(S,T) = Σ_{S→T} f(u,v) − Σ_{T→S} f(u,v)`.
* **Facts:**

  * `f(S,T) = |f|` (by conservation telescoping over all internal vertices).
  * `|f| ≤ c(S,T)` for **every** cut.
* **Theorem (Max‑Flow = Min‑Cut):** maximum flow value equals the **minimum** cut capacity. FF’s terminal partition `(S,T)` achieves equality.

---

## 4) Picking augmenting paths: Edmonds–Karp (EK)

* **Rule:** always pick a **shortest‑edge‑count** augmenting path (BFS in `G_f`).
* **Guarantee:** Number of augmentations is `O(|V|·|E|)`; each BFS is `O(|E|)` ⇒ total `O(|V|·|E|^2)`.
* **Practical note:** FF’s `O(|E||f*|)` bound still holds; EK trades capacity‑dependence for a clean polynomial bound.
* **Beyond scope (faster in theory):** Dinic `O(|V|^2|E|)`; Push–Relabel `O(|V|^3)`; recent near‑linear research (FYI only).

---

## 5) Modeling tricks (extend the basic model)

### 5.1 Multiple sources/sinks

* Add a **super‑source** `s` with edges to all sources and a **super‑sink** `t` with edges from all sinks. Encode any source/sink limits as those edge capacities (else use a very large number).

### 5.2 Vertex capacities

* For vertex `v` with capacity `C(v)`, **split** into `v_in` and `v_out`, connect `v_in→v_out` with capacity `C(v)`. Point all original **incoming** edges to `v_in` and all **outgoing** from `v_out`.

---

## 6) Canonical applications (how to build the networks)

### 6.1 Movie rental (dispatch as many movies as possible)

* Nodes: `s`, customers `u_i`, movies `v_j`, `t`.
* Edges: `s→u_i` capacity **5**; `u_i→v_j` capacity **1** if customer `i` wants movie `j`; `v_j→t` capacity \*\*m\_j\` (copies).
* **Run:** Max‑flow (EK). **Time:** `O((n+k+2)(nk+n+k)^2)`; tighter using `O(|E|·|f*|) = O((nk+n+k)·n) = O(n^2k)` since at most `5n` total flow leaves `s`.

### 6.2 Cargo allocation on a grid (row/column limits)

* Nodes: `s`, rows `r_i`, columns `c_j`, `t`.
* Edges: `s→r_i` cap `C_r(i)`; `r_i→c_j` cap `C(i,j)` for usable cells; `c_j→t` cap `C_c(j)`.
* **Run:** EK. **Time:** `O((n+m+2)(nm+n+m)^2) = O((n+m)(nm)^2)`.

### 6.3 Vertex‑disjoint red→blue paths (each black used ≤1)

* **Build:** super‑`s` to all red; all blue to `t`; **split** each black into `v_in→v_out` with cap 1; direct edges accordingly; **all edges cap 1**.
* **Run:** EK. **Time:** `O(|V|·|E|^2) = O(n(n+m)^2)`. Using `O(|E|·|f*|)`, `|f*| ≤ min(r,b)` ⇒ `O((n+m)·min(r,b)) = O(n(n+m))`.

### 6.4 Maximum bipartite matching

* Given bipartite `A ∪ B`, orient edges `A→B`, add `s→A` (cap 1) and `B→t` (cap 1). Max‑flow value = matching size.
* Residual graph edges flip along chosen matches to expose **augmenting paths**; terminate when none remain.

### 6.5 Job centre (qualifications → jobs)

* Build bipartite graph between workers and jobs (edge iff worker has **all** required qualifications). Reduce to **max bipartite matching** via flow.
* **Time:** build edges in `O(nmk)`; then flow in `O(|E|·|f*|) ≤ O((nm+n+m)·min(n,m))` ⇒ overall `O(nm(k+min(n,m)))`.

---

## 7) Checklists & proof patterns

* **FF correctness:** define `S` as residual‑reachable; argue **S→T saturated**, **T→S empty** ⇒ `|f|=c(S,T)`.
* **Cut computations:** (i) compute `c(S,T)` using only `S→T`; (ii) compute `f(S,T)` as forward minus backward; (iii) verify `|f|=f(S,T)`.
* **Modeling:** be explicit about node types, edges, and capacities; argue each constraint via **capacity or conservation**.
* **Complexity:** always quote both `O(|V||E|^2)` (EK) and the tighter `O(|E||f*|)` when a small `|f*|` bound is available.

---

## 8) Micro‑drills (self‑test)

1. Given a flow, draw the residual graph and find one augmenting path and its bottleneck.
2. Prove `f(S,T)=|f|` using conservation (one paragraph).
3. Construct the movie‑rental network for `n=3, k=3` and compute the tightest time bound via `O(|E||f*|)`.
4. Convert a vertex‑capacity instance to edge‑only via node‑splitting.
5. For a bipartite graph, perform two residual augmentations by hand and show the matching size grows by 1 each time.
6. Provide a worst‑case example where FF with arbitrary choices takes `Ω(|f*|)` augmentations.

---

## 9) Puzzle (meds A & B) — quick solution

You spilled **two A** and **two B** pills but can’t tell which is which and must take one of each pill per day for two days.
**Trick:** Cut **all four** pills in **half**. Day 1, take **one half from each pill** (four halves total) → together this is exactly **1×A + 1×B**. Day 2, take the remaining four halves → again **1×A + 1×B**.
