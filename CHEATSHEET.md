# Capgemini Recruitment Assessment Cheatsheet

A concise, high-yield master reference summarizing core strategies, formulas, traps, and instant spotting rules across all assessment stages.

---

## 1. AI Literacy & GenAI
- **GenAI vs SQL ("Librarian vs Author")**: SQL DB is a *Librarian* (deterministic retrieval of existing rows); LLM is an *Author* (probabilistic next-token generation generalizing beyond training data).
- **LLM Pipeline**: Raw Text $\to$ Tokenization (~100 words $\approx$ 130–135 tokens) $\to$ Vector Embeddings (semantic proximity) $\to$ Transformer Multi-Head Self-Attention $\to$ Probability Softmax $\to$ Next Token.
- **Temperature ($T$) & Softmax**: $P(w_i) = \frac{e^{z_i / T}}{\sum_j e^{z_j / T}}$. Lower $T$ ($0.0 \le T \le 0.2$) for deterministic coding/SQL/compliance; Higher $T$ ($0.7 \le T \le 1.0$) for creativity / higher hallucination risk.
- **Context Window & Eviction**: Hard upper token limit for prompt + history + output. Exceeding limit silently drops earliest tokens (FIFO buffer eviction).
- **"Lost in the Middle" Effect**: Self-attention exhibits recency/primacy bias; middle/early tokens suffer attention dispersion in long contexts.
- **Guided Decoding (Schema Enforcement)**: Masks out invalid vocabulary tokens (logits $= -\infty$) using BNF grammar/JSON schema, mathematically guaranteeing structured syntax.
- **Prompt Formula ("Run To Catch Fast Cars")**: **R**ole • **T**ask • **C**ontext • **F**ormat • **C**onstraints (RTC-FC).
- **Prompt Strategies**: Zero-Shot CoT (*"Let's think step by step"* distributes compute over scratchpad tokens); Few-Shot (2–5 exemplars); Self-Consistency (majority vote over sampled paths).
- **BPE Tokenization Math Failure**: Numbers split into irregular subwords based on frequency, breaking decimal place value alignment (e.g., $9.11 > 9.9$).
- **RAG Architecture & Re-ranking**: Ingestion (semantic chunking with 10–20% sliding window overlap) $\to$ Fast Bi-Encoder Retrieval (Dot Product on normalized vectors) $\to$ Cross-Encoder Re-ranking (full query-document cross-attention) $\to$ LLM generation.
- **Evaluation Frameworks**: RAGAS / TruLens **Faithfulness** measures factual grounding in context; surface n-gram metrics (BLEU/ROUGE) cannot detect hallucination.
- **Guardrails & Security**: Enforce RBAC filtering at retrieval time; sandbox untrusted external inputs in tags (prevents indirect injection); implement dual-context guards (prevents direct jailbreaks); scrub PII (GDPR/DPDP compliance).
- **Fine-Tuning vs Prompting**: Fine-tuning modifies internal weights ($W, b$); prompt engineering operates in-context on frozen weights.

---

## 2. Technical MCQs & Pseudo-code Tracing
- **Pseudo-code Rules**:
  - **Inclusive Bounds**: `for i = a to b` includes both $a$ and $b$ (e.g., `0 to 4` = 5 iterations).
  - **Block Closures**: Structured statements must formally terminate (`end if`, `end for`); omission = **Syntax Error**.
  - **Scope Trap**: Re-declaring an accumulator (e.g., `sum = 0`) inside a loop resets its state on every iteration.
  - **Digital Root**: $1 + ((n - 1) \pmod 9)$ computes repeated sum of digits instantly in $O(1)$.
  - **Short-Circuit**: In `x && y`, if $x$ evaluates to $0$, $y$ never executes. In `x || y`, if $x$ is non-zero, $y$ never executes.
- **Master Precedence Rank**:
  $$\text{Grouping } () \ > \ \text{Unary } (++,\, !,\, \sim) \ > \ \text{Multiplicative } (*,\, /,\, \%) \ > \ \text{Additive } (+,\, -) \ > \ \text{Shifts } (\ll,\, \gg)$$
  $$\text{Relational } (<,\, <=) \ > \ \text{Equality } (==,\, !=) \ > \ \text{Bitwise } \& \ > \ \text{Bitwise } \oplus \ > \ \text{Bitwise } \mid \ > \ \text{Logical } \&\& \ > \ \text{Logical } \mid\mid \ > \ \text{Assignment } =$$
