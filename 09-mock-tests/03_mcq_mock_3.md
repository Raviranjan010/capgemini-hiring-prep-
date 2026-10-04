[Home](../README.md) > [09-mock-tests](README.md) > 03_mcq_mock_3.md

# Full-Length MCQ Mock Assessment 3

**Time Limit**: 40 minutes | **Total Questions**: 40 MCQs

---

### MOCK3-001: Tokenization Subword Splitting (BPE)

**Section**: AI Literacy | **Difficulty**: Easy

Why do modern LLMs use subword tokenization (such as Byte-Pair Encoding) instead of whole-word vocabularies?

- **A**: To handle out-of-vocabulary words and morphologically rich languages within a fixed vocabulary size
- **B**: To eliminate the need for GPU computation
- **C**: To convert text directly to audio
- **D**: To avoid using embedding layers

---

### MOCK3-002: Cross-Encoder vs Bi-Encoder in RAG Re-ranking

**Section**: AI Literacy | **Difficulty**: Hard

Why are Cross-Encoders applied during RAG re-ranking rather than initial vector retrieval across millions of documents?

- **A**: Cross-encoders are too fast to use indices.
- **B**: Cross-encoders jointly process query and document via full cross-attention, which is highly accurate but too computationally expensive for million-scale search.
- **C**: Cross-encoders only work on French texts.
- **D**: Bi-encoders cannot compute cosine similarity.

---

### MOCK3-003: Self-Consistency Prompting Mechanics

**Section**: AI Literacy | **Difficulty**: Medium

How does the Self-Consistency decoding strategy improve mathematical reasoning accuracy?

- **A**: By setting temperature to 0 and greedy decoding once
- **B**: By sampling multiple distinct reasoning paths at non-zero temperature and taking the majority-voted answer
- **C**: By fine-tuning the model on calculator code
- **D**: By deleting prompt stop tokens

---

### MOCK3-004: Data Contamination in Benchmark Evals

**Section**: AI Literacy | **Difficulty**: Medium

What is benchmark data contamination in LLM evaluation?

- **A**: A database corruption during SQL export
- **B**: Test evaluation sets leaking into pre-training or fine-tuning datasets, causing inflated benchmark scores
- **C**: Hardware thermal throttling during inference
- **D**: Users submitting invalid prompts

---

### MOCK3-005: Context Window Truncation Degradation

**Section**: AI Literacy | **Difficulty**: Medium

When an input document exceeds the model's maximum context length and is truncated silently, what failure mode is observed?

- **A**: The model generates random characters.
- **B**: The model answers based only on remaining partial text, missing critical conditions located in the truncated portion.
- **C**: The model automatically doubles its GPU memory.
- **D**: The API returns HTTP 200 with an empty string.

---

### MOCK3-006: Responsible AI: Algorithmic Bias in Hiring Tools

**Section**: AI Literacy | **Difficulty**: Easy

If an AI resume-screening model trained on historical data systematically penalizes women's colleges, what type of bias is manifested?

- **A**: Historical / Training Data Bias
- **B**: Quantization Error
- **C**: Subnet Allocation Fault
- **D**: Network Latency Bias

---

### MOCK3-007: Prompt Chaining vs Single Monolithic Prompt

**Section**: AI Literacy | **Difficulty**: Medium

Why is prompt chaining (breaking a task into modular sub-prompts) preferred for complex pipelines?

- **A**: It consumes zero tokens.
- **B**: It isolates failure points, allows validation of intermediate outputs, and reduces reasoning load per step.
- **C**: It removes the need for APIs.
- **D**: It guarantees 100% latency reduction.

---

### MOCK3-008: Hardcoded API Keys in AI Code

**Section**: AI Literacy | **Difficulty**: Easy

What critical security violation occurs when a developer includes `openai_api_key = 'sk-...'` directly inside a public Git repository?

- **A**: Secret Credential Exposure Vulnerability
- **B**: SQL Injection
- **C**: Cross-Site Scripting (XSS)
- **D**: Buffer Overflow

---

### MOCK3-009: Embedding Dimensionality Trade-off

**Section**: AI Literacy | **Difficulty**: Medium

