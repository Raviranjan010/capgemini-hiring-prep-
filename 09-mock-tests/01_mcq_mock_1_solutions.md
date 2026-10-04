[Home](../README.md) > [09-mock-tests](README.md) > 01_mcq_mock_1_solutions.md

# MCQ Mock 1: Solutions & Explanations

---

### MOCK1-001: Vector Search Distance Metric for Normalized Embeddings

**Correct Answer**: **A** (Cosine Similarity)

#### Why
For unit-normalized vectors, cosine similarity equals dot product directly.

- **5-Second Shortcut / Tip**: Cosine = Dot Product when |u| = |v| = 1.
- **Trap**: Assuming Euclidean distance is unrelated.

---
### MOCK1-002: Indirect Prompt Injection Source Vector

**Correct Answer**: **B** (An automated web scraper feeding a malicious webpage containing hidden system instructions into an LLM summary agent)

#### Why
Indirect prompt injection occurs when the attack payload arrives through third-party external data rather than the user's direct chat input.

- **5-Second Shortcut / Tip**: Indirect = from external data/web/email.
- **Trap**: Confusing with direct jailbreak.

---
### MOCK1-003: Low Temperature for Code Generation

**Correct Answer**: **B** (It enforces deterministic token selection by concentrating probability mass on top logits.)

#### Why
Low temperature flattens softmax variance, forcing greedy selection of the most probable tokens for syntax validity.

- **5-Second Shortcut / Tip**: Low temp = deterministic syntax.
- **Trap**: Thinking temp alters network speed.

---
### MOCK1-004: RAG Chunking Semantic Loss

**Correct Answer**: **B** (Loss of sentence-level context and semantic fragmentation)

#### Why
Tiny chunks split dependent clauses from their subjects, destroying semantic coherence.

- **5-Second Shortcut / Tip**: Tiny chunk = broken context.
- **Trap**: Assuming small chunks cause GPU OOM.

---
### MOCK1-005: Hallucination vs Knowledge Cutoff

**Correct Answer**: **B** (Hallucination)

#### Why
Hallucination refers to generating factually fabricated, plausibly sounding statements without ground-truth support.

- **5-Second Shortcut / Tip**: Fabricated citations = Hallucination.
- **Trap**: Confusing with data drift.

---
### MOCK1-006: Chain-of-Thought Prompting Advantage

**Correct Answer**: **B** (It forces the model to generate intermediate reasoning tokens before reaching a final answer.)

#### Why
Generating intermediate reasoning tokens allows transformer attention to condition final answers on derived intermediate facts.

- **5-Second Shortcut / Tip**: CoT = intermediate computation scratchpad.
- **Trap**: Thinking CoT updates weights.

---
### MOCK1-007: BM25 Hybrid Search Necessity

**Correct Answer**: **A** (Dense embeddings fail to retrieve exact alphanumeric identifiers and error codes.)

#### Why
Dense semantic embeddings smooth out specific alphanumeric characters (SKUs, IDs, error codes), which BM25 matches exactly.

- **5-Second Shortcut / Tip**: BM25 catches exact alphanumeric codes.
- **Trap**: Assuming BM25 is a transformer.

---
### MOCK1-008: PII Exposure in LLM Prompts

**Correct Answer**: **B** (Apply automated PII masking or tokenized pseudonymization)

#### Why
PII must be masked or redacted before crossing regulatory and third-party API trust boundaries.

- **5-Second Shortcut / Tip**: Mask PII before API calls.
- **Trap**: Assuming compression provides legal compliance.

---
### MOCK1-009: RLHF Reward Model Objective

**Correct Answer**: **B** (To score candidate completions based on human preference data (helpfulness, honesty, safety))

#### Why
The reward model evaluates LLM responses and produces a scalar reward reflecting alignment with human preferences.

- **5-Second Shortcut / Tip**: Reward model = scalar alignment score.
- **Trap**: Confusing with vector re-ranker.

---
### MOCK1-010: RAGAS Faithfulness Metric

**Correct Answer**: **B** (The ratio of claims in the generated answer that can be directly inferred from the retrieved context)

