
# Dynamic Programming (DP) Notes

## Core Idea
Solve complex problems by breaking them into **overlapping subproblems**, storing solutions to avoid redundant calculations. Combines recursion with memoization/tabulation.

---

## Key Properties
1. **Optimal Substructure**  
   Optimal solution to a problem contains optimal solutions to its subproblems.
2. **Overlapping Subproblems**  
   Subproblems recur multiple times; solutions can be cached and reused.

## DP vs. Other Paradigms
| **Greedy**              | **Divide & Conquer**       | **Dynamic Programming**       |
|-------------------------|----------------------------|-------------------------------|
| Local optimal choices   | Independent subproblems    | Overlapping subproblems       |
| May not be optimal      | No reuse of subproblems    | Reuses subproblem solutions   |

## Steps to Design DP Algorithms
1. **Define Subproblems**  
   Smaller instances of the main problem.
2. **Formulate Recurrence**  
   Relate subproblem solutions (e.g., `opt(i) = f(opt(j))` for `j < i`).
3. **Identify Base Cases**  
   Smallest subproblems with trivial solutions (e.g., `opt(0) = 0`).
4. **Compute in Order**  
   Solve subproblems bottom-up or top-down with memoization.
5. **Construct Overall Solution**  
   Combine subproblem results (e.g., `opt(n)` or `max(opt(i))`).

