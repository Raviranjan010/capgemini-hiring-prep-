[Home](../README.md) > [09-mock-tests](README.md) > 01_mcq_mock_1.md

# Full-Length MCQ Mock Assessment 1

**Time Limit**: 40 minutes | **Total Questions**: 40 MCQs

---

### MOCK1-001: Vector Search Distance Metric for Normalized Embeddings

**Section**: AI Literacy | **Difficulty**: Easy

When text embeddings are normalized to unit length ($L_2$ norm = 1), which similarity metric is mathematically proportional to Dot Product?

- **A**: Cosine Similarity
- **B**: Manhattan Distance
- **C**: Levenshtein Distance
- **D**: Minkowski Distance

---

### MOCK1-002: Indirect Prompt Injection Source Vector

**Section**: AI Literacy | **Difficulty**: Medium

Which of the following scenarios represents an indirect prompt injection attack?

- **A**: A developer typing an adversarial prompt in the IDE console
- **B**: An automated web scraper feeding a malicious webpage containing hidden system instructions into an LLM summary agent
- **C**: A user repeatedly prompting 'Ignore previous instructions'
- **D**: A database administrator querying SQL tables

---

### MOCK1-003: Low Temperature for Code Generation

**Section**: AI Literacy | **Difficulty**: Easy

Why is a low temperature ($T \le 0.2$) recommended when using an LLM to generate SQL or parse JSON?

- **A**: It increases creative hallucination.
- **B**: It enforces deterministic token selection by concentrating probability mass on top logits.
- **C**: It expands the context window size.
- **D**: It speeds up network transit time.

---

### MOCK1-004: RAG Chunking Semantic Loss

**Section**: AI Literacy | **Difficulty**: Medium

In a RAG pipeline, what risk arises when chunk size is set too small (e.g. 30 tokens)?

- **A**: Excessive GPU memory consumption during retrieval
- **B**: Loss of sentence-level context and semantic fragmentation
- **C**: Failure of vector indexing algorithms
- **D**: Increased risk of direct prompt injection

---

### MOCK1-005: Hallucination vs Knowledge Cutoff

**Section**: AI Literacy | **Difficulty**: Medium

When an LLM confidently cites a nonexistent scientific paper with fabricated author names, this is an example of:

- **A**: Data Drift
- **B**: Hallucination
- **C**: Concept Shift
- **D**: Overfitting

---

### MOCK1-006: Chain-of-Thought Prompting Advantage

**Section**: AI Literacy | **Difficulty**: Easy

How does Chain-of-Thought (CoT) prompting improve LLM performance on multi-step reasoning tasks?

- **A**: It compresses the context window.
- **B**: It forces the model to generate intermediate reasoning tokens before reaching a final answer.
- **C**: It eliminates the need for vector embeddings.
- **D**: It fine-tunes model weights during runtime.

---

### MOCK1-007: BM25 Hybrid Search Necessity

**Section**: AI Literacy | **Difficulty**: Medium

Why do enterprise RAG systems combine BM25 sparse keyword search with dense vector embeddings?

- **A**: Dense embeddings fail to retrieve exact alphanumeric identifiers and error codes.
- **B**: BM25 uses deep neural transformers.
- **C**: Vector databases cannot store text strings.
- **D**: BM25 reduces context window usage.

---

### MOCK1-008: PII Exposure in LLM Prompts

**Section**: AI Literacy | **Difficulty**: Easy

Under data privacy regulations (GDPR, DPDP), what must be done before transmitting client logs containing customer email addresses to an external LLM API?

- **A**: Compress the logs using Gzip
- **B**: Apply automated PII masking or tokenized pseudonymization
- **C**: Set model temperature to 1.0
- **D**: Use few-shot examples

---

### MOCK1-009: RLHF Reward Model Objective

**Section**: AI Literacy | **Difficulty**: Medium

During Reinforcement Learning from Human Feedback (RLHF), what is the function of the reward model?

- **A**: To tokenize incoming text streams
- **B**: To score candidate completions based on human preference data (helpfulness, honesty, safety)
- **C**: To re-rank vector database retrieval results
- **D**: To perform integer quantization on weights

---

### MOCK1-010: RAGAS Faithfulness Metric

**Section**: AI Literacy | **Difficulty**: Hard

In the RAGAS evaluation framework, what does the 'Faithfulness' metric measure?

- **A**: How fast the vector database retrieves top-k chunks
- **B**: The ratio of claims in the generated answer that can be directly inferred from the retrieved context
- **C**: The grammatical correctness of user prompts
- **D**: The token cost of the generation

---

### MOCK1-011: CIDR Usable Hosts Calculation

**Section**: Networking | **Difficulty**: Easy

How many usable host IP addresses are available in a subnet with CIDR `/28`?

- **A**: 16
- **B**: 14
- **C**: 30
- **D**: 12

---

### MOCK1-012: TCP 3-Way Handshake Flags

**Section**: Networking | **Difficulty**: Easy

What is the exact sequence of TCP flag packets exchanged during connection establishment?