- **Stack Postfix**: Push operands; on operator, pop right operand first, then left operand; evaluate and push result back.
- **Tree & Graph Properties**:
  - **Visual Traversal Paths**:
    - **Pre-order** ($\text{Root} \to L \to R$): Left-perimeter outline ("Pant-shape" / top-to-bottom scan).
    - **In-order** ($L \to \text{Root} \to R$): Orthogonal projection onto horizontal axis (sorted order in BSTs).
    - **Post-order** ($L \to R \to \text{Root}$): Bottom-up leaf elimination up to root.
    - **Level-order**: Horizontal layer-by-layer scanning using Queue (BFS).
  - **Full vs Complete Binary Tree**: Full = strictly degree 0 or 2 children; Complete = all levels full except last (filled left-to-right; contiguous array friendly).
  - Circular Queue full condition: `(rear + 1) % N == front` (reserving 1 empty slot).
  - Comparison-based sorting theoretical lower bound: $\Omega(N \log N)$ (decision tree $N!$ leaves).
- **Sorting Algorithms & Invariants**:
  - **Bubble Sort**: Pass $k$ locks $k$ largest elements at `arr[n-k ... n-1]`. Pass 1 puts max element at `arr[n-1]`. Stable: **Yes** ($O(N)$ best, $O(N^2)$ worst, $O(1)$ space).
  - **Selection Sort**: Pass $k$ locks $k$ smallest elements at `arr[0 ... k-1]`. Stable: **No** ($O(N^2)$ all cases, $O(1)$ space).
  - **Insertion Sort**: Inserts into sorted prefix; optimal for nearly sorted arrays. Stable: **Yes** ($O(N)$ best, $O(N^2)$ worst, $O(1)$ space).
  - **Merge Sort**: Stable: **Yes** ($O(N \log N)$ all cases, $O(N)$ space).
  - **Quick Sort**: In-place partitioning. Stable: **No** ($O(N \log N)$ avg, $O(N^2)$ worst, $O(\log N)$ space).
  - **Heap Sort**: Binary max-heap. Stable: **No** ($O(N \log N)$ all cases, $O(1)$ space).
- **OS & DBMS Fundamentals**:
  - **2NF**: No partial dependencies (no non-prime attribute depends on a proper subset of any candidate key).
  - **Thrashing**: Total working sets exceed RAM frames, causing continuous page swapping and near-zero CPU execution.
  - **Hash Map Overwrite**: Inserting an existing key updates value in-place without altering map size or creating duplicates.
  - **Open Addressing (Linear Probing)**: $h(k, i) = (h(k) + i) \pmod M$. First empty slot receives the key after collision.
- **Bitwise Formulas**:
  - Lowest set bit: `n & (-n)`
  - Clear lowest set bit / Power of 2: `(n & (n - 1)) == 0`
  - Bitwise Equality: $A[i] \ \& \ A[j] == A[i] \oplus A[j] \iff A[i] = 0 \text{ and } A[j] = 0 \implies \binom{Z}{2} = \frac{Z(Z - 1)}{2}$.

---

## 3. Code Debugging (20 Minutes)
- **Time Protocol**: Hit RUN at Minute 2 to clear syntax errors; apply the 10-point scanner from Minutes 4 to 15; test edge cases in final 5 minutes.
- **Four Primary Exceller Failure Modes**:
  1. **Undirected Graphs**: Pushing only `adj[u].push_back(v)` without reverse link `adj[v].push_back(u)`.
  2. **Premature Loop Exit**: `return result;` placed accidentally inside `for` loop body.
  3. **0-Indexed vs 1-Indexed**: Allocating size $n$ instead of $n + 1$ or accessing out-of-bounds index `dp[n]`.
  4. **0/1 Knapsack 1D DP**: Forward loop (`w = wt[i]; w <= W`) converts problem to Unbounded Knapsack; **must loop backwards** (`w = W; w >= wt[i]; w--`).

---

## 4. AI-Assisted (Vibe) Coding (20 Minutes)
- **Prompt Format**: State Role, Context, Task, Data Structures, Base Cases, and Asymptotic Bounds in Turn 1.
- **Decision Rule**:
  - Non-negative elements only ($\ge 0$) $\implies$ Two-Pointer Sliding Window ($O(N)$ Time, $O(1)$ Space).
  - Negative values, modulo constraints, or bitwise XOR $\implies$ Prefix Sum / Prefix XOR + HashMap ($O(N)$ Time, $O(N)$ Space).
  - Sliding Window Extremes (Min/Max over $K$) $\implies$ Monotonic Deque ($O(N)$ Time, $O(K)$ Space; never use a heap).