#### Why
Faithfulness measures groundedness: ensuring every generated claim is supported by the retrieved context chunks.

- **5-Second Shortcut / Tip**: Faithfulness = grounded in retrieved context.
- **Trap**: Confusing with answer relevance.

---
### MOCK1-011: CIDR Usable Hosts Calculation

**Correct Answer**: **B** (14)

#### Why
Host bits = $32 - 28 = 4$. Total addresses = $2^4 = 16$. Usable hosts = $2^4 - 2 = 14$ (subtracting network and broadcast).

- **5-Second Shortcut / Tip**: 2^(32-prefix) - 2.
- **Trap**: Forgetting to subtract 2.

---
### MOCK1-012: TCP 3-Way Handshake Flags

**Correct Answer**: **A** (SYN -> SYN-ACK -> ACK)

#### Why
Client sends SYN, server responds with SYN-ACK, client acknowledges with ACK.

- **5-Second Shortcut / Tip**: SYN -> SYN-ACK -> ACK.
- **Trap**: Confusing termination FIN with start SYN.

---
### MOCK1-013: SQL NULL Comparison with Equality

**Correct Answer**: **B** (Returns 0 rows)

#### Why
In SQL 3-valued logic, comparison with NULL using `=` evaluates to UNKNOWN, which WHERE treats as false. Must use `IS NULL`.

- **5-Second Shortcut / Tip**: Equality with NULL always fails; use IS NULL.
- **Trap**: Assuming bonus = NULL works.

---
### MOCK1-014: WHERE vs HAVING Filtering Order

**Correct Answer**: **B** (WHERE before, HAVING after)

#### Why
WHERE filters rows before aggregation; HAVING filters aggregated group metrics.

- **5-Second Shortcut / Tip**: WHERE = row filter, HAVING = group filter.
- **Trap**: Reversing execution order.

---
### MOCK1-015: Third Normal Form (3NF) Condition

**Correct Answer**: **B** (Transitive functional dependencies)

#### Why
3NF eliminates transitive dependencies ($X \to Y$ and $Y \to Z$ where $Z$ is non-prime).

- **5-Second Shortcut / Tip**: 3NF = No transitive dependencies.
- **Trap**: Confusing 2NF partial with 3NF transitive.

---
### MOCK1-016: ACID Isolation Anomaly: Phantom Read

**Correct Answer**: **D** (Serializable)

#### Why
Serializable isolation level prevents dirty reads, non-repeatable reads, and phantom reads completely.

- **5-Second Shortcut / Tip**: Serializable eliminates phantom reads.
- **Trap**: Assuming Repeatable Read prevents phantom reads.

---
### MOCK1-017: Virtual Function Dispatch in C++

**Correct Answer**: **B** (Via vtable (virtual method table) and vptr pointers)

#### Why
Compilers create a `vtable` per class with virtual methods, and each object instance holds a hidden `vptr`.

- **5-Second Shortcut / Tip**: Runtime dispatch = vtable + vptr.
- **Trap**: Confusing with compile-time overloading.

---
### MOCK1-018: Diamond Problem Resolution

**Correct Answer**: **B** (Using virtual base classes (`virtual public Base`))

#### Why
Virtual inheritance ensures only one copy of the common ancestor base class is inherited by the derived leaf.

- **5-Second Shortcut / Tip**: Virtual base class solves diamond problem.
- **Trap**: Thinking friend classes solve it.

---
### MOCK1-019: Deadlock Necessary Conditions

**Correct Answer**: **C** (Preemption Allowed)

#### Why
The condition is NO PREEMPTION. If preemption is allowed, resources can be forcibly reclaimed, breaking deadlock.

- **5-Second Shortcut / Tip**: No Preemption is the condition, not preemption.
- **Trap**: Overlooking the negative.

---
### MOCK1-020: Banker's Algorithm Data Structures

**Correct Answer**: **A** (Need = Max - Allocation)

#### Why
Need matrix is computed as `Need[i][j] = Max[i][j] - Allocation[i][j]`.

- **5-Second Shortcut / Tip**: Need = Max - Allocation.
- **Trap**: Adding matrices instead of subtracting.