- **A**: SYN -> SYN-ACK -> ACK
- **B**: ACK -> SYN -> SYN-ACK
- **C**: SYN -> ACK -> DATA
- **D**: FIN -> ACK -> FIN-ACK

---

### MOCK1-013: SQL NULL Comparison with Equality

**Section**: SQL | **Difficulty**: Easy

What is the result of evaluating `SELECT * FROM Employees WHERE bonus = NULL;` in standard SQL?

- **A**: Returns all rows where bonus is NULL
- **B**: Returns 0 rows
- **C**: Throws a syntax error
- **D**: Returns all rows where bonus is 0

---

### MOCK1-014: WHERE vs HAVING Filtering Order

**Section**: SQL | **Difficulty**: Easy

In SQL query execution, which clause filters individual rows *before* aggregation, and which filters *after* `GROUP BY`?

- **A**: HAVING before, WHERE after
- **B**: WHERE before, HAVING after
- **C**: Both filter before
- **D**: Both filter after

---

### MOCK1-015: Third Normal Form (3NF) Condition

**Section**: DBMS | **Difficulty**: Medium

A relational table is in 3NF if it is in 2NF and has no:

- **A**: Partial functional dependencies
- **B**: Transitive functional dependencies
- **C**: Foreign keys
- **D**: Composite primary keys

---

### MOCK1-016: ACID Isolation Anomaly: Phantom Read

**Section**: DBMS | **Difficulty**: Hard

Under which ANSI SQL transaction isolation level are Phantom Reads completely prevented?

- **A**: Read Uncommitted
- **B**: Read Committed
- **C**: Repeatable Read
- **D**: Serializable

---

### MOCK1-017: Virtual Function Dispatch in C++

**Section**: OOPs | **Difficulty**: Medium

In C++, how is runtime polymorphism (dynamic binding) implemented internally by the compiler?

- **A**: Via function overloading tables
- **B**: Via vtable (virtual method table) and vptr pointers
- **C**: Via template metaprogramming
- **D**: Via stack frame unwinding

---

### MOCK1-018: Diamond Problem Resolution

**Section**: OOPs | **Difficulty**: Easy

In C++, how is the diamond problem in multiple inheritance resolved so that the base class is instantiated only once?

- **A**: Using friend classes
- **B**: Using virtual base classes (`virtual public Base`)
- **C**: Using private inheritance
- **D**: Using static methods

---

### MOCK1-019: Deadlock Necessary Conditions

**Section**: OS | **Difficulty**: Easy

Which of the following is NOT one of Coffman's four necessary conditions for deadlock?

- **A**: Mutual Exclusion
- **B**: Hold and Wait
- **C**: Preemption Allowed
- **D**: Circular Wait

---

### MOCK1-020: Banker's Algorithm Data Structures

**Section**: OS | **Difficulty**: Medium

In Dijkstra's Banker's Algorithm for deadlock avoidance, which formula computes the Need matrix?

- **A**: Need = Max - Allocation
- **B**: Need = Allocation + Available
- **C**: Need = Max + Available
- **D**: Need = Allocation - Max

---

### MOCK1-021: Thrashing Cause in Operating Systems

**Section**: OS | **Difficulty**: Easy

What causes thrashing in a virtual memory system?

- **A**: High CPU burst times
- **B**: A process spending more time paging in/out pages than executing instructions
- **C**: Deadlock between two processes
- **D**: Too few page faults occurring

---

### MOCK1-022: Circular Queue Full Condition

**Section**: DSA Theory | **Difficulty**: Easy

In an array of size $N$ implementing a circular queue with `front` and `rear`, what is the queue full condition?

- **A**: rear == front
- **B**: (rear + 1) % N == front
- **C**: rear == N - 1
- **D**: (front + 1) % N == rear

---

### MOCK1-023: BST Inorder Traversal Property

**Section**: DSA Theory | **Difficulty**: Easy

What property is always true for the inorder traversal of a Binary Search Tree (BST)?

- **A**: Nodes are visited in strictly descending order
- **B**: Nodes are visited in sorted non-decreasing order
- **C**: Leaf nodes appear first
- **D**: Root node appears last

---

### MOCK1-024: Comparison-Based Sorting Lower Bound

**Section**: DSA Theory | **Difficulty**: Medium

What is the mathematical worst-case lower bound for any comparison-based sorting algorithm on $N$ elements?

- **A**: Omega(N)
- **B**: Omega(N log N)
- **C**: Omega(N^2)
- **D**: Omega(1)

---

### MOCK1-025: Hash Map Linear Probing Clustering

**Section**: DSA Theory | **Difficulty**: Medium

In open addressing hash tables, what performance drawback is caused by linear probing?

- **A**: Secondary Clustering
- **B**: Primary Clustering
- **C**: Segmentation Faults
- **D**: Infinite Re-hashing

---

### MOCK1-026: Negative Modulo Evaluation

**Section**: Pseudocode | **Difficulty**: Easy

What is the output of the following pseudocode in standard C/Java?
```text
Integer res = (-11) % 4
Print res
```

- **A**: 1
- **B**: -3
- **C**: -1
- **D**: 3

