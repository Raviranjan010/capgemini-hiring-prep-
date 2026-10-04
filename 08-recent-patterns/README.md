[Home](../README.md) > [Recent Patterns](README.md)

# Capgemini Recent Assessment Patterns

This folder indexes the patterns reported across candidate exam debriefs, videos, and campus placement reports. Use this matrix to identify high-yield problem archetypes and practice close variants so you learn the structural logic rather than memorizing a single question.

> **Important**: Pattern practice items marked `[PATTERN]` are newly designed practice questions modelled on reported patterns, not actual past exam questions. Real exam questions will differ.

---

## Master Reported Patterns Table

| Pattern Archetype | Assessment Section | Where Reported | Existing Questions / IDs | Key Structural Mechanics |
| :--- | :--- | :--- | :--- | :--- |
| **RAG Retrieval Failure & Lost-in-the-Middle** | AI Literacy | Gemini Chat | `AI-005`, `AI-020`, `AI-040` | Chunking split across boundaries, sliding window overlap loss, context degradation at middle positions. |
| **Hybrid Keyword + Dense Vector Search** | AI Literacy | Gemini Chat | `AI-021`, `AI-032`, `AI-033` | Dense embeddings fail on exact alphanumeric SKU/error codes; BM25 hybrid indexing resolves exact match. |
| **Direct & Indirect Prompt Injection** | AI Literacy | KN Academy Video & Chat | `AI-006`, `AI-023`, `AI-034`, `AI-041` | System prompt override via untrusted external payload, delimiter escaping, and multi-turn jailbreaking. |
| **Hallucination vs Data Poisoning vs Training Drift** | AI Literacy | KN Academy Video & Chat | `AI-002`, `AI-011`, `AI-024`, `AI-036` | Confident probabilistic fabrication vs adversarial corpus manipulation vs temporal cutoff limitations. |
| **Temperature, Top-p & Guided Structured Decoding** | AI Literacy | Gemini Chat | `AI-019`, `AI-030`, `AI-039` | Low temperature / greedy argmax for deterministic JSON/SQL grammar; high temperature for creative synthesis. |
| **RAGAS Evaluation & Faithfulness Metrics** | AI Literacy | Gemini Chat | `AI-035` | Groundedness in retrieved chunks vs answer relevance to user query. |
| **PII Redaction & Enterprise Governance** | AI Literacy | KN Academy Video | `AI-004`, `AI-038` | Hardcoded database credentials in generated code; regulatory masking under GDPR/DPDP. |
| **Negative Modulo & Integer Division Truncation** | Pseudocode & Bitwise | Campus Reports & Chat | `PSE-005`, `PSE-013`, `PSE-018` | `-13 % 5` or bitwise precedence where `%` has higher precedence than `+` or bitwise shifts. |
| **Short-Circuit Logical Operator Evaluation** | Pseudocode & Bitwise | KN Academy Video | `PSE-009`, `PSE-010` | Second operand skipped if first operand satisfies `AND` (false) or `OR` (true); side effects not executed. |
| **Brian Kernighan & Bitwise Masking Tricks** | Pseudocode & Bitwise | Gemini Chat | `BIT-001`, `BIT-002`, `BIT-006` | `n & (n - 1)` clears lowest set bit; `x ^ y` identifies mismatched bits. |
| **Nested Loop & Variable Scope Mutation** | Pseudocode & Bitwise | Campus Reports | `PSE-012`, `PSE-014` | Inner loop modifies loop variable of outer loop or accesses shadowed outer variable. |
| **Off-by-One Loop Bounds & Null Pointers** | Code Debugging | KN Academy Video | `DBG-001`, `DBG-004`, `DBG-007` | Iterating `<= length` instead of `< length`; referencing node before null check. |
| **Premature Return in Search / Traversal** | Code Debugging | KN Academy Video | `DBG-002`, `DBG-005` | Returning from loop on first non-match instead of completing scan. |
| **One-Way Directed Edge Representation Bug** | Code Debugging | Gemini Chat | `DBG-013`, `DBG-015` | Adding edge `u -> v` but forgetting `v -> u` in undirected graph or vice-versa. |
| **0/1 Knapsack Space Optimization Direction** | Code Debugging | KN Academy Video & Chat | `DBG-014` | Iterating capacity forward ($0 \to W$) reuses current item infinitely (unbounded); must iterate backwards ($W \to w$). |
| **Binary Search Midpoint Overflow & Stagnant Bounds** | Code Debugging | KN Academy Video | `DBG-018`, `DBG-019` | `mid = (low + high) / 2` integer overflow; setting `low = mid` causing infinite loop when `low + 1 == high`. |
| **Negative Modulo Normalization in Prefix Sum** | AI-Assisted Coding | Gemini Chat | `AIC-003`, `AIC-008`, `AIC-010` | `rem = (sum % k + k) % k` required in C++/Java when negative sums exist. |
| **Grid BFS with Obstacle Elimination Quota** | AI-Assisted Coding | Gemini Chat | `AIC-019` | State tracking requires 3D state `(r, c, obstacles_remaining)` rather than simple 2D visited matrix. |
| **Binary Tree Boundary Traversal Tripartite Logic** | AI-Assisted Coding | Gemini Chat | `AIC-020` | Separate left boundary, leaves, and bottom-up right boundary; avoid duplicate inclusion of root and leaves. |
| **Decode Ways Intermediate Zero Trap** | AI-Assisted Coding | KN Academy Video | `AIC-014`, `AIC-015` | Single digit `'0'` is invalid; two digits with leading `'0'` like `'06'` cannot form valid letter. |
| **Harmonic Subarray Sliding Window Condition** | AI-Assisted Coding | KN Academy Video | `AIC-001` | Shrinking left pointer while window condition invalid; tracking frequencies with hash map. |
| **Multi-Source BFS / Kahn's Topological Cycle** | AI-Assisted Coding | KN Academy Video & Chat | `AIC-011`, `AIC-012`, `AIC-013` | Indegree zero queue; if visited count `< total nodes`, directed cycle exists. |
| **Switch Challenge Composite Operator Deduction** | Cognitive Games | KN Academy Video | `COG-018` to `COG-032` | Tracing multi-layer permutations of positions 1-4; comparing input and output digits to eliminate operators. |
| **Motion Challenge Minimum Ice Path Obstacle** | Cognitive Games | KN Academy Video | `COG-001` to `COG-004` | Sliding blocks on friction-free grid; moving secondary blocks to create backstops. |
| **Grid & Digit Challenge BODMAS Balancing** | Cognitive Games | KN Academy Video | `COG-008` to `COG-014` | Placing digits 1-9 to satisfy row/column equations; checking multiplication/division priority. |
| **Deductive Syllogism Conversion Traps** | Cognitive Games | KN Academy Video | `COG-015` to `COG-017` | "All A are B" does NOT imply "All B are A"; "Some A are B" does NOT imply "Some A are not B". |
| **Behavioral Forced-Choice Priority Alignment** | Behavioral Profiling | KN Academy Video | `BEH-001` to `BEH-003` | Delivery SLAs > unchecked innovation; team consensus > lone wolf autonomy; consistency tracking engine. |

---

## Pattern Detail Files

- [01. AI Literacy Patterns](01_ai_literacy.md)
- [02. CS and Pseudocode Patterns](02_cs_and_pseudocode.md)
- [03. Debugging Patterns](03_debugging.md)
- [04. AI-Assisted Coding Patterns](04_ai_assisted_coding.md)
- [05. Cognitive and Behavioral Patterns](05_cognitive.md)

---

Previous: [07-communication](../07-communication/README.md) | Next: [09-mock-tests](../09-mock-tests/README.md)
