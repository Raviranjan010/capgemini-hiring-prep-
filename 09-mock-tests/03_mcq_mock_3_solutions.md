[Home](../README.md) > [09-mock-tests](README.md) > 03_mcq_mock_3_solutions.md

# MCQ Mock 3: Solutions & Explanations

---

### MOCK3-001: Tokenization Subword Splitting (BPE)

**Correct Answer**: **A** (To handle out-of-vocabulary words and morphologically rich languages within a fixed vocabulary size)

#### Why
Subword tokenization breaks unseen words into known constituent subwords, preventing out-of-vocabulary exceptions.

- **5-Second Shortcut / Tip**: BPE = Handles rare/new words with compact vocab.
- **Trap**: Assuming whole-word vocab is smaller.

---
### MOCK3-002: Cross-Encoder vs Bi-Encoder in RAG Re-ranking

**Correct Answer**: **B** (Cross-encoders jointly process query and document via full cross-attention, which is highly accurate but too computationally expensive for million-scale search.)

#### Why
Bi-encoders precompute embeddings for sub-millisecond retrieval; cross-encoders perform full joint cross-attention on candidate top-k ($k \approx 50$) for high accuracy.

- **5-Second Shortcut / Tip**: Bi-encoder retrieves candidates fast; Cross-encoder re-ranks accurately.
- **Trap**: Thinking cross-encoders are fast enough for full scan.

---
### MOCK3-003: Self-Consistency Prompting Mechanics

**Correct Answer**: **B** (By sampling multiple distinct reasoning paths at non-zero temperature and taking the majority-voted answer)

#### Why
Self-consistency generates diverse reasoning traces and selects the final answer that achieves consensus across traces.

- **5-Second Shortcut / Tip**: Self-consistency = Sample multiple paths + Majority vote.
- **Trap**: Confusing with greedy single pass.

---
### MOCK3-004: Data Contamination in Benchmark Evals

**Correct Answer**: **B** (Test evaluation sets leaking into pre-training or fine-tuning datasets, causing inflated benchmark scores)

#### Why
Contamination occurs when models memorize test questions during training, disguising memorization as general reasoning.

- **5-Second Shortcut / Tip**: Contamination = Test data leaked into training.
- **Trap**: Confusing with runtime database bugs.

---
### MOCK3-005: Context Window Truncation Degradation

**Correct Answer**: **B** (The model answers based only on remaining partial text, missing critical conditions located in the truncated portion.)

#### Why
Truncation drops tail/head tokens, depriving the model of necessary context and producing incomplete or hallucinated answers.

- **5-Second Shortcut / Tip**: Truncation = missing critical truncated clauses.
- **Trap**: Expecting crash or random tokens.

---
### MOCK3-006: Responsible AI: Algorithmic Bias in Hiring Tools

**Correct Answer**: **A** (Historical / Training Data Bias)

#### Why
Models trained on biased historical human decisions replicate and amplify systemic historical biases.

- **5-Second Shortcut / Tip**: Historical bias = mirrored past human discrimination.
- **Trap**: Confusing with hardware quantization.

---
### MOCK3-007: Prompt Chaining vs Single Monolithic Prompt

**Correct Answer**: **B** (It isolates failure points, allows validation of intermediate outputs, and reduces reasoning load per step.)

#### Why
Modular prompts allow deterministic validation checks, retries of failed steps, and focused context per operation.

- **5-Second Shortcut / Tip**: Chaining = modular validation + reduced complexity.
- **Trap**: Assuming zero tokens.

---
### MOCK3-008: Hardcoded API Keys in AI Code

**Correct Answer**: **A** (Secret Credential Exposure Vulnerability)

#### Why
Hardcoded credentials in code repositories expose private API keys to unauthorized third parties and automated credential scrapers.

- **5-Second Shortcut / Tip**: Hardcoded secret = Credential leakage.
- **Trap**: Classifying as SQL injection.

---
### MOCK3-009: Embedding Dimensionality Trade-off

**Correct Answer**: **A** (Higher dimensions capture richer semantic nuance but increase vector index storage and retrieval latency.)

#### Why
Larger embedding dimensions preserve finer semantic distinctions at the cost of higher storage footprints and computation times.

- **5-Second Shortcut / Tip**: High dim = richer semantics, higher RAM/latency.
- **Trap**: Assuming smaller dims take more RAM.

---
### MOCK3-010: NeMo Guardrails Input Rail Function