---

### MOCK1-027: Precedence of Bitwise Shifts vs Addition

**Section**: Pseudocode | **Difficulty**: Medium

What is the printed value?
```text
Integer a = 2
Integer val = a << 2 + 1
Print val
```

- **A**: 16
- **B**: 9
- **C**: 12
- **D**: 8

---

### MOCK1-028: Short-Circuit Logical AND Side Effect

**Section**: Pseudocode | **Difficulty**: Medium

Trace the code below:
```c
int x = 0, y = 5;
if (x++ && ++y) {
    y += 10;
}
printf("%d %d", x, y);
```

- **A**: 1 5
- **B**: 1 6
- **C**: 0 5
- **D**: 1 16

---

### MOCK1-029: Brian Kernighan Set Bit Clearing

**Section**: Bitwise | **Difficulty**: Easy

What does the operation `x & (x - 1)` accomplish in binary arithmetic?

- **A**: Sets the highest bit to 1
- **B**: Clears the lowest set bit to 0
- **C**: Inverts all bits
- **D**: Multiplies x by 2

---

### MOCK1-030: Two's Complement NOT Identity

**Section**: Bitwise | **Difficulty**: Easy

What is the decimal value of `~7` in signed two's complement integer representation?

- **A**: -8
- **B**: -7
- **C**: 8
- **D**: 0

---

### MOCK1-031: Power of Two Bitwise Condition

**Section**: Bitwise | **Difficulty**: Easy

Which expression returns true if and only if positive integer $n$ is a power of 2?

- **A**: (n & (n - 1)) == 0
- **B**: (n | (n - 1)) == 0
- **C**: (n & (n + 1)) == 0
- **D**: (n ^ (n - 1)) == 0

---

### MOCK1-032: XOR Self-Cancellation Identity

**Section**: Bitwise | **Difficulty**: Easy

Evaluate: `12 ^ 7 ^ 12`

- **A**: 0
- **B**: 7
- **C**: 12
- **D**: 19

---

### MOCK1-033: Recursive Factorial Stack Tracing

**Section**: Pseudocode | **Difficulty**: Easy

What is returned by `calc(3)`?
```text
function calc(n):
    if n <= 1 then return 2
    return n * calc(n - 1)
```

- **A**: 12
- **B**: 6
- **C**: 24
- **D**: 8

---

### MOCK1-034: Nested Loop Geometric Series Counter

**Section**: Pseudocode | **Difficulty**: Medium

What is printed by this pseudocode?
```text
Integer count = 0
For i = 1 to 8 step i = i * 2:
    For j = 1 to i:
        count = count + 1
Print count
```

- **A**: 15
- **B**: 16
- **C**: 8
- **D**: 31

---

### MOCK1-035: Digital Root Modulo 9 Shortcut

**Section**: Pseudocode | **Difficulty**: Medium

What is the single-digit repeated digital root of 4875?

- **A**: 6
- **B**: 3
- **C**: 9
- **D**: 1

---

### MOCK1-036: Syllogism: Particular Affirmative Trap

**Section**: Deductive Reasoning | **Difficulty**: Medium

Statements: 1. Some managers are engineers. 2. All engineers are graduates.
Conclusions:
I. Some managers are graduates.
II. Some managers are not graduates.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both I and II follow
- **D**: Neither follows

---

### MOCK1-037: Conditional Contrapositive (Modus Tollens)

**Section**: Deductive Reasoning | **Difficulty**: Easy

Premises: 1. If server load exceeds 90%, auto-scaling triggers. 2. Auto-scaling did not trigger.
Conclusion:

- **A**: Server load did not exceed 90%
- **B**: Server load was exactly 90%
- **C**: Auto-scaling is broken
- **D**: Server is offline

---

### MOCK1-038: Behavioral: Delivering Realistic Trade-offs

**Section**: Behavioral Scenarios | **Difficulty**: Medium

You realize an assigned sprint deliverable cannot be completed on time with full unit testing. What is the safest course of action?

- **A**: Skip writing tests and submit the feature quietly.
- **B**: Work hidden overtime without alerting leads.
- **C**: Inform the project lead proactively, present realistic scope trade-offs, and reprioritize with stakeholders.
- **D**: Wait until the sprint demo to announce the delay.

---

### MOCK1-039: Grammar: Subject-Verb Agreement with 'As Well As'

**Section**: English Communication | **Difficulty**: Easy

Choose the grammatically correct option:
'The lead architect, as well as the database engineers, ________ present at the review.'

- **A**: was
- **B**: were
- **C**: are
- **D**: have been

---

### MOCK1-040: Vocabulary: Professional Context Meaning

**Section**: English Communication | **Difficulty**: Easy

What does the word **METICULOUS** mean in a software quality review context?

- **A**: Hasty and superficial
- **B**: Showing great attention to detail and thoroughness
- **C**: Overly complicated
- **D**: Theoretical and unverified

---

Previous: [README.md](README.md) | Next: [01_mcq_mock_1_solutions.md](01_mcq_mock_1_solutions.md)