---
### MOCK1-021: Thrashing Cause in Operating Systems

**Correct Answer**: **B** (A process spending more time paging in/out pages than executing instructions)

#### Why
Thrashing happens when the sum of working sets exceeds available physical frames, causing continuous page swapping.

- **5-Second Shortcut / Tip**: Thrashing = paging time > execution time.
- **Trap**: Confusing with CPU burst.

---
### MOCK1-022: Circular Queue Full Condition

**Correct Answer**: **B** ((rear + 1) % N == front)

#### Why
A circular queue of capacity $N-1$ is full when advancing `rear` by 1 wraps around to `front`: `(rear + 1) % N == front`.

- **5-Second Shortcut / Tip**: (rear + 1) % N == front.
- **Trap**: Assuming rear == N - 1 for circular queue.

---
### MOCK1-023: BST Inorder Traversal Property

**Correct Answer**: **B** (Nodes are visited in sorted non-decreasing order)

#### Why
Left subtree values < root < right subtree values guarantees sorted order during inorder traversal.

- **5-Second Shortcut / Tip**: BST Inorder = Sorted Order.
- **Trap**: Confusing inorder with postorder.

---
### MOCK1-024: Comparison-Based Sorting Lower Bound

**Correct Answer**: **B** (Omega(N log N))

#### Why
A comparison decision tree requires $N!$ leaves, so tree height is at least $\log_2(N!) = \Omega(N \log N)$.

- **5-Second Shortcut / Tip**: Decision tree height = Omega(N log N).
- **Trap**: Assuming linear time is possible for comparison sort.

---
### MOCK1-025: Hash Map Linear Probing Clustering

**Correct Answer**: **B** (Primary Clustering)

#### Why
Linear probing causes occupied slots to form long contiguous blocks (Primary Clustering), degrading lookup time.

- **5-Second Shortcut / Tip**: Linear probing = Primary clustering.
- **Trap**: Confusing with secondary clustering (quadratic probing).

---
### MOCK1-026: Negative Modulo Evaluation

**Correct Answer**: **B** (-3)

#### Why
`-11 = 4 * (-2) + (-3)`. In C/Java, remainder takes the sign of dividend: `-11 % 4 = -3`.

- **5-Second Shortcut / Tip**: Sign follows dividend: -11 % 4 = -3.
- **Trap**: Applying Python modulo (+1).

---
### MOCK1-027: Precedence of Bitwise Shifts vs Addition

**Correct Answer**: **A** (16)

#### Why
Addition `+` has higher precedence than left shift `<<`. `2 + 1 = 3`. Then `2 << 3 = 2 * 8 = 16`.

- **5-Second Shortcut / Tip**: + first: 2 + 1 = 3; then 2 << 3 = 16.
- **Trap**: Doing shift first: (2 << 2) + 1 = 9.

---
### MOCK1-028: Short-Circuit Logical AND Side Effect

**Correct Answer**: **A** (1 5)

#### Why
`x++` evaluates to `0` (false), short-circuiting the `&&` operator. `++y` never runs. `x` increments to `1`. `y` stays `5`.

- **5-Second Shortcut / Tip**: x is 0 -> short-circuit skips ++y. Output: 1 5.
- **Trap**: Executing ++y despite false left operand.

---
### MOCK1-029: Brian Kernighan Set Bit Clearing

**Correct Answer**: **B** (Clears the lowest set bit to 0)

#### Why
Subtracting 1 flips all trailing bits up to the lowest set bit; AND-ing with `x` zeroes that lowest bit.

- **5-Second Shortcut / Tip**: x & (x - 1) clears lowest set bit.
- **Trap**: Thinking it checks parity.

---
### MOCK1-030: Two's Complement NOT Identity

**Correct Answer**: **A** (-8)

#### Why
Identity: `~x = -(x + 1)`. For $x = 7$: `~7 = -(7 + 1) = -8`.

- **5-Second Shortcut / Tip**: ~x = -(x + 1) -> ~7 = -8.
- **Trap**: Guessing -7 or 8.

---
### MOCK1-031: Power of Two Bitwise Condition

**Correct Answer**: **A** ((n & (n - 1)) == 0)

