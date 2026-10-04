[Home](../README.md) > [08-recent-patterns](README.md) > 03_debugging.md

# 03. Code Debugging Reported Patterns

This document outlines the systematic bugs intentionally planted in Capgemini's 20-minute code debugging round.

## Reported Pattern Archetypes

### 1. Planted Off-by-One and Loop Boundary Bugs
- **Reported In**: Video Debriefs
- **Teached By**: `DBG-005`, `DBG-006`, `DBG-008`, `DBG-019`, `DBG-022`
- **Pattern Details**:
  - 0-indexed loops running `<= n` instead of `< n`, causing `ArrayIndexOutOfBoundsException`.
  - Jump Game II running loop to `< n` instead of `< n - 1`, triggering an extra unnecessary jump at the target index.
  - Binary search midpoint using `(low + high) / 2` causing signed integer overflow on large arrays, or `low = mid` causing infinite loops.

### 2. Accumulator & Tracker Re-Initialization Faults
- **Reported In**: Video Debriefs
- **Teached By**: `DBG-005`, `DBG-018`, `DBG-020`, `DBG-036`
- **Pattern Details**:
  - In 2D matrix row-sum calculations, declaring `rowSum` outside the loop without resetting it to 0 per row.
  - Initializing maximum trackers to `0` instead of `Integer.MIN_VALUE` or `nums[0]`, completely failing when all inputs are negative numbers.
  - Minimization DP tables defaulting to `0` in Java, causing `Math.min(0, ...)` to return `0` instead of identifying shortest paths or fewest coins.

### 3. Directionality & State Restoration in Graphs and DP
- **Reported In**: Gemini Chat & Video Debriefs
- **Teached By**: `DBG-014`, `DBG-015`, `DBG-016`, `DBG-017`
- **Pattern Details**:
  - Adding unidirectional edges (`adj[u].add(v)`) in undirected graphs.
  - In space-optimized 0/1 knapsack, looping capacity forward (`w = wt[i] to W`), turning it into an unbounded knapsack by reusing the current item.
  - In directed graph cycle detection, forgetting to reset `inStack[u] = false` during backtracking, causing false cycle detections on subsequent disjoint paths.

---

Previous: [02_cs_and_pseudocode.md](02_cs_and_pseudocode.md) | Next: [04_ai_assisted_coding.md](04_ai_assisted_coding.md)
