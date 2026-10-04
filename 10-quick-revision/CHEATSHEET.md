[Home](../README.md) > [10-quick-revision](README.md) > CHEATSHEET.md

# Capgemini Quick Revision Cheatsheet

High-speed formula, trick, and bug reference for last-minute review.

## 1. Bitwise Manipulation
- `x & (x - 1)`: Clears lowest set bit (Brian Kernighan).
- `x & (-x)`: Isolates lowest set bit.
- `(x & (x - 1)) == 0`: Checks if $x > 0$ is a power of 2.
- `~x = -(x + 1)`: Two's complement inversion (`~0 = -1`, `~(-1) = 0`, `~7 = -8`).
- `x ^ x = 0`, `x ^ 0 = x`: XOR self-cancellation for finding lone non-duplicate.
- `a ^= b; b ^= a; a ^= b;`: In-place swap without extra variable.
- `x << k = x * 2^k`; `x >> k = floor(x / 2^k)`.

## 2. Operator Precedence & Modulo
- **Precedence Order**: `()` -> `++`, `--` -> `*`, `/`, `%` -> `+`, `-` -> `<<`, `>>` -> `<`, `>` -> `==`, `!=` -> `&` -> `^` -> `|` -> `&&` -> `||` -> `?:` -> `=`.
- **Trap**: `a & 1 == 0` evaluates as `a & (1 == 0) = 0`. Use `(a & 1) == 0`.
- **Trap**: `a << 1 + 2` evaluates as `a << (1 + 2) = a << 3`.
- **Negative Modulo (C/Java)**: Dividend sign dictates remainder: `(-14) % 3 = -2`.
- **Short-Circuit**: In `A && B`, if $A$ is false, $B$ never runs. In `A || B`, if $A$ is true, $B$ never runs. Increments inside skipped branches do NOT execute.

## 3. Code Debugging Quick Rules
- **Binary Search**: `mid = low + (high - low) / 2` (prevents integer overflow). Pointers: `low = mid + 1`, `high = mid - 1`. Loop: `while (low <= high)`.
- **Kadane's Algorithm**: Initialize `maxSoFar = nums[0]`, never 0 (fails all-negative arrays).
- **2D Matrix Loops**: Outer `row < n`, inner `col < m`. Reset `rowSum = 0` inside outer loop.
- **Top-Right Matrix Search**: Start at `(0, m - 1)`. If `val < target` move down (`r++`); if `val > target` move left (`c--`). Guard: `r < n && c >= 0`.
- **Jump Game**: Reach update is `Math.max(reach, i + nums[i])`. Reaching `nums.length - 1` needs `< n - 1` loop in Jump Game II.
- **0/1 Knapsack**: 1D DP capacity loop MUST run in reverse: `for (int w = W; w >= wt[i]; w--)`. Forward loop converts to unbounded knapsack.
- **Graphs**: Undirected edges require both `adj[u].add(v)` and `adj[v].add(u)`. Mark visited immediately upon enqueue in BFS.

## 4. AI Literacy & GenAI Core Facts
- **RAG Hybrid Search**: Dense vectors capture semantics; BM25 matches exact alphanumeric error codes and SKUs.
- **Lost-in-the-Middle**: Put critical retrieved chunks at the very beginning or end of prompt.
- **Prompt Injection**: System override attacks. Use XML delimiters (`<context>`) to separate instructions from untrusted data.
- **Temperature / Top-p**: Use low temperature ($T \le 0.2$) for deterministic JSON/SQL code; high $T$ for creative writing.
- **RLHF**: Uses reward model scoring human preference pairs (helpfulness, honesty, safety).

## 5. CS Fundamentals & SQL
- **Subnetting**: Usable hosts in `/n` = $2^{32-n} - 2$. (`/24` = 254; `/28` = 14).
- **SQL NULL**: `val = NULL` evaluates to UNKNOWN. Always use `IS NULL` or `IS NOT NULL`.
- **WHERE vs HAVING**: WHERE filters rows before aggregation; HAVING filters groups after `GROUP BY`.
- **Banker's Algorithm**: `Need = Max - Allocation`. Uses matrices/vectors, not graphs.
- **Syllogisms**: "Some A are B" does NOT guarantee "Some A are not B". "All A are B" does NOT guarantee "All B are A". Two negative premises yield no definite conclusion.

---

Previous: [README.md](README.md) | Next: [formulas-and-traps.md](formulas-and-traps.md)