#### Why
A power of 2 has exactly one set bit. Clearing it via `n & (n - 1)` leaves 0.

- **5-Second Shortcut / Tip**: Power of 2: n & (n - 1) == 0.
- **Trap**: Using n + 1 instead of n - 1.

---
### MOCK1-032: XOR Self-Cancellation Identity

**Correct Answer**: **B** (7)

#### Why
XOR is commutative: `(12 ^ 12) ^ 7 = 0 ^ 7 = 7`.

- **5-Second Shortcut / Tip**: Duplicates cancel: x ^ x = 0.
- **Trap**: Manual bit-by-bit calculation.

---
### MOCK1-033: Recursive Factorial Stack Tracing

**Correct Answer**: **A** (12)

#### Why
`calc(3) = 3 * calc(2) = 3 * (2 * calc(1)) = 3 * 2 * 2 = 12`.

- **5-Second Shortcut / Tip**: Notice base case returns 2, so 3 * 2 * 2 = 12.
- **Trap**: Assuming base case returns 1 (yielding 6).

---
### MOCK1-034: Nested Loop Geometric Series Counter

**Correct Answer**: **A** (15)

#### Why
`i` takes values `1, 2, 4, 8`. Inner loop adds `1 + 2 + 4 + 8 = 15`.

- **5-Second Shortcut / Tip**: Powers of 2 sum: 1 + 2 + 4 + 8 = 15.
- **Trap**: Calculating 2^4 = 16.

---
### MOCK1-035: Digital Root Modulo 9 Shortcut

**Correct Answer**: **A** (6)

#### Why
$4 + 8 + 7 + 5 = 24 \implies 2 + 4 = 6$. Alternatively, $1 + (4875 - 1) \% 9 = 1 + 4874 \% 9 = 1 + 5 = 6$.

- **5-Second Shortcut / Tip**: Sum digits repeatedly: 4+8+7+5=24 -> 2+4=6.
- **Trap**: Stopping at 24.

---
### MOCK1-036: Syllogism: Particular Affirmative Trap

**Correct Answer**: **A** (Only I follows)

#### Why
The managers who are engineers must be graduates (I follows). But 'Some are' never proves 'Some are not'. II does not follow.

- **5-Second Shortcut / Tip**: 'Some + All' gives 'Some'. Never assume 'Some are not'.
- **Trap**: Assuming II follows from I.

---
### MOCK1-037: Conditional Contrapositive (Modus Tollens)

**Correct Answer**: **A** (Server load did not exceed 90%)

#### Why
If P then Q; Not Q -> therefore Not P. Server load did not exceed 90%.

- **5-Second Shortcut / Tip**: Modus Tollens: not Q implies not P.
- **Trap**: Speculating that auto-scaling is broken.

---
### MOCK1-038: Behavioral: Delivering Realistic Trade-offs

**Correct Answer**: **C** (Inform the project lead proactively, present realistic scope trade-offs, and reprioritize with stakeholders.)

#### Why
Proactive transparency, risk management, and structured prioritization reflect enterprise professionalism.

- **5-Second Shortcut / Tip**: Early transparency > hidden overwork or cutting quality.
- **Trap**: Cutting tests to meet demo.

---
### MOCK1-039: Grammar: Subject-Verb Agreement with 'As Well As'

**Correct Answer**: **A** (was)

#### Why
Phrases like 'as well as' are parenthetical; the verb agrees with the primary subject 'The lead architect' (singular: 'was').

- **5-Second Shortcut / Tip**: Parenthetical 'as well as' does not alter singular subject.
- **Trap**: Matching verb to plural 'engineers'.

---
### MOCK1-040: Vocabulary: Professional Context Meaning

**Correct Answer**: **B** (Showing great attention to detail and thoroughness)

#### Why
'Meticulous' means showing great attention to detail; very careful and precise.

- **5-Second Shortcut / Tip**: Meticulous = Thorough and detailed.
- **Trap**: Confusing with complicated.

---
Previous: [01_mcq_mock_1.md](01_mcq_mock_1.md) | Next: [02_mcq_mock_2.md](02_mcq_mock_2.md)