**Time Complexity**:  
$$(\text{\# subproblems}) \times (\text{time per subproblem})$$

---

## Core Examples

### 1. Tower of Hanoi
**Problem**: Move `n` disks from peg 1 to peg 3 (min moves).  
**Subproblems**: `moves(i)` = min moves for `i` disks.  
**Recurrence**:  
$$\text{moves}(i) = 2 \cdot \text{moves}(i-1) + 1$$  
**Base Case**: `moves(1) = 1`.  
**Solution**: Compute iteratively for `i = 1 to n` → $O(n)$ time.  
**Closed Form**: $\text{moves}(n) = 2^n - 1$.

---

### 2. Hopscotch Paths
**Problem**: Count ways to reach square `n` (steps: ±1, ±2).  
**Subproblems**: `num(i)` = ways to reach square `i`.  
**Recurrence**:  
$$\text{num}(i) = \text{num}(i-1) + \text{num}(i-2)$$  
**Base Cases**: `num(0) = 1`, `num(1) = 1`.  
**Solution**: Fibonacci sequence → $O(n)$ time.

---

### 3. Longest Increasing Subsequence (LIS)
**Problem**: Find longest strictly increasing subsequence.  
**Subproblems**: `opt(i)` = LIS ending at `A[i]`.  
**Recurrence**:  
$$\text{opt}(i) = 1 + \max_{\substack{j < i \\ A[j] < A[i]}} \text{opt}(j)$$  
**Base Case**: `opt(i) = 1` for all `i`.  
**Solution**: Compute for `i = 1 to n` → $O(n^2)$ time.  
**Optimization**: $O(n \log n)$ with binary search.

---

### 4. 0-1 Knapsack
**Problem**: Maximize value with weight ≤ `C` (each item once).  
**Subproblems**: `opt(i, k)` = max value using first `k` items and capacity `i`.  
**Recurrence**:  
$$\text{opt}(i, k) = \max \begin{cases} 
\text{opt}(i, k-1) \\ 
v_k + \text{opt}(i - w_k, k-1) 
\end{cases}$$  
**Base Cases**: `opt(i, 0) = 0`, `opt(0, k) = 0`.  
**Order**: For `k = 1 to n`, then `i = 1 to C` → $O(nC)$ time.

---

### 5. Longest Common Subsequence (LCS)
**Problem**: Find longest subsequence common to two strings.  
**Subproblems**: `opt(i, j)` = LCS of `A[1..i]` and `B[1..j]`.  
**Recurrence**:  
$$\text{opt}(i, j) = \begin{cases} 
1 + \text{opt}(i-1, j-1) & \text{if } A[i] = B[j] \\ 
\max(\text{opt}(i-1, j), \text{opt}(i, j-1)) & \text{else}
\end{cases}$$  
**Base Cases**: `opt(i,0) = 0`, `opt(0,j) = 0`.  
**Solution**: Fill table row-wise → $O(nm)$ time.

---

### 6. Edit Distance
**Problem**: Min cost to transform `A` into `B` (insert/delete/replace).  
**Subproblems**: `opt(i, j)` = min cost for `A[1..i]` → `B[1..j]`.  
**Recurrence**:  
$$\text{opt}(i, j) = \min \begin{cases} 
c_D + \text{opt}(i-1, j) \\ 
c_I + \text{opt}(i, j-1) \\ 
\text{opt}(i-1, j-1) + [c_R \text{ if } A[i] \neq B[j]] 
\end{cases}$$  
**Base Cases**:  
- `opt(i, 0) = i \cdot c_D` (delete all),  
- `opt(0, j) = j \cdot c_I` (insert all).  
**Solution**: $O(nm)$ time.

---

## Advanced Applications

### 1. Matrix Chain Multiplication
**Problem**: Parenthesize matrices to minimize multiplications.  
**Subproblems**: `opt(i, j)` = min multiplications for $A_i \dots A_j$.  
**Recurrence**:  
$$\text{opt}(i, j) = \min_{i \leq k < j} \left[ \text{opt}(i,k) + \text{opt}(k+1,j) + s_i s_k s_j \right]$$  
**Base Case**: `opt(i, i) = 0`.  
**Order**: By chain length `L = 2 to n` → $O(n^3)$ time.

---

### 2. Shortest Path in DAG
**Problem**: Find shortest paths from source `s` (negative weights allowed).  
**Subproblems**: `opt(t)` = shortest path from `s` to `t`.  
**Recurrence**:  
$$\text{opt}(t) = \min_{(v,t) \in E} \left[ \text{opt}(v) + w(v,t) \right]$$  
**Base Case**: `opt(s) = 0`.  
**Solution**: Compute in topological order → $O(n + m)$ time.

---

### 3. Bellman-Ford (SSSP)
**Problem**: Shortest paths from `s` with negative weights (no negative cycles).  
**Subproblems**: `opt(i, t)` = shortest path with ≤ `i` edges.  
**Recurrence**:  
$$\text{opt}(i, t) = \min \left( \text{opt}(i-1, t), \min_{(v,t) \in E} \left[ \text{opt}(i-1, v) + w(v,t) \right] \right)$$  
**Base Cases**: `opt(0, s) = 0`, `opt(0, t) = \infty$ for `t ≠ s`.  
**Solution**: For `i = 1 to n-1` → $O(nm)$ time.

---

### 4. String Matching (Rabin-Karp)
**Problem**: Find pattern `B` in text `A`.  
**Key Insight**: Rolling hash for contiguous substrings.  
**Recurrence for Hash**:  
$$H(A^{s+1}) = d \cdot H(A^s) - d^m \cdot A[s] + A[s+m] \mod p$$  
**Solution**: Compute `H(B)` and `H(A^s)` for all `s` → $O(n + m)$ average time.

---

## Key Takeaways
- **When to Use DP**:  
  - Problems with recursive structure and overlapping subproblems.  
  - Optimization (min/max) or counting problems.  
- **Avoid Redundancy**: Store solutions to subproblems (table/memoization).  
- **Complexity**: Often polynomial, but pseudopolynomial for knapsack/coin change.

> *"DP is just recursion with memoization. Figure out the recurrence, and the rest is bookkeeping."* — Anonymous  
**Next**: Practice identifying subproblems and recurrences for new problems!

---

## Puzzle Solution Insight
Scale the table by 2× linearly. Original 25 coins cover half the area; 100 coins (1/4 size) cover the scaled table without gaps.
