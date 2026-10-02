# AI-Assisted Coding Protocol (45 Minutes)

## The 45-Minute Round in Plain Words
In this round, you are given 1 algorithmic coding problem to solve within 45 minutes inside an integrated development portal equipped with an AI chatbot assistant.
- **Evaluation Criteria**: Code correctness, edge case handling, and optimal time/space complexity.
- **Bot Interaction**: Evaluators look for clear, structured, and token-efficient prompts. Asking the bot the entire question verbatim or having long conversational exchanges wastes time and risks hallucinated or non-compiling boilerplate. Provide concise, targeted specifications.

---

## Algorithmic Decision Flowchart

When given an array or subarray problem, determine the approach using this decision sequence:

1. **Are all array elements strictly non-negative ($\ge 0$)?**
   - **YES** $\implies$ Monotonicity holds. Expanding the right pointer strictly increases sum; shrinking the left pointer strictly decreases sum.
   - **Action**: Use **Two-Pointer Sliding Window** ($O(N)$ Time, $O(1)$ Space).
2. **Does the array contain negative numbers, modulo constraints, or bitwise XOR?**
   - **YES** $\implies$ Monotonicity is broken. Adding an element can increase or decrease the sum arbitrarily.
   - **Action**: Use **Prefix Sum / Prefix XOR + HashMap** ($O(N)$ Time, $O(N)$ Space).
3. **Does the problem ask for Sliding Window Maximum or Minimum across fixed size $K$?**
   - **Action**: Use a **Monotonic Deque** ($O(N)$ Time, $O(K)$ Space). Do **NOT** use a PriorityQueue/Heap, which takes $O(N \log K)$ and requires inefficient $O(K)$ stale-element deletions.

> **CRITICAL RULE**: Always use `long` (Java) or `long long` (C++) for prefix sum accumulators to avoid 32-bit integer overflow when elements sum beyond $2 \times 10^9$.

---

---

## Prompt Engineering & Coding Strategies

In Capgemini's coding round, questions penalize trial-and-error prompting. Every API call consumes token limits and turn counts.

### The "RTC-FC" Prompt Structure
Use this five-part formula whenever drafting or analyzing prompt quality:
- **Role (R)**: Sets expertise, perspective, and persona (*"Act as a Lead Java Backend Architect."*)
- **Context (C)**: Operational environment and background (*"We are processing high-frequency UPI transactions."*)
- **Task (T)**: Explicit command or deliverable (*"Refactor the provided payment validation method."*)
- **Format (F)**: Output layout and schema (*"Output only Java code inside Markdown with inline comments."*)
- **Constraints (C)**: Guardrails (*"Ensure O(N) time complexity and no external dependencies."*)

> **Memory Trick**: *"Run To Catch Fast Cars"* (**R**ole • **T**ask • **C**ontext • **F**ormat • **C**onstraints)

### Examination Coding Tactics
1. **Pre-prompt Clarification**: Define constraints (language version, time/space targets, edge cases like empty arrays, negative numbers, or integer overflow) in the first turn.
2. **Zero Trust Review**: LLMs frequently produce code that compiles cleanly but violates asymptotic bounds ($O(N^2)$ vs. $O(N)$) or silently fails edge cases. Always verify complexity and test zero/negative values before submitting.

---

## Token-Saving Prompt Templates

Use these concise prompt structures to get clean code from the exam AI chatbot without exhausting your token allowance:

### 1. Problem Clarification Prompt
```text
Role: Senior Algorithm Engineer.
Context: High-throughput array processing.
Task: Solve [Problem Name]. 
Format: Single Java method with inline comments, no explanatory fluff.
Constraints: Time O(N), Space O(1) or O(N). Handle empty/single-element arrays and negatives.
```

### 2. Shrink-Left Pointer Correction Prompt
```text
When the window condition invalidates, shrink left pointer: 
decrement map frequency, subtract nums[left] from currentSum, remove key if frequency is 0, then increment left++.
```

### 3. Debugging Prompt Pattern
When code fails a test case or compiler check:
```text
Task: Fix the bug in the provided function.
Target Function:
[Paste full target function here]

Failure Details:
- Failing Test Input: [e.g., nums = [-3, -1, -2], k = 2]
- Expected Output: [e.g., -1]
- Actual Output / Compiler Error: [e.g., Output 0 / IndexOutOfBoundsException]

Constraint: Only modify the inner logic of the target function; do not refactor unproblematic helper functions.
```

### 4. Worked Example: Gas Station Single-Pass Prompt
```text
Task: Find starting gas station index for circular tour in Java.
Approach: Single-pass Greedy. Track totalSurplus += gas[i] - cost[i] and currentTank += gas[i] - cost[i].
If currentTank < 0, reset startStation = i + 1 and currentTank = 0.
After the loop, if totalSurplus < 0 return -1, otherwise return startStation.
Complexity: Time O(N), Space O(1).
```

---

## "Before You Submit" Checklist
1. **Negatives & Zeroes**: Did you test negative inputs to verify sliding window assumptions did not fail?
2. **Empty & Single Element**: Does the solution safely handle `nums == null`, `nums.length == 0`, and `nums.length == 1`?
3. **Integer Overflow**: Are prefix sums, products, and accumulators declared as `long` / `long long`?
4. **Duplicates & Modulo**: Did you handle negative modulo remainders using `((r % k) + k) % k`?
5. **Boundary Parameters**: Have you tested boundary values ($k = 0$, $k > n$, or $k$ larger than total sum)?

---

## Module Files
1. [01_sliding_window.md](01_sliding_window.md) - Maximum Sum Harmonic Subarray and Sliding Window Maximum (Monotonic Deque).
2. [02_prefix_sum_hashmap.md](02_prefix_sum_hashmap.md) - Subarray Sum = K, Continuous Subarray Sum, Divisible by K, Contiguous Array, XOR = K, and Single Number III.
