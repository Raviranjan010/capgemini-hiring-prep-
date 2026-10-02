# Capgemini Recruitment Assessment Cheatsheet

A concise, high-yield master reference summarizing core strategies, formulas, traps, and instant spotting rules across all assessment stages.

---

## 1. AI Literacy & GenAI
- **GenAI vs SQL ("Librarian vs Author")**: SQL DB is a *Librarian* (deterministic retrieval of existing rows); LLM is an *Author* (probabilistic next-token generation generalizing beyond training data).
- **LLM Pipeline**: Raw Text $\to$ Tokenization (~100 words $\approx$ 130–135 tokens) $\to$ Vector Embeddings (semantic proximity) $\to$ Transformer Multi-Head Self-Attention $\to$ Probability Softmax $\to$ Next Token.
- **Temperature ($T$) & Softmax**: $P(w_i) = \frac{e^{z_i / T}}{\sum_j e^{z_j / T}}$. Lower $T$ ($0.0 \le T \le 0.2$) for deterministic coding/SQL/compliance; Higher $T$ ($0.7 \le T \le 1.0$) for creativity / higher hallucination risk.
- **Context Window & Eviction**: Hard upper token limit for prompt + history + output. Exceeding limit silently drops earliest tokens (FIFO buffer eviction).
- **Prompt Formula ("Run To Catch Fast Cars")**: **R**ole • **T**ask • **C**ontext • **F**ormat • **C**onstraints (RTC-FC).
- **Prompt Strategies**: Zero-Shot CoT (*"Let's think step by step"*); Few-Shot (2–5 exemplars); Self-Consistency (majority vote over sampled paths).
- **RAG Architecture**: Ingestion (chunking + embeddings) $\to$ Retrieval (top-$k$ vector similarity) $\to$ Augmentation (grounding in prompt). Eliminates fine-tuning costs.
- **Guardrails & Security**: Enforce RBAC filtering at retrieval time; sandbox untrusted external inputs in tags (prevents indirect injection); implement dual-context guards (prevents direct jailbreaks); scrub PII (GDPR/DPDP compliance).
- **Fine-Tuning vs Prompting**: Fine-tuning modifies internal weights ($W, b$); prompt engineering operates in-context on frozen weights.

---

## 2. Technical MCQs & Operator Precedence
- **Master Precedence Rank**:
  $$\text{Grouping } () \ > \ \text{Unary } (++,\, !,\, \sim) \ > \ \text{Multiplicative } (*,\, /,\, \%) \ > \ \text{Additive } (+,\, -) \ > \ \text{Shifts } (\ll,\, \gg)$$
  $$\text{Relational } (<,\, <=) \ > \ \text{Equality } (==,\, !=) \ > \ \text{Bitwise } \& \ > \ \text{Bitwise } \oplus \ > \ \text{Bitwise } \mid \ > \ \text{Logical } \&\& \ > \ \text{Logical } \mid\mid \ > \ \text{Assignment } =$$
- **Stack Postfix**: Push operands; on operator, pop right operand first, then left operand; evaluate and push result back.
- **Tree Traversal Reconstruction**: Preorder first element is Root; locate Root in Inorder to partition Left and Right subtrees; recurse to derive Postorder.
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
  - **LCA in Binary Tree**: Post-order DFS; if `root == p || root == q` return root; if both subtrees non-null, root is LCA; else return non-null child.
  - **Bitwise Equality Inversions**: Count zeroes $Z \implies \frac{Z(Z - 1)}{2}$ using `long long`.
  - **Palindromic Partitioning Min Cuts**: Precompute 2D `isPal[i][j]` in $O(N^2)$, then 1D `dp[i] = min(dp[j - 1] + 1)`.
- **Accumulator Type**: Always declare prefix sums, counts, and totals as `long long` / `long` to prevent 32-bit integer overflow.
- **Negative Modulo**: Always normalize remainders using `((r % k) + k) % k`.

---

## 5. Cognitive & Behavioral
- **Motion Challenge**: Trace paths backwards from the target goal hole; position all sliding blocks before moving the ball; use boundary walls as anchors.
- **Bubble Memory**: Encode coordinates verbally as clock hours or compass directions; trace paths with your finger; chunk sequences into groups of 3.
- **Deductive Logic**: *"Some A are B"* converts symmetrically to *"Some B are A"* (never infer *"Some A are not B"*). Conditional $P \implies Q$ guarantees only the contrapositive $\neg Q \implies \neg P$.
- **Behavioral Profiling**: Prioritize collaboration over solitary heroics, deadline execution over unconstrained experimentation, and adaptability over complaints. Maintain consistency across disguised questions.

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