- **Core Problem Patterns**:
  - **Decode Ways (LC #91)**: Single-digit if $s[i-1] \ne '0' \implies \text{add } dp[i-1]$; Double-digit if $10 \le \text{val}(s[i-2 \dots i-1]) \le 26 \implies \text{add } dp[i-2]$. Base case $\text{dp}[0] = 1$. Rolling state compression (`prev1`, `prev2`) achieves $O(1)$ space. Leading `'0'` collapses count to `0`.
  - **Coin Change (LC #322)**: Initialize DP array with `amount + 1` (never `Integer.MAX_VALUE`, which overflows to `Integer.MIN_VALUE` upon adding 1). Transition: $\text{dp}[i] = \min(\text{dp}[i], 1 + \text{dp}[i - \text{coin}])$.
  - **Strict Alternating Parity**: Check parity violations with `((arr[i] ^ arr[i - 1]) & 1) == 0`. Never use `arr[i] % 2 == 1` because negative odd numbers return `-1` in Java.
  - **Move Hashes / Run-Length Encoding**: Use `StringBuilder` in a single pass ($O(N)$ time and space) to avoid $O(N^2)$ memory churn from immutable String concatenation.
  - **Is Graph Bipartite?**: Alternating 2-coloring via BFS using bitwise XOR (`color[v] = color[u] ^ 1`). A graph is bipartite $\iff$ it has **NO odd-length cycles**. Must iterate an outer loop $0 \le i < V$ to handle disconnected components.
  - **Course Schedule (Cycle Detection)**: Model as directed dependency graph. Apply Kahn's Algorithm (BFS Topological Sort) using an `inDegree` array. Push `inDegree == 0` nodes to queue; schedule is valid if processed node count equals $N$.
  - **Number of Islands**: BFS flood fill; sink visited cells in-place (`grid[r][c] = '0'`) immediately upon enqueuing to eliminate $O(M \times N)$ auxiliary visited memory.
  - **0/1 Matrix Shortest Distance**: Multi-source BFS initializing all `0` cells with distance `0` and enqueuing them simultaneously; expands in $O(M \times N)$ time without running redundant single-source BFS passes.
  - **LCA in Binary Tree**: Post-order DFS; if `root == p || root == q` return root; if both subtrees non-null, root is LCA; else return non-null child.
  - **Bitwise Equality Inversions**: Count zeroes $Z \implies \frac{Z(Z - 1)}{2}$ using `long long`.
  - **Palindromic Partitioning Min Cuts**: Precompute 2D `isPal[i][j]` in $O(N^2)$, then 1D `dp[i] = min(dp[j - 1] + 1)`.
- **Accumulator Type**: Always declare prefix sums, counts, and totals as `long long` / `long` to prevent 32-bit integer overflow.
- **Negative Modulo**: Always normalize remainders using `((r % k) + k) % k`.

---

## Master High-Yield Patterns Matrix

| Topic | Frequently Tested Pattern | Rule to Remember |
| :--- | :--- | :--- |
| **Arrays & Pointers** | Move zeroes / Move # to front | Two-pointer technique; do not allocate redundant arrays. Use `StringBuilder` in Java. |
| **String Manipulation** | Anagram check, Run-length encoding | Use frequency array `int[26]` for lowercase alphabet letters; single-pass runs. |
| **Dynamic Programming** | Decode Ways, Coin Change, House Robber | Identify transition dependency window ($k=2 \implies O(1)$ space using rolling variables). |
| **Bitwise Operations** | $(A \ \& \ B) \mid (A \wedge B)$, parity checking | $(A \ \& \ B) \mid (A \oplus B) \equiv A \mid B$; Parity violation: `((a ^ b) & 1) == 0`. |
| **SQL & DBMS** | WHERE vs HAVING, Second highest salary | `WHERE` filters rows before aggregation; `HAVING` filters aggregated groups. |
| **AI Assessment** | Prompting, RAG, Hallucination | State inputs, clear constraints, and edge cases to score high; $T \to 0$ for deterministic code. |

---

## 5. Cognitive & Behavioral Assessment
- **Switch Challenge**:
  - **Operator Mechanics**: Digit at index $i$ means *"pull from previous index $d_i$ into position $i$"*.
  - **Anchor Element Strategy**: Track a single unique symbol (e.g. $\bigstar$) to eliminate 2–3 options in 5–10 seconds.
  - **Inverse Operator**: $P^{-1}$ maps elements back to original indices. Self-inverse permutations (e.g., `3 4 1 2`) invert to themselves.
- **Digit / Symbol Grid Challenge (Mini-Sudoku)**:
  - **Latin Square Property**: Every row and column must contain each symbol exactly once.
  - **Solving Priority**: Solve Maximum Density rows/columns (3 of 4 filled) first $\to$ Intersection Cross-Check $\to$ Hypothesis backtracking.
- **Pattern Recognition & Visual Reasoning**:
  - **Odd-One-Out Detection**: Calculate degree delta per jump ($+45^\circ, +90^\circ, +135^\circ$) to spot rotational violations.
  - **Count Invariance**: Verify segment line counts match polygon sides ($n$-gon has $n$ lines).
  - **Symmetry Checks**: Distinguish between $180^\circ$ point-rotational symmetry ($C_2$, e.g., letter N) and bilateral reflection line symmetry ($D_1/D_2$, e.g., H, I, X, O).
  - Check row/column symmetry mirrors, total filled symbol counts, and $90^\circ / 180^\circ$ rotational invariance.
- **Digit Equation Balancing**:
  - Bound the multiplier first: in $A \times B \pm C = T$, approximate $A \times B$ close to $T$, then resolve $C$ with single digits ($1 \le d \le 9$).
- **Motion Challenge**:
  - Reverse path planning: work backward from the target goal hole (*"Which obstacle directly blocks the goal? Move it first"*).
  - Move count scoring: game scores total moves regardless of whether a block or ball is moved. A 5-step ball detour beats 3 block slides + 3 ball steps (6 moves).
  - Move all obstacles into clearance pockets first before executing uninterrupted ball slides.
- **Bubble Memory**:
  - Cowan's working memory model ($4 \pm 1$ items): divide long sequences into two 4-item batches grouped by screen quadrant.
  - Encode coordinates verbally as clock hours or compass directions; trace paths with your finger; chunk sequences into groups of 3.
- **Deductive Logic & Syllogisms**:
  - Conversion: *"Some A are B"* converts symmetrically to *"Some B are A"* (never infer *"Some A are not B"*).
  - Contrapositive: $(P \implies Q) \equiv (\neg Q \implies \neg P)$.
  - Modus Tollens: $(P \implies Q) \land \neg Q \implies \neg P$.
  - Sub-alternation: *"All A are B"* unconditionally guarantees *"Some A are B"*.
  - Disjoint Chain: $(F \subseteq P) \land (M \cap P = \emptyset) \implies F \cap M = \emptyset$.
- **Behavioral Profiling**: Prioritize collaboration over solitary heroics, deadline execution over unconstrained experimentation, and adaptability over complaints. Maintain consistency across disguised questions.

---

## 6. Exam-Day Strategic Checklist

### Cognitive Games (Keep Calm & Track Singular Elements)
- [ ] **Switch Challenge**: Do not attempt to map all 4 symbols simultaneously. Track the path of a single symbol (Anchor Element) to eliminate wrong multiple-choice options in seconds.
- [ ] **Visual Reasoning**: Track degree change per step ($+45^\circ, +90^\circ$) and check internal element counts before checking rotation.
- [ ] **Motion Challenge**: Work backward from the target goal. Ask: *"Which obstacle is directly blocking the target hole?"* Move that obstacle first.
- [ ] **Grid / Sudoku**: Scan for rows or columns with only one missing slot before trying to resolve cells with multiple candidate symbols.
- [ ] **Digit Balancing**: Anchor the multiplication pair close to the target value first; check remaining difference fits in single digits.
- [ ] **Bubble Memory**: Use the 3-item chunking technique and physical cursor tracing during rapid flashes.

### English & Communication (6 Modules)
- [ ] **Workplace Escalation Emails**: Address all prompt constraints (out-of-office lead, staging downtime, explicit update deadline by 3:00 PM). Never make unauthorized technical commitments.
- [ ] **Spoken Technical Speech (90s)**: Structure strictly as 25s Productivity, 30s QA/Hallucination Risks, 35s Ethics/IP Governance.
- [ ] **Listening Comprehension**: Distinguish event start time (Thursday 3:00 PM audit) from preparation deadline (Wednesday evening log scans).
- [ ] **Grammar Correction**: Remember proximity rule for *Neither...nor* (verb agrees with closest subject) and verify introductory participial clauses modify the immediate subject (dangling modifiers).

### Technical Traversals & Algorithms
- [ ] **Pre-order**: Check if the very first printed value matches the root node.
- [ ] **In-order**: Check if values from a Binary Search Tree appear in sorted ascending order.
- [ ] **Bubble Sort**: Remember that each pass locks the next largest value into the rightmost position ($k$ largest at end).
- [ ] **Hash Maps**: Inserting an existing key updates the value in place; it never creates a duplicate entry or changes size.
- [ ] **Open Addressing**: On collision at $h(k)$, probe sequentially $(h(k) + i) \pmod M$ to find the first free index.

---

## 10-Second Spotting List
1. **Tree Balancing**: Check if `left == -1` returns `0`, `Math.abs` checks `>= 1`, or `return 1 + Math.max` is missing.
2. **BFS Level Order**: Check for dynamic `q.size()` in loop header or pushing null children without guards.
3. **Matrix Row Sum**: Check for `maxSum = 0` (needs `INT_MIN`), swapped `n`/`m` bounds, and un-reset `rowSum = 0`.
4. **Sorted Matrix Search**: Verify start is top-right `(0, m - 1)` and boundary check strictly uses `&&` (never `||`).
5. **Jump Game I**: Check for inverted reach check (`i < maxReach`), missing early exit, or trailing `return false;`.
6. **Jump Game II**: Check if loop runs to `< n` instead of `< n - 1`, or uses `nums[i]` instead of `i + nums[i]`.
7. **Gas Station**: Check for `start = i` instead of `i + 1`, and reset condition checking `<= 0` instead of `< 0`.
8. **Merge Intervals**: Check comparator (`a[0]`), inclusive overlap (`<=`), and missing trailing interval after loop.
9. **Linked List Cycle**: Check for `fast.next != null` guard and pointer reference comparison (`slow == fast`).
10. **Monotonic Stack**: Verify reverse iteration (`n - 1 down to 0`) and `!st.empty()` guards before every `top()` / `pop()`.
11. **Subsets Backtracking**: Check for `new ArrayList<>(current)` snapshot, `i + 1` recursion, and state undo `remove()`.
12. **Undirected Graph DFS**: Check if `adj[edge[1]].push_back(edge[0])` is missing or `return components;` is inside the loop.
13. **0/1 Knapsack 1D DP**: Check if capacity loop runs forward (`w++`); must be reverse (`w--`).
14. **Bitwise AND vs Equality**: Check `x & 1 == 0` precedence trap; `==` runs first, so use `(x & 1) == 0`.
15. **Validate BST**: Check if code only tests immediate children (`root->left->val >= root->val`); must pass down global ancestor bounds `(minVal, maxVal)`.
16. **Directed Graph Cycle**: Check for missing `inStack[u] = false;` on backtrack before returning `false`, falsely flagging subsequent paths as cycles.
17. **Kadane's Algorithm**: Check if `maxSoFar` / `currMax` are initialized to `0`; returns 0 on all-negative arrays (must initialize to `nums[0]`).
18. **Binary Search (Bounds)**: Check for `high = mid` (causes TLE / infinite loop when `low + 1 == high`) and `(low + high) / 2` (integer overflow); fix to `high = mid - 1` and `low + (high - low) / 2`.
19. **Undirected Graph BFS**: Check if `q.pop()` is called immediately after `q.front()` (omission causes infinite loop TLE); verify `vis[0] = true` root initialization; ensure `vis[neighbor] = true` is marked on enqueue (not dequeue) to avoid duplicate pushes.
20. **Palindrome Partitioning DP**: In 2D table `isPal[i][j] = (s[i] == s[j]) && (j - i <= 2 || isPal[i + 1][j - 1])`, check outer loop direction: `for (int i = 0; i < n; i++)` reads uncomputed row $i + 1$; must loop backwards `for (int i = n - 1; i >= 0; i--)`.
21. **Decode Ways**: Check leading `'0'` guard (`s.charAt(0) == '0' -> return 0`) and base case `dp[0] = 1`. Intermediate zero with preceding digit $> 2$ (e.g., `"30"`) immediately collapses ways to `0`.
22. **Coin Change DP Overflow**: Check initialization; using `Integer.MAX_VALUE` triggers 32-bit signed integer overflow when computing `dp[i - c] + 1` (must initialize with `amount + 1`).
23. **Bitwise Logic Shortcut**: Recognize $(A \ \& \ B) \mid (A \oplus B)$ as Boolean identity for $A \mid B$. Check parity with `((a ^ b) & 1) == 0`.
24. **Array Frequency / Bounds**: Check `for (int i = 0; i <= arr.length; i++)` (causes `ArrayIndexOutOfBoundsException`) and ensure frequencies use a `HashMap` or sorted scan to avoid printing duplicates.


