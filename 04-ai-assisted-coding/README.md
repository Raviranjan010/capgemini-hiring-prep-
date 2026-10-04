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
4. **Does the problem have a fixed historical recurrence window ($k=2$ steps like Decode Ways, Fibonacci, House Robber)?**
   - **Action**: Use **Rolling Variables** (`prev1`, `prev2`) to achieve $O(N)$ Time and strictly $O(1)$ Space.
5. **Does the problem test strict alternating parity (Odd/Even)?**
   - **Action**: Use bitwise operations `((arr[i] ^ arr[i - 1]) & 1) == 0` to detect parity violations. Never use `% 2 == 1` because negative odd numbers yield `-1` in Java.

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
3. [03_tree_lca_bitwise_palindrome_partition.md](03_tree_lca_bitwise_palindrome_partition.md) - Lowest Common Ancestor (LCA), Bitwise Equality Inversions ($A[i] \& A[j] == A[i] \oplus A[j]$), Palindromic Partitioning Min Cuts (DP), and Palindrome Partitioning I (All Partitions with 6-Turn Dialog & Loop Direction Audit).
4. [04_graph_bipartite_course_schedule_islands.md](04_graph_bipartite_course_schedule_islands.md) - Is Graph Bipartite (2-Coloring BFS, Odd Cycle Theorem, 5-Turn Dialog), Course Schedule (Kahn's In-Degree), Number of Islands (In-Place Sinking), and 0/1 Matrix (Multi-Source BFS).
5. [05_dp_decode_ways_and_frequent_patterns.md](05_dp_decode_ways_and_frequent_patterns.md) - Capgemini AI Assist Platform Deep Dive, Decode Ways (LeetCode #91 with 6-Turn Bot Dialog & O(1) space), Coin Change (amount+1 overflow defense), Strict Alternating Parity, Move Hashes to Front, and Run-Length Compression.
6. [06_grid_bfs_shortest_path_obstacles.md](06_grid_bfs_shortest_path_obstacles.md) - Shortest Path in 2D Grid with Obstacles Elimination (LeetCode #1293: BFS with Pruned State Space, Manhattan distance shortcut, one-shot prompt template to save tokens).
7. [07_binary_tree_boundary_traversal.md](07_binary_tree_boundary_traversal.md) - Anti-Clockwise Binary Tree Boundary Traversal (3-phase protocol: left boundary, leaves left-to-right, right boundary bottom-to-top).