What is the trade-off when selecting high-dimensional embeddings (e.g. 3072 dims) vs smaller embeddings (e.g. 512 dims)?

- **A**: Higher dimensions capture richer semantic nuance but increase vector index storage and retrieval latency.
- **B**: Higher dimensions cannot be compared with cosine similarity.
- **C**: Smaller embeddings are always more accurate.
- **D**: Smaller embeddings use more RAM.

---

### MOCK3-010: NeMo Guardrails Input Rail Function

**Section**: AI Literacy | **Difficulty**: Medium

In an enterprise LLM architecture, what is the role of an Input Guardrail?

- **A**: To compress incoming network packets
- **B**: To intercept, sanitize, and validate user prompts before they reach the foundation model, blocking jailbreaks and off-topic queries
- **C**: To write code directly into the database
- **D**: To restart the operating system

---

### MOCK3-011: DNS Resolution Record Types

**Section**: Networking | **Difficulty**: Easy

Which DNS record maps a domain name directly to an IPv4 address?

- **A**: AAAA record
- **B**: A record
- **C**: CNAME record
- **D**: MX record

---

### MOCK3-012: HTTP 401 vs 403 Status Codes

**Section**: Networking | **Difficulty**: Easy

What is the difference between HTTP Status Code 401 and 403?

- **A**: 401 means Unauthorized (missing/invalid credentials), while 403 means Forbidden (authenticated, but lacking permissions).
- **B**: 401 is server error; 403 is client error.
- **C**: 401 is page not found; 403 is redirected.
- **D**: They are identical in all specifications.

---

### MOCK3-013: SQL Window Function: ROW_NUMBER vs DENSE_RANK

**Section**: SQL | **Difficulty**: Medium

When assigning ranks to duplicate values (e.g. scores `100, 100, 90`), what does `DENSE_RANK()` produce?

- **A**: 1, 2, 3
- **B**: 1, 1, 2
- **C**: 1, 1, 3
- **D**: 1, 2, 2

---

### MOCK3-014: SQL Correlated Subquery Mechanics

**Section**: SQL | **Difficulty**: Hard

What defines a correlated subquery in SQL?

- **A**: A subquery that executes once before the outer query
- **B**: A subquery that references column values from the outer query and must be evaluated for each row processed by the outer query
- **C**: A subquery containing a JOIN clause
- **D**: A subquery that only returns strings

---

### MOCK3-015: BCNF vs 3NF Strictness

**Section**: DBMS | **Difficulty**: Medium

What additional condition does Boyce-Codd Normal Form (BCNF) enforce compared to 3NF?

- **A**: For every functional dependency X -> Y, X must be a superkey.
- **B**: Non-prime attributes can be partially dependent on a composite key.
- **C**: Tables cannot contain foreign keys.
- **D**: Tables must have at most 3 columns.

---

### MOCK3-016: Database Index: B+ Tree vs Hash Index

**Section**: DBMS | **Difficulty**: Easy

Why are B+ Trees preferred over Hash Indexes as the default indexing structure in relational databases?

- **A**: B+ Trees support efficient range queries (`BETWEEN`, `<`, `>`) and ordered scans.
- **B**: Hash indexes consume less CPU.
- **C**: B+ Trees only work with strings.
- **D**: Hash indexes cannot handle integers.

---

### MOCK3-017: Virtual Destructor Importance

**Section**: OOPs | **Difficulty**: Medium

Why should a base class containing virtual functions always declare a virtual destructor (`virtual ~Base();`)?

- **A**: To speed up object construction
- **B**: To ensure the derived class destructor is invoked when deleting a derived object through a base class pointer, preventing memory leaks
- **C**: To make the destructor private
- **D**: To prevent class inheritance

---

### MOCK3-018: C++ Copy Constructor Signature

**Section**: OOPs | **Difficulty**: Easy

What is the standard correct parameter signature of a copy constructor in C++?

- **A**: ClassName(ClassName other)
- **B**: ClassName(const ClassName& other)
- **C**: ClassName(ClassName* other)
- **D**: ClassName(int size)

---

### MOCK3-019: Semaphore vs Mutex Key Difference

**Section**: OS | **Difficulty**: Medium

What is the fundamental architectural difference between a Mutex and a Binary Semaphore?