**Correct Answer**: **B** (To intercept, sanitize, and validate user prompts before they reach the foundation model, blocking jailbreaks and off-topic queries)

#### Why
Input rails filter malicious payloads, toxic input, and prompt injections before the LLM processes the prompt.

- **5-Second Shortcut / Tip**: Input rails = pre-LLM security barrier.
- **Trap**: Confusing with network compression.

---
### MOCK3-011: DNS Resolution Record Types

**Correct Answer**: **B** (A record)

#### Why
'A' record maps hostname to IPv4; 'AAAA' maps to IPv6; 'CNAME' creates an alias.

- **5-Second Shortcut / Tip**: A record = IPv4 address.
- **Trap**: Confusing A with AAAA (IPv6).

---
### MOCK3-012: HTTP 401 vs 403 Status Codes

**Correct Answer**: **A** (401 means Unauthorized (missing/invalid credentials), while 403 means Forbidden (authenticated, but lacking permissions).)

#### Why
401 requires authentication; 403 acknowledges who you are but forbids access.

- **5-Second Shortcut / Tip**: 401 = Not authenticated; 403 = Authenticated but forbidden.
- **Trap**: Treating 401 and 403 as identical.

---
### MOCK3-013: SQL Window Function: ROW_NUMBER vs DENSE_RANK

**Correct Answer**: **B** (1, 1, 2)

#### Why
`ROW_NUMBER()` produces `1, 2, 3`; `RANK()` produces `1, 1, 3` (skipping 2); `DENSE_RANK()` produces `1, 1, 2` (no rank skipped).

- **5-Second Shortcut / Tip**: DENSE_RANK = no gap in rank numbers: 1, 1, 2.
- **Trap**: Confusing with standard RANK() gaps.

---
### MOCK3-014: SQL Correlated Subquery Mechanics

**Correct Answer**: **B** (A subquery that references column values from the outer query and must be evaluated for each row processed by the outer query)

#### Why
Correlated subqueries depend on the current outer query row, requiring per-row evaluation.

- **5-Second Shortcut / Tip**: Correlated = references outer query columns.
- **Trap**: Assuming single independent execution.

---
### MOCK3-015: BCNF vs 3NF Strictness

**Correct Answer**: **A** (For every functional dependency X -> Y, X must be a superkey.)

#### Why
3NF allows $X \to Y$ if $Y$ is a prime attribute. BCNF strictly requires $X$ to be a superkey for every dependency.

- **5-Second Shortcut / Tip**: BCNF = Left side must always be superkey.
- **Trap**: Confusing BCNF with 2NF.

---
### MOCK3-016: Database Index: B+ Tree vs Hash Index

**Correct Answer**: **A** (B+ Trees support efficient range queries (`BETWEEN`, `<`, `>`) and ordered scans.)

#### Why
B+ Tree leaf nodes are linked in sequential order, making range queries $O(\log N + K)$, whereas Hash indexes only support point equality ($=$).

- **5-Second Shortcut / Tip**: B+ Tree = Range queries + sorting.
- **Trap**: Assuming hash index supports range.

---
### MOCK3-017: Virtual Destructor Importance

**Correct Answer**: **B** (To ensure the derived class destructor is invoked when deleting a derived object through a base class pointer, preventing memory leaks)

#### Why
Without a virtual destructor, deleting via base pointer invokes only the base destructor, leaking derived class resources.

- **5-Second Shortcut / Tip**: Virtual destructor = safe polymorphic delete.
- **Trap**: Thinking it affects construction speed.

---
### MOCK3-018: C++ Copy Constructor Signature

**Correct Answer**: **B** (ClassName(const ClassName& other))

#### Why
Passing by reference (`const ClassName&`) is required to avoid infinite recursive copying.

- **5-Second Shortcut / Tip**: Copy constructor = const ClassName&.
- **Trap**: Passing by value (causes compiler error).

---
### MOCK3-019: Semaphore vs Mutex Key Difference

**Correct Answer**: **A** (A Mutex has ownership (only the locking thread can unlock it), whereas a Binary Semaphore can be signaled by any thread.)

#### Why
Mutex enforces ownership semantics (lock/unlock by same thread); semaphores are signaling mechanisms.

- **5-Second Shortcut / Tip**: Mutex = Ownership; Semaphore = Signaling.
- **Trap**: Treating them as identical.

---
### MOCK3-020: Paging vs Segmentation Memory Management

