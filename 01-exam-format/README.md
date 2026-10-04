# Capgemini Recruitment Assessment Format & Blueprint

## Capgemini Exceller Test Pipeline Breakdown

Capgemini has revamped its hiring framework from conventional syntax quizzes to AI workflows, algorithmic debugging, and collaborative system architecture.

```text
                     CAPGEMINI EXCELLER ASSESSMENT ARCHITECTURE
                                         │
        ┌────────────────────────────────┴────────────────────────────────┐
        ▼                                                                 ▼
[Section 1: Technical MCQs]                                    [Section 2: Practical Coding]
- 40 Questions total (50 Mins)                                 - 1 Debugging Problem (15-20 Mins)
  ├─ AI Literacy: 20 Questions (25 Mins)                         - 1 AI Assist Problem (30 Mins)
  └─ CS Fundamentals: 20 Questions (25 Mins)
```

| Stage | Section Name | Duration | Questions / Problem | Nature & Scoring Rules |
| :---: | :--- | :---: | :---: | :--- |
| **Section 1A** | AI Literacy & GenAI | 25 min | 20 MCQs | Real-world 4-6 line scenarios: LLM runtimes, prompt injection, RAG architectures, chunking, vector indexing (HNSW/IVF), and RAGAS metrics. |
| **Section 1B** | CS Fundamentals & Pseudocode | 25 min | 20 MCQs | Scenario-based OS (file loaders, deadlocks), DBMS isolation levels, networking, and bitwise/recursive pseudocode tracing. |
| **Section 2A** | Code Debugging Assessment | 15–20 min | 1 Complex Problem | Finding and fixing subtle logic bugs: array index bounds (`i < n - 1`), negative number modulo traps (`-3 % 2`), binary search overflows, and duplicate counts. |
| **Section 2B** | AI-Assisted (Vibe) Coding | 30 min | 1 Interactive Problem | Algorithmic challenges (Grid BFS with obstacle quotas, Binary Tree boundary traversal, 1D DP) solved via an AI conversational bot under a strict ~2,000 token budget. |
| **Subsequent** | Cognitive & Behavioral Profiling | ~68 min total | 4 Modules | Motion Challenge (6m), Bubble/Grid Memory (12m), Deductive Logic (25m), Workplace Behavioral (25m). |
| **Subsequent** | Spoken Communication | ~30 min | Automated Voice | SVAR / Versant audio evaluation (reading, repetition, sentence builds, extempore speech). |

### Key Assessment Rules & Timing Dynamics

1. **Time Allocation & Pacing**:
   - Technical MCQs provide **50 minutes for 40 questions** (~1.15 minutes / 75 seconds per question).
   - Questions use 4- to 6-line production engineering scenarios rather than simple 1-line definitions.
2. **Relative Cutoffs & Scoring**:
   - The technical MCQs are intentionally rigorous. A score of **6 to 8 questions correct out of 20 per section** (7–10 in AI Literacy, 8–10 in CS) is competitive enough to clear the threshold for standard packages (e.g., 4.25 LPA).
3. **AI Assist Token Budget**:
   - Candidates receive a finite pool of tokens (**~2,000 tokens**) to chat with the built-in AI bot. Every query typed and every response generated consumes tokens from this budget.
4. **Intentional Bot Flaws**:
   - The platform AI assistant is programmed to generate code with intentional edge-case bugs, off-by-one errors, or incorrect algorithmic complexity ($O(N \cdot M)$ standard DP instead of BFS for 4-directional obstacle paths) on the first or second iteration. Evaluation explicitly measures your ability to identify, critique, and correct these flaws.

---

## Master Strategy for Candidates

```text
               HOW TO APPROACH THE 4 EXAM SECTIONS
                                │
   ┌────────────────┬───────────┴───────────┬────────────────┐
   ▼                ▼                       ▼                ▼
[AI Literacy]   [CS MCQs]              [Debugging]     [AI Assist]
Focus on RAG,   Skip long edge cases   Look for loop   Prompt explicitly;
context limits, first; aim for 7-8     bounds and      manage your
and embeddings  solid answers          negative %      2,000 tokens
```

| Component | Target Score / Strategy | Common Traps | High-Yield Concepts |
| :--- | :--- | :--- | :--- |
| **AI Literacy** | 7–10 / 20 correct answers | Don't assume natural language instructions guarantee output formats. | Context windows, RAG chunking, HNSW/IVF, Hybrid Search (BM25), RAGAS metrics, Lost-in-the-Middle. |
| **CS Fundamentals** | 8–10 / 20 correct answers | Watch for 1.15-minute time limits on scenario questions. | Brian Kernighan's bit algorithm, recursion trees, Deadlock prevention formula, SQL transaction isolation. |
| **Code Debugging** | 100% test case pass | Watch out for `-3 % 2 == -1` in parity checks. | `i < n - 1` vs `i < n`, binary search integer overflow (`low + (high - low) / 2`), duplicate prints. |
| **AI Assist Coding** | Pass hidden test cases in <1,000 tokens | Don't just paste code; direct the AI's algorithm choices. | BFS with obstacle tracking, Boundary traversal, Decode Ways, Coin Change. |

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