- **A**: A Mutex has ownership (only the locking thread can unlock it), whereas a Binary Semaphore can be signaled by any thread.
- **B**: Semaphores cannot be used in multi-threaded programs.
- **C**: Mutexes use busy waiting while semaphores do not.
- **D**: They are completely identical in implementation.

---

### MOCK3-020: Paging vs Segmentation Memory Management

**Section**: OS | **Difficulty**: Easy

What is the key structural distinction between Paging and Segmentation?

- **A**: Paging uses fixed-size memory blocks; Segmentation divides memory into variable-sized logical modules (functions, arrays).
- **B**: Paging causes external fragmentation while segmentation does not.
- **C**: Paging is visible to programmers while segmentation is hardware transparent.
- **D**: Segmentation only works on 32-bit CPUs.

---

### MOCK3-021: Process vs Thread Resource Sharing

**Section**: OS | **Difficulty**: Easy

Which resource is shared among all threads belonging to the same process?

- **A**: Thread Stack
- **B**: Program Counter (PC)
- **C**: Heap memory and Global variables
- **D**: CPU Register Set

---

### MOCK3-022: Circular Linked List Tail Insertion

**Section**: DSA Theory | **Difficulty**: Medium

In a circular singly linked list maintained with only a `tail` pointer, what is the time complexity to insert a new node at the front?

- **A**: O(1)
- **B**: O(N)
- **C**: O(log N)
- **D**: O(N^2)

---

### MOCK3-023: Dijkstra's Algorithm Negative Weights Failure

**Section**: DSA Theory | **Difficulty**: Easy

Why does Dijkstra's algorithm fail on graphs containing negative edge weights?

- **A**: It crashes with a divide-by-zero error.
- **B**: Its greedy assumption (that the shortest distance to a finalized vertex cannot be reduced further) is violated by negative edges.
- **C**: It cannot build adjacency lists.
- **D**: Priority queues reject negative numbers.

---

### MOCK3-024: Graph Bipartite Property

**Section**: DSA Theory | **Difficulty**: Easy

A graph is bipartite if and only if it contains no:

- **A**: Even-length cycles
- **B**: Odd-length cycles
- **C**: Connected components
- **D**: Directed edges

---

### MOCK3-025: Space Complexity of Recursive Fibonacci

**Section**: DSA Theory | **Difficulty**: Easy

What is the auxiliary space complexity (call stack depth) of naive recursive Fibonacci `fib(N)`?

- **A**: O(1)
- **B**: O(N)
- **C**: O(2^N)
- **D**: O(N log N)

---

### MOCK3-026: Operator Precedence: Shift vs Relational

**Section**: Pseudocode | **Difficulty**: Medium

What is printed?
```text
Integer a = 8
Integer res = a >> 1 < 5
Print res
```

- **A**: 1 (True)
- **B**: 0 (False)
- **C**: 4
- **D**: Syntax error

---

### MOCK3-027: Negative Integer Modulo Rule

**Section**: Pseudocode | **Difficulty**: Easy

What is the output of `(-20) % 6` in C/Java?

- **A**: -2
- **B**: 4
- **C**: 2
- **D**: -4

---

### MOCK3-028: Short-Circuit with Logical AND Cascade

**Section**: Pseudocode | **Difficulty**: Medium

Trace the final value of `c`:
```c
int a = 0, b = 1, c = 0;
if (a && ++c) {
    c += 5;
} else if (b && ++c) {
    c += 10;
}
printf("%d", c);
```

- **A**: 11
- **B**: 1
- **C**: 10
- **D**: 12

---

### MOCK3-029: Toggling K-th Bit using XOR

**Section**: Bitwise | **Difficulty**: Easy

Which operation flips bit 2 (representing $2^2 = 4$) of integer $x$?

- **A**: x = x ^ (1 << 2)
- **B**: x = x | (1 << 2)
- **C**: x = x & ~(1 << 2)
- **D**: x = x >> 2

---

### MOCK3-030: XOR Inversion for Parity Check

**Section**: Bitwise | **Difficulty**: Medium

What does `(x ^ (x >> 1)) & 1` evaluate?