**Correct Answer**: **A** (Paging uses fixed-size memory blocks; Segmentation divides memory into variable-sized logical modules (functions, arrays).)

#### Why
Pages are fixed physical chunks (internal fragmentation only); segments reflect variable-size programmer logical views (external fragmentation).

- **5-Second Shortcut / Tip**: Paging = Fixed size; Segmentation = Logical variable size.
- **Trap**: Confusing fragmentation types.

---
### MOCK3-021: Process vs Thread Resource Sharing

**Correct Answer**: **C** (Heap memory and Global variables)

#### Why
Threads share code, data, and heap segments; each thread maintains its own private stack and register state.

- **5-Second Shortcut / Tip**: Shared = Heap + Globals; Private = Stack + PC.
- **Trap**: Assuming stack is shared.

---
### MOCK3-022: Circular Linked List Tail Insertion

**Correct Answer**: **A** (O(1))

#### Why
`tail->next` points directly to the head. Front insertion updates pointers in $O(1)$ constant time.

- **5-Second Shortcut / Tip**: Tail pointer gives O(1) front and rear access.
- **Trap**: Assuming O(N) traversal needed.

---
### MOCK3-023: Dijkstra's Algorithm Negative Weights Failure

**Correct Answer**: **B** (Its greedy assumption (that the shortest distance to a finalized vertex cannot be reduced further) is violated by negative edges.)

#### Why
Greedy finalization assumes paths only grow in weight; a negative edge can retroactively shorten an already visited node's path.

- **5-Second Shortcut / Tip**: Dijkstra greedy assumption fails on negative weights.
- **Trap**: Assuming Bellman-Ford has the same issue.

---
### MOCK3-024: Graph Bipartite Property

**Correct Answer**: **B** (Odd-length cycles)

#### Why
2-coloring succeeds if and only if the graph has no odd-length cycles.

- **5-Second Shortcut / Tip**: Bipartite = No odd cycles.
- **Trap**: Confusing odd cycles with even cycles.

---
### MOCK3-025: Space Complexity of Recursive Fibonacci

**Correct Answer**: **B** (O(N))

#### Why
Maximum stack depth along any single branch of the recursion tree is $N$, giving $O(N)$ space (despite $O(2^N)$ time).

- **5-Second Shortcut / Tip**: Stack depth = O(N); Time = O(2^N).
- **Trap**: Confusing time complexity with space complexity.

---
### MOCK3-026: Operator Precedence: Shift vs Relational

**Correct Answer**: **A** (1 (True))

#### Why
Bitwise shift `>>` has higher precedence than relational `<`. `8 >> 1 = 4`. Then `4 < 5` evaluates to true (`1`).

- **5-Second Shortcut / Tip**: Shift before relational: (8 >> 1) < 5 = 4 < 5 = 1.
- **Trap**: Evaluating relational first.

---
### MOCK3-027: Negative Integer Modulo Rule

**Correct Answer**: **A** (-2)

#### Why
`-20 = 6 * (-3) + (-2)`. Remainder takes the sign of dividend (-20), yielding `-2`.

- **5-Second Shortcut / Tip**: Sign follows dividend: -20 % 6 = -2.
- **Trap**: Applying Python result (+4).

---
### MOCK3-028: Short-Circuit with Logical AND Cascade

**Correct Answer**: **A** (11)

#### Why
In `a && ++c`, `a = 0` short-circuits, so `++c` is skipped. In `else if (b && ++c)`, `b = 1` evaluates right side: `++c` increments `c` to `1`. Condition is true: `c += 10 -> 1 + 10 = 11`.

- **5-Second Shortcut / Tip**: First branch skipped by a=0; second branch increments c=1, then adds 10 -> 11.
- **Trap**: Incrementing c in both branches.

---
### MOCK3-029: Toggling K-th Bit using XOR

**Correct Answer**: **A** (x = x ^ (1 << 2))

#### Why
XOR with a 1-bit toggles the bit (`0 ^ 1 = 1`, `1 ^ 1 = 0`).

- **5-Second Shortcut / Tip**: Toggle bit k: x ^ (1 << k).
- **Trap**: Using OR which only turns on.

---
### MOCK3-030: XOR Inversion for Parity Check

**Correct Answer**: **A** (Whether the two lowest bits of x differ)

#### Why
`x & 1` is bit 0; `(x >> 1) & 1` is bit 1. XOR-ing them yields 1 if they differ, and 0 if they match.

