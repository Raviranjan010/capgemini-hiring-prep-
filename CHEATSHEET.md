# Capgemini Recruitment Assessment Cheatsheet

A concise, high-yield master reference summarizing core strategies, formulas, traps, and instant spotting rules across all assessment stages.

---

## 1. AI Literacy
- **GenAI vs SQL ("Librarian vs Author")**: SQL DB is a *Librarian* (deterministic retrieval of existing rows); LLM is an *Author* (probabilistic next-token generation generalizing beyond training data).
- **LLM Pipeline**: Raw Text $\to$ Tokenization (~100 words $\approx$ 130–135 tokens) $\to$ Vector Embeddings (semantic proximity) $\to$ Transformer Multi-Head Self-Attention $\to$ Probability Softmax $\to$ Next Token.
- **Context Window & Eviction**: Hard upper token limit for prompt + history + output. Exceeding limit silently drops earliest tokens (FIFO buffer eviction).
- **Prompt Formula ("Run To Catch Fast Cars")**: **R**ole • **T**ask • **C**ontext • **F**ormat • **C**onstraints (RTC-FC).
- **Prompt Chaining**: Decompose complex multi-step reasoning tasks into discrete, sequential prompt calls with verifiable intermediate outputs.
- **RAG Architecture**: Combine BM25/TF-IDF Sparse Keyword Search (exact token/code lookups) with Dense Vector Search (semantic similarity) via Hybrid Search.
- **Guardrails & Security**: Enforce Role-Based Access Control (RBAC) metadata filtering deterministically at retrieval time; sandbox untrusted external inputs in tags to prevent indirect prompt injection.
- **Prompt Strategies**: Use Zero-Shot Chain-of-Thought (*"Let's think step by step"*) for multi-step arithmetic; use Guided Decoding / JSON Schema constraints to guarantee valid JSON syntax.
- **Fine-Tuning vs Prompting**: Fine-tuning modifies internal weights ($W, b$); prompt engineering operates in-context on frozen weights.

---

## 2. Code Debugging (20 Minutes)
- **Time Protocol**: Hit RUN at Minute 2 to clear syntax errors; apply the 10-point scanner from Minutes 4 to 15; test edge cases in final 5 minutes.
- **Arrays & Matrices**: Enforce `< n` loop bounds. In 2D matrices, outer loop runs up to rows $n$, inner loop runs up to columns $m$.
- **Accumulators & Extremes**: Reinitialize per-row/per-level sums inside the outer loop. Initialize maximum trackers to `INT_MIN` (never `0`) to handle negative inputs.
- **BFS Queues**: Freeze level size with `int size = q.size()` before entering the loop; check `node.left != null` before enqueuing.
- **Pointers & References**: Guard pointer dereferencing with `node != null` and `fast.next != null`; compare node references (`slow == fast`) rather than values (`.val`).

---

## 3. AI-Assisted Coding (45 Minutes)
- **Decision Rule**:
  - Non-negative elements only ($\ge 0$) $\implies$ Two-Pointer Sliding Window ($O(N)$ Time, $O(1)$ Space).
  - Negative values, modulo constraints, or bitwise XOR $\implies$ Prefix Sum / Prefix XOR + HashMap ($O(N)$ Time, $O(N)$ Space).
  - Sliding Window Extremes (Min/Max over $K$) $\implies$ Monotonic Deque ($O(N)$ Time, $O(K)$ Space; never use a heap).
- **Accumulator Type**: Always declare prefix sums and running totals as `long` (Java) or `long long` (C++) to prevent 32-bit integer overflow.
- **Negative Modulo**: Always normalize remainders using `((r % k) + k) % k`.

---

## 4. Cognitive & Behavioral
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