- **A**: Whether the two lowest bits of x differ
- **B**: Whether x is negative
- **C**: The square of x
- **D**: Always 0

---

### MOCK3-031: Fast Integer Multiplication via Shifts

**Section**: Bitwise | **Difficulty**: Easy

Which expression computes $9 \times x$ using shifts and addition?

- **A**: (x << 3) + x
- **B**: (x << 3) - x
- **C**: (x << 4) + x
- **D**: (x << 2) + x

---

### MOCK3-032: Nested Loop Quadratic Summation

**Section**: Pseudocode | **Difficulty**: Medium

What is printed?
```text
Integer count = 0
For i = 1 to 4:
    For j = i to 4:
        count = count + 1
Print count
```

- **A**: 10
- **B**: 16
- **C**: 8
- **D**: 12

---

### MOCK3-033: Recursive Head and Tail Print Order

**Section**: Pseudocode | **Difficulty**: Medium

What sequence is printed by `f(2)`?
```text
function f(n):
    if n <= 0 then return
    print n
    f(n - 1)
    print n
```

- **A**: 2 1 1 2
- **B**: 2 1 2 1
- **C**: 1 2 2 1
- **D**: 2 1

---

### MOCK3-034: Binary Search Invariant Check

**Section**: Pseudocode | **Difficulty**: Easy

In binary search for target in sorted array, why is `mid = low + (high - low) / 2` preferred over `mid = (low + high) / 2`?

- **A**: It executes in $O(1)$ while the other is $O(N)$
- **B**: It prevents signed 32-bit integer overflow when `low + high` exceeds $2^{31} - 1$
- **C**: It works on unsorted arrays
- **D**: It avoids division by zero

---

### MOCK3-035: Bitwise NOT Evaluation on Negative

**Section**: Bitwise | **Difficulty**: Easy

What is the decimal value of `~(-10)` in two's complement?

- **A**: 9
- **B**: -11
- **C**: 10
- **D**: -9

---

### MOCK3-036: Syllogism: 'Only' Conversion Rule

**Section**: Deductive Reasoning | **Difficulty**: Medium

Statements: 1. Only professionals are members. 2. Some members are athletes.
Conclusions:
I. All members are professionals.
II. Some athletes are professionals.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

---

### MOCK3-037: Fallacy of Denying the Antecedent

**Section**: Deductive Reasoning | **Difficulty**: Medium

Premises: 1. If an employee completes the security training, they receive a certificate. 2. Maya did not complete the training.
Conclusion:

- **A**: Maya will not receive a certificate
- **B**: Maya may or may not receive a certificate (e.g. through exemption)
- **C**: Maya is terminated
- **D**: Training is mandatory

---

### MOCK3-038: Behavioral: Autonomous Troubleshooting Timebox

**Section**: Behavioral Scenarios | **Difficulty**: Medium

You encounter an obscure build error that is blocking your feature. What is the recommended balance between autonomy and team escalation?

- **A**: Struggle alone for 3 days to prove independence.
- **B**: Immediately message the lead without reading error logs.
- **C**: Timebox independent research (e.g. 1-2 hours) with log analysis, then escalate with documented reproduction steps.
- **D**: Abandon the feature and pick another ticket.

---

### MOCK3-039: Sentence Correction: Parallelism in Technical Writing

**Section**: English Communication | **Difficulty**: Easy

Which sentence maintains grammatical parallelism?

- **A**: The microservice is designed for scalability, reliability, and to perform fast.
- **B**: The microservice is designed for scalability, reliability, and high performance.
- **C**: The microservice is designed to scale, reliability, and perform fast.
- **D**: The microservice is designed scaling, reliable, and performance.

---

### MOCK3-040: Speech Delivery: Speaking Cadence Benchmark

**Section**: English Communication | **Difficulty**: Easy

What is the ideal pacing target for automated oral assessment engines?

- **A**: 120 - 150 words per minute
- **B**: 250 - 300 words per minute
- **C**: 50 words per minute
- **D**: As fast as possible

---

Previous: [02_mcq_mock_2_solutions.md](02_mcq_mock_2_solutions.md) | Next: [03_mcq_mock_3_solutions.md](03_mcq_mock_3_solutions.md)