- **5-Second Shortcut / Tip**: XOR of bit 0 and bit 1 checks if adjacent bits differ.
- **Trap**: Assuming sign check.

---
### MOCK3-031: Fast Integer Multiplication via Shifts

**Correct Answer**: **A** ((x << 3) + x)

#### Why
$9x = 8x + x = (x \ll 3) + x$.

- **5-Second Shortcut / Tip**: 9x = (x << 3) + x.
- **Trap**: Using subtraction (7x).

---
### MOCK3-032: Nested Loop Quadratic Summation

**Correct Answer**: **A** (10)

#### Why
Outer passes: when $i=1$: 4; $i=2$: 3; $i=3$: 2; $i=4$: 1. Sum = $4 + 3 + 2 + 1 = 10$.

- **5-Second Shortcut / Tip**: Triangular number sum: 4+3+2+1 = 10.
- **Trap**: Assuming 4 x 4 = 16.

---
### MOCK3-033: Recursive Head and Tail Print Order

**Correct Answer**: **A** (2 1 1 2)

#### Why
Pre-order prints descending `2, 1`, base case returns, post-order prints ascending `1, 2`. Output: `2 1 1 2`.

- **5-Second Shortcut / Tip**: Symmetric unwind: 2 1 1 2.
- **Trap**: Missing post-order print.

---
### MOCK3-034: Binary Search Invariant Check

**Correct Answer**: **B** (It prevents signed 32-bit integer overflow when `low + high` exceeds $2^{31} - 1$)

#### Why
When array length approaches $2 \times 10^9$, `low + high` overflows integer limits.

- **5-Second Shortcut / Tip**: Prevents integer overflow.
- **Trap**: Assuming speed difference.

---
### MOCK3-035: Bitwise NOT Evaluation on Negative

**Correct Answer**: **A** (9)

#### Why
Formula: `~x = -(x + 1)`. For $x = -10$: `~(-10) = -(-10 + 1) = -(-9) = 9`.

- **5-Second Shortcut / Tip**: ~(-10) = 9.
- **Trap**: Guessing -11 or -9.

---
### MOCK3-036: Syllogism: 'Only' Conversion Rule

**Correct Answer**: **A** (Both I and II follow)

#### Why
'Only A are B' means 'All B are A' (All members are professionals, so I follows). Statement 2 places some athletes inside members, who are all professionals (II follows). Both follow.

- **5-Second Shortcut / Tip**: 'Only A are B' = 'All B are A'. Both follow.
- **Trap**: Misinterpreting 'Only'.

---
### MOCK3-037: Fallacy of Denying the Antecedent

**Correct Answer**: **B** (Maya may or may not receive a certificate (e.g. through exemption))

#### Why
Denying antecedent does not guarantee negation of consequent. Maya could receive certification via prior equivalent credit.

- **5-Second Shortcut / Tip**: Denying antecedent = Indeterminate.
- **Trap**: Concluding she definitely gets no certificate.

---
### MOCK3-038: Behavioral: Autonomous Troubleshooting Timebox

**Correct Answer**: **C** (Timebox independent research (e.g. 1-2 hours) with log analysis, then escalate with documented reproduction steps.)

#### Why
Timeboxed troubleshooting shows self-sufficiency, while structured escalation with clear context respects team sprint velocity.

- **5-Second Shortcut / Tip**: Timebox 1-2h independent effort, then structured escalation.
- **Trap**: Struggling indefinitely alone.

---
### MOCK3-039: Sentence Correction: Parallelism in Technical Writing

**Correct Answer**: **B** (The microservice is designed for scalability, reliability, and high performance.)

#### Why
All listed elements must be parallel nouns: 'scalability', 'reliability', and 'high performance'.

- **5-Second Shortcut / Tip**: Keep list items parallel in form.
- **Trap**: Mixing nouns with infinitive verbs.

---
### MOCK3-040: Speech Delivery: Speaking Cadence Benchmark

**Correct Answer**: **A** (120 - 150 words per minute)

#### Why
120-150 WPM matches natural professional conversational speech and maximizes acoustic clarity algorithms.

- **5-Second Shortcut / Tip**: 120-150 WPM conversational speed.
- **Trap**: Racing to speak over 200 WPM.

---
Previous: [03_mcq_mock_3.md](03_mcq_mock_3.md) | Next: [04_coding_mock.md](04_coding_mock.md)
