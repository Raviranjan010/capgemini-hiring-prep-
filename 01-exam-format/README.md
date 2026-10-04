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
- **Decision Rule**: Monotonicity $\implies$ Sliding Window; Negatives/Mod/XOR $\implies$ Prefix Sum + Map; Extremes over $K$ $\implies$ Monotonic Deque; Bounded dependency ($k=2$) $\implies$ Rolling DP ($O(1)$ space).

---

## Capgemini AI Assist Round Architecture & Evaluation Engine

The AI Assist Coding Assessment marks a fundamental shift from traditional competitive programming platforms. In conventional coding rounds (e.g., HackerRank, AMCAT, Mettl), evaluation is strictly binary: whether code passes hidden test cases within execution limits. Capgemini's platform evaluates **Generative AI Collaboration Proficiency**, measuring how an engineer leverages LLM tooling without hallucinations, syntax bugs, or algorithmic regressions.

### Platform Interface Layout

```text
+------------------------------------------------------------------------------------+
|                               CAPGEMINI AI ASSIST UI                              |
+------------------------------------+-----------------------------------------------+
| LEFT PANEL: Problem Specification  | RIGHT PANEL: AI Conversational Agent          |
| - Problem Statement & Constraints  | - Prompt Input Bar                            |
| - Sample Inputs & Expected Outputs | - Conversational History (Scored by Engine)   |
| - Edge Case Advisories             | - Contextual Follow-up Chips & Clarifications |
+------------------------------------+-----------------------------------------------+
| BOTTOM PANEL: Code Editor & Execution Console                                      |
| - Language Selector (Java, C++, Python, C#) | [Run Code] [Submit Solution]         |
| - Compilation Output, Custom Test Input, Diff Viewer                               |
+------------------------------------------------------------------------------------+
```

### The Multi-Vector Evaluation Engine

The automated scoring matrix tracks four distinct competencies:

```text
                      AI Collaboration Score (100%)
                                    │
      ┌─────────────────────────────┼─────────────────────────────┐
      ▼                             ▼                             ▼
Prompt Engineering           Algorithmic Defense          Code Review & Audit
    Depth (30%)                  Rigor (35%)                  Speed (35%)
 - Problem reformulation      - Answering bot queries      - Catching AI bugs
 - Explicit edge cases        - Explaining recurrences     - Optimizing space/time
 - Constraint grounding       - Mathematical dry runs      - Validating syntax
```

1. **Prompt Engineering Depth (30%)**:
   - **Banned Behavior**: Submitting bare commands like *"Write the complete solution in Java"* or copy-pasting the raw problem description directly into the prompt bar. This triggers a penalty for lack of independent comprehension.
   - **Target Behavior**: Restating the problem formally, declaring mathematical state transitions, specifying constraints ($O(1)$ space, zero-handling), and directing the bot on design patterns.
2. **Algorithmic Defense Rigor (35%)**:
   - The AI bot generates targeted technical questions (e.g., *"Why did you set $\text{dp}[0] = 1$?"*, *"Can you dry run input '106'?"*).
   - Candidates must explain the underlying logic clearly. Answers are evaluated via semantic matching against gold-standard algorithmic explanations.
3. **Code Review & Auditing (35%)**:
   - The code delivered by the bot may deliberately contain subtle off-by-one errors, unhandled boundary cases (like consecutive zeroes), or suboptimal allocations ($O(N)$ space when $O(1)$ is achievable).
   - Candidates must inspect, patch, refactor, and test the solution before hitting Submit.

---

## Actionable Playbook & Prompt Templates

```text
                      EXAM STRATEGY TIMELINE
                                 │
     ┌───────────────────────────┼───────────────────────────┐
     ▼                           ▼                           ▼
[Minute 00-02]              [Minute 02-06]              [Minute 06-10+]
Classify & Formalize        Drive Bot Interaction       Audit, Optimize & Run
- Identify pattern (DP/Win) - Use Prompt Template       - Verify O(1) space
- Write down constraints    - Answer bot questions      - Dry run edge cases
- Note edge cases (0s/null) - Request strict code       - Execute test suite
```

### High-Scoring Prompt Templates

#### Phase 1: Problem Formulation
```text
I am addressing this problem using Bottom-Up Dynamic Programming.
- State: Let dp[i] represent the total valid decodings for the prefix s[0...i-1].
- Transitions:
    1. Single-digit: If s[i-1] != '0', add dp[i-1].
    2. Two-digit: If 10 <= Integer(s[i-2...i-1]) <= 26, add dp[i-2].
- Base Conditions: dp[0] = 1 (empty prefix), dp[1] = (s[0] != '0') ? 1 : 0.
- Memory: To optimize memory to O(1) space, I only need to maintain the two 
  preceding states (prev1, prev2).

Please provide the optimized O(1) auxiliary space implementation in Java.
```

#### Phase 2: Edge-Case Verification
```text
Please verify that this implementation handles these boundary cases:
1. Leading zero: "028" -> returns 0
2. Valid embedded zeroes: "10" and "20" -> returns 1
3. Invalid embedded zeroes: "30" and "100" -> returns 0
4. Boundary values: "26" -> returns 2; "27" -> returns 1

Does the state machine handle these inputs correctly without throwing index-out-of-bounds exceptions?
```

#### Phase 3: Final Pre-Submission Audit
- [ ] Guard against null, empty strings, and leading zero (`s.charAt(0) == '0' -> return 0`).
- [ ] Space complexity is strictly $O(1)$ using two rolling pointers, not an array of size $N+1$.
- [ ] Substring parsing does not cause `StringIndexOutOfBoundsException` at $i=1$.
- [ ] Early termination triggers if both single-digit and two-digit checks fail (`current == 0`).

---

## Video Walkthrough & Analysis

For step-by-step video walkthroughs of actual mock problems, scoring rubrics, and the assessment interface, review:
- [Capgemini Exceller Exam Analysis](http://www.youtube.com/watch?v=7USJXlHXaiw) — Analyzes the live test interface, section mechanics, and mock questions.
- [KN Academy Capgemini AI Assist Preparation Video](https://youtu.be/Fn41k0hxcs0) — Live demonstration of the AI conversational agent interface, prompt flow, and Decode Ways technical defense.
