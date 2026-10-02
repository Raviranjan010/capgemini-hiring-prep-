# Code Debugging Round (20-Minute Protocol)

## The 20-Minute Action Plan

```text
[00:00 - 02:00]  Read Problem & Constraints: Check for negative values, empty/single-element inputs, 0-indexing, matrix dimensions.
       │
[02:00 - 04:00]  Click RUN Immediately: Flush out compilation errors, missing semicolons, and type mismatches.
       │
[04:00 - 15:00]  Apply the 10-Point Bug Scanner: Locate logical, structural, and traversal bugs.
       │
[15:00 - 20:00]  Stress Test Edge Cases: Single element, all negatives, empty input, duplicates, Integer.MIN_VALUE.
```

---

## The 10-Point Bug Scanner

1. **Pointer Dereference Guard**: Verify `node != null` is evaluated *before* accessing `node.val`, `node.left`, or `fast.next.next`.
2. **Loop Boundaries**: Verify 0-indexed loops run `< n` instead of `<= n`. For adjacent element comparisons, verify the boundary stops at `< n - 1`.
3. **2D Matrix Dimensions**: Verify outer row loops iterate up to row count $n$ (`mat.length`) and inner column loops iterate up to column count $m$ (`mat[0].length`).
4. **Accumulator Resets**: Ensure per-row, per-level, or per-window sums (e.g., `rowSum = 0`) are reinitialized inside the outer loop.
5. **Min/Max Initialization**: When inputs can contain negative values, initialize maximum trackers to `INT_MIN` or `Integer.MIN_VALUE` (never `0`).
6. **Comparison vs Assignment**: Check for accidental assignment operators (`if (val = target)`) instead of equality checks (`==`).
7. **Boolean Connectors**: Verify boundary conditions use logical AND (`&&`) to ensure all dimensions are valid, rather than logical OR (`||`).
8. **Loop Updates & Pointer Advances**: Check that while-loop pointers (`left++`, `right--`, `curr = curr.next`, `row++`, `col--`) advance unconditionally to prevent infinite loops (TLE).
9. **Base Cases & Error Propagation**: Ensure recursive base cases return valid markers (e.g., propagating `-1` immediately up tree height recursion, checking strict inequality `Math.abs(left - right) > 1`).
10. **Integer Division & Overflow Cast**: Ensure divisions cast to floating point (`(double) a / b`) where decimals matter, and use `long` accumulators for sums exceeding $2 \times 10^9$.

---

## Bug Type Quick Reference Table

| Bug Category | Where to Look in Code | Common Culprit in Exam |
| :--- | :--- | :--- |
| **Tree Balancing** | Recursive return statements & differences | Missing `-1` bubble-up; checking `>= 1` instead of `> 1`; forgetting `+ 1` on height. |
| **BFS Level Order** | Queue size evaluation & child insertion | Evaluating `q.size()` dynamically in the loop header; pushing null children. |
| **2D Matrix Traversal** | Loop bounds & per-row accumulators | Inverted row/col indices ($n$ vs $m$); initializing `maxSum = 0` with negative values. |
| **Sorted Matrix Search** | Starting corner & movement logic | Starting at top-left instead of top-right; using `||` instead of `&&` in boundary check. |
| **Jump Game Reach** | Reach check condition & loop termination | Inverted reach check (`i < maxReach`); missing early exit when `maxReach >= n - 1`. |
| **Jump Game II Jumps** | Loop termination index & reach formula | Running loop up to `< n` instead of `< n - 1`; using `nums[i]` instead of `i + nums[i]`. |
| **Circular Gas Tour** | Station pointer reset & zero balance | Setting `start = i` instead of `i + 1`; resetting on `<= 0` instead of strictly `< 0`. |
| **Interval Merging** | Sorting comparator & end updates | Sorting by end time; using `<` instead of `<=`; forgetting trailing interval after loop. |
| **Linked List Fast/Slow**| Loop guard conditions & node equality | Dereferencing `fast.next.next` without checking `fast.next != null`; comparing `.val` instead of `==`. |
| **Monotonic Stack** | Stack query order & empty guard | Calling `st.top()` without `!st.empty()`; forward iteration instead of reverse. |

---

## Video References
- Common Debugging Bugs Walkthrough: See [RESOURCES.md](../RESOURCES.md#video-references) (unverified link: https://youtu.be/f_9-TT2hGQ4).
- Height-Balanced Tree Debugging: See [RESOURCES.md](../RESOURCES.md#video-references) (unverified link: https://youtu.be/YEZy2e_PARE).
- 2D Matrix Debugging (7 Bugs): See [RESOURCES.md](../RESOURCES.md#video-references) (unverified link: https://youtu.be/SjbedEKjacM).
- Jump Game I Live Walkthrough: See [RESOURCES.md](../RESOURCES.md#video-references) (verified playlist link).

## Module Files
1. [01_trees.md](01_trees.md) - Height-Balanced Tree, Level Order BFS, and Zigzag Traversal.
2. [02_matrix.md](02_matrix.md) - 2D Matrix Maximum Row Sum (7 bugs) and Staircase Search in Sorted Matrix.
3. [03_greedy_and_intervals.md](03_greedy_and_intervals.md) - Jump Game I & II, Gas Station Circular Tour, and Merge Intervals.
4. [04_linkedlist_stack_backtracking.md](04_linkedlist_stack_backtracking.md) - Linked List Cycle, Next Greater Element, and Subsets Backtracking.
