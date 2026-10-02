# Capgemini Recruitment Assessment Format & Blueprint

## Capgemini Exceller Test Pipeline Breakdown

Capgemini has revamped its hiring framework from conventional syntax quizzes to AI workflows, algorithmic debugging, and collaborative system architecture.

```text
Capgemini Exceller Test Pipeline
├── 1. AI Literacy & GenAI           (20 Questions | 20 Mins)
├── 2. Technical MCQs & Pseudo-code   (20 Questions | 20 Mins)
├── 3. Code Debugging Assessment     (1 Complex Problem | 20 Mins)
└── 4. AI-Assisted (Vibe) Coding     (1 Interactive Problem | 20 Mins)
```

| Stage | Section Name | Duration | Questions / Problem | Nature & Scoring Rules |
| :---: | :--- | :---: | :---: | :--- |
| **Section 1** | AI Literacy & GenAI | 20 min | 20 MCQs | Foundational LLM mechanics, embeddings, RAG vs fine-tuning, prompt design, temperature, and hallucinations. |
| **Section 2** | Technical MCQs & Pseudo-code | 20 min | 20 MCQs | Operator precedence, bitwise logic, stack/queue tracing, tree reconstructions, recursion, and CS core. |
| **Section 3** | Code Debugging Assessment | 20 min | 1 Complex Problem | Spotting logic flaws, edge cases, unidirectional graphs, premature returns, and DP loop bounds. All test cases must pass. |
| **Section 4** | AI-Assisted (Vibe) Coding | 20 min | 1 Interactive Problem | Hard algorithmic problem solved via integrated AI chatbot under strict token budgets and hidden test cases. |
| **Subsequent** | Cognitive & Behavioral Profiling | ~68 min total | 4 Modules | Motion Challenge (6m), Bubble/Grid Memory (12m), Deductive Logic (25m), Workplace Behavioral (25m). |
| **Subsequent** | Spoken Communication | ~30 min | Automated Voice | SVAR / Versant audio evaluation (reading, repetition, sentence builds, extempore speech). |

---

## Actionable Section Strategy Checklist

### 1. AI Literacy (First 20 Mins)
- **Pacing**: Aim for ~45 seconds per question to finish with 5 minutes to spare.
- **Focus**: Do not overthink edge cases; questions assess awareness of fundamental LLM mechanics (embeddings, RAG vs. Fine-tuning, prompt design, temperature, and hallucinations).
- **Core Spotting**:
  - Deterministic DB retrieval (Librarian) vs Probabilistic next-token generation (Author).
  - Temperature: Low ($0.0 \le T \le 0.2$) for code/SQL/compliance; High ($0.7 \le T \le 1.0$) for creativity.
  - RAG: External grounding in verified sources to solve hallucination and cutoff without fine-tuning.
  - Direct Injection (jailbreaks) vs Indirect Injection (data ingestion).

### 2. Pseudo-code & Logic (Next 20 Mins)
- **Precedence First**: Always write down operator precedence tables for arithmetic/bitwise questions to avoid falling for compiler assumptions.
  - Arithmetic `* / %` > `+ -` > Bitwise Shifts `<< >>` > Relational `< <=` > Equality `== !=` > Bitwise `&` > Bitwise `^` > Bitwise `|` > Logical `&&` > Logical `||`.
- **Trace Mentally**: Trace loops mentally using tabular step rows for variables ($i$, $j$, count, accumulator).
- **Stack Postfix**: Push operands, pop right operand first on operator, push result back.

### 3. Code Debugging (20 Mins)
- **Read & Inspect**: Read the problem statement and inspect the provided code side-by-side.
- **Four Primary Failure Modes**:
  1. **Unidirectional vs. Bidirectional edges** in graph queries (`adj[u].push_back(v)` without `adj[v].push_back(u)`).
  2. **Premature loop return statements** (`return result;` placed inside loop body instead of after loop).
  3. **Strict 0-indexed vs. 1-indexed vector allocation mismatch** (allocating size $n$ but accessing index $n$).
  4. **Forward vs. backward capacity traversal in DP arrays** (forward loop turns 0/1 knapsack into unbounded knapsack).

### 4. AI-Assisted Coding (20 Mins)
- **Be Technically Explicit**: In your first prompt, specify: Data structures, Base cases, Traversal order, and Time/Space bounds.
- **Token Discipline**: Use remaining tokens only to fix failing edge cases rather than rebuilding the solution from scratch.
- **Decision Rule**: Monotonicity $\implies$ Sliding Window; Negatives/Mod/XOR $\implies$ Prefix Sum + Map; Extremes over $K$ $\implies$ Monotonic Deque.

---

## Video Walkthrough & Analysis

For a step-by-step video walkthrough of actual mock problems, scoring rubrics, and the assessment interface, review:
- [Capgemini Exceller Exam Analysis](http://www.youtube.com/watch?v=7USJXlHXaiw) — Analyzes the live test interface, section mechanics, and mock questions.
