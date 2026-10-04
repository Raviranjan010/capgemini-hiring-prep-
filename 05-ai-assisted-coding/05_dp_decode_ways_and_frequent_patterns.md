[Home](../README.md) > [05-ai-assisted-coding](README.md) > 05_dp_decode_ways_and_frequent_patterns.md

# Comprehensive Guide: Capgemini AI Assist Coding Round & Master Question Bank

**Tag**: [VIDEO] [MOCK-EXAM] [RECENT-PATTERN]  
**Video Reference**: [KN Academy Capgemini AI Assist Preparation Video](https://youtu.be/Fn41k0hxcs0)  
**Practice Links**:
- Decode Ways: [LeetCode #91](https://leetcode.com/problems/decode-ways/)
- Coin Change: [LeetCode #322](https://leetcode.com/problems/coin-change/)
- Climbing Stairs: [LeetCode #70](https://leetcode.com/problems/climbing-stairs/)
- House Robber: [LeetCode #198](https://leetcode.com/problems/house-robber/)
- Word Break: [LeetCode #139](https://leetcode.com/problems/word-break/)

---

## 1. Deep Dive: The Capgemini AI Assist Round Architecture

The AI Assist Coding Assessment marks a fundamental shift from traditional competitive programming platforms. In conventional coding rounds (e.g., HackerRank, AMCAT, Mettl), evaluation is strictly binary: whether code passes hidden test cases within execution limits.

Capgemini's platform evaluates **Generative AI Collaboration Proficiency**, measuring how an engineer leverages LLM tooling without hallucinations, syntax bugs, or algorithmic regressions.

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

- **Prompt Engineering Depth (30%)**:
  - **Banned Behavior**: Submitting bare commands like *"Write the complete solution in Java"* or copy-pasting the raw problem description directly into the prompt bar. This triggers a penalty for lack of independent comprehension.
  - **Target Behavior**: Restating the problem formally, declaring mathematical state transitions, specifying constraints ($O(1)$ space, zero-handling), and directing the bot on design patterns.
- **Algorithmic Defense Rigor (35%)**:
  - The AI bot generates targeted technical questions (e.g., *"Why did you set $\text{dp}[0] = 1$?"*, *"Can you dry run input '106'?"*).
  - Candidates must explain the underlying logic clearly. Answers are evaluated via semantic matching against gold-standard algorithmic explanations.
- **Code Review & Auditing (35%)**:
  - The code delivered by the bot may deliberately contain subtle off-by-one errors, unhandled boundary cases (like consecutive zeroes), or suboptimal allocations ($O(N)$ space when $O(1)$ is achievable).
  - Candidates must inspect, patch, refactor, and test the solution before hitting Submit.

---

## 2. Exhaustive Problem Breakdown: Decode Ways (LeetCode #91)

### Mathematical Formulation

Given an encoded string of numeric digits $s = s[0 \dots n-1]$, where mapping is defined by:
$$f: \{1, 2, \dots, 26\} \to \{\text{'A'}, \text{'B'}, \dots, \text{'Z'}\}$$

Determine the total cardinality of valid character sequences:
$$\vert{}\mathcal{D}(s)\vert{}$$

A sequence of indices $0 = i_0 < i_1 < i_2 < \dots < i_k = n$ forms a valid partition of $s$ if and only if for every token $t_j = s[i_j \dots i_{j+1}-1]$:
1. $\vert{}t_j\vert{} \in \{1, 2\}$
2. If $\vert{}t_j\vert{} = 1$, then $t_j \in \{\text{'1'}, \text{'2'}, \dots, \text{'9'}\}$ (strictly no leading or standalone zero).
3. If $\vert{}t_j\vert{} = 2$, then $10 \le \text{Integer}(t_j) \le 26$.

### Transition Mechanics & Decision Tree

At every character $s[i-1]$ (1-based indexing for length $i$), evaluate two branching paths:

```text
                             Prefix s[0...i-1]
                                     │
               ┌─────────────────────┴─────────────────────┐
               ▼                                           ▼
      Take Single Digit (s[i-1])                   Take Two Digits (s[i-2...i-1])
               │                                           │
       Is s[i-1] != '0'?                           Is 10 <= value <= 26?
        /             \                             /             \
      YES              NO                         YES              NO
       │                │                          │                │
  Valid: add dp[i-1]  Invalid: add 0        Valid: add dp[i-2]   Invalid: add 0
```

Combining these transitions yields the core recurrence:
$$\text{dp}[i] = \underbrace{\Big(\mathbb{I}(s[i-1] \ne \text{'0'}) \cdot \text{dp}[i-1]\Big)}_{\text{Single-digit branch}} + \underbrace{\Big(\mathbb{I}(10 \le \text{val}(s[i-2 \dots i-1]) \le 26) \cdot \text{dp}[i-2]\Big)}_{\text{Two-digit branch}}$$

Where $\mathbb{I}(P)$ is the indicator function:
$$\mathbb{I}(P) = \begin{cases} 1 & \text{if } P \text{ is true} \\ 0 & \text{if } P \text{ is false} \end{cases}$$

### Granular Edge-Case Taxonomy

| Scenario Class | Test String | Correct Output | Common Algorithmic Failure | Root Cause |
| :--- | :--- | :---: | :--- | :--- |
| **Leading Zero** | `"0"`, `"06"`, `"012"` | `0` | Returns 1 or 2 | Fails to detect that an initial `'0'` has no valid mapping, processing `'6'` or `'12'` as a clean tail. |
| **Trapped Zero (Valid)** | `"10"`, `"20"`, `"110"` | `1`, `1`, `1` | Returns 0 (False Zero) | Single-digit check on `'0'` evaluates to 0, zeroing out running state if not coupled to `prev2`. |
| **Terminal Zero (Invalid)**| `"30"`, `"70"`, `"90"` | `0` | Returns 1 or throws index error | Substring `"30"` $> 26$, and single digit `'0'` is invalid. Both branches yield 0, making string undecodable. |
| **Consecutive Zeroes** | `"100"`, `"2004"` | `0` | Returns 1 | Second `'0'` can neither stand alone nor pair with the preceding `'0'` (no token `"00"`). |
| **Upper Bound Limit** | `"26"` vs `"27"` | `"26"` $\to 2$<br/>`"27"` $\to 1$ | Returns 2 for `"27"` | Fails to enforce $\le 26$, treating `"27"` as valid letter mapping. |
| **Symmetric Alternation** | `"1111"` | `5` | Returns 4 | Missing combinations in recurrence; mimics Fibonacci: $\{F_1=1, F_2=2, F_3=3, F_4=5\}$. |

---

## 3. Transcript-Grounded AI Bot Interactions & Technical Defense

```text
                             AI AGENT CONVERSATION FLOW
                                         │
        ┌────────────────────────────────┴────────────────────────────────┐
        ▼                                                                 ▼
[Phase 1: Conceptualization]                                     [Phase 2: Mechanics]
- Q1: Problem Reformulation                                      - Q3: The Zero Anomaly ('10','20','30','06')
- Q2: State Transition & Recurrence                              - Q4: Base Case Foundation (dp[0]=1)
        │                                                                 │
        └────────────────────────────────┬────────────────────────────────┘
                                         ▼
                             [Phase 3: Execution & Scale]
                             - Q5: Granular Trace of '106' & '226'
                             - Q6: State Compression O(N) -> O(1)
```

### Scenario 1: Conceptual Problem Reformulation
- **Bot Prompt**: *"Can you explain the question in your own words?"*
- **Candidate Answer**:
  > *"The problem asks for the total number of distinct alphanumeric string permutations that can generate the digit sequence $s$, under the fixed mapping $A \to 1, B \to 2, \dots, Z \to 26$.*  
  > *Because codes vary between 1 and 2 digits in length, this maps to a constrained tiling problem on a 1D grid of length $N$:*  
  > *- Tiles of length 1 are valid over character set $\{'1', '2', \dots, '9'\}$.*  
  > *- Tiles of length 2 are valid over numeric interval $[10, 26]$.*  
  > *Any partition utilizing a tile outside these parameters (such as an isolated zero or a two-digit integer $\ge 27$) invalidates that specific path. The goal is to compute the sum of all valid paths spanning indices $0$ to $n$."*

### Scenario 2: Approach & Recurrence Defense
- **Bot Prompt**: *"What approach did you use to solve this problem, and how do you avoid redundant recalculation?"*
- **Candidate Answer**:
  > *"I used bottom-up Dynamic Programming to eliminate the exponential time complexity ($O(2^N)$) of naive recursion caused by overlapping subproblems.*  
  > *Let $\text{dp}[i]$ denote the total valid decodings for prefix $s[0 \dots i-1]$:*  
  > $$\text{dp}[i] = \text{Branch}_1 + \text{Branch}_2$$  
  > *Where:*  
  > *- $\text{Branch}_1 = \text{dp}[i-1]$ if $s[i-1] \in ['1', '9']$, else $0$.*  
  > *- $\text{Branch}_2 = \text{dp}[i-2]$ if the integer value of $s[i-2 \dots i-1]$ satisfies $10 \le v \le 26$, else $0$.*  
  > *Storing previous states eliminates repeated evaluations of identical string tails, bounding runtime to $O(N)$."*

### Scenario 3: The Zero Anomaly
- **Bot Prompt**: *"Why is '0' a special case? Explain what should happen for strings '10', '20', '30', and '06'."*
- **Candidate Answer**:
  > *"'0' is an anomaly because the alphabet encoding domain is strictly 1-indexed ($1 \le \text{char} \le 26$). A standalone zero cannot map to any character.*  
  > *- **'10' and '20' (Valid)**: For single digit: $s[1] = \text{'0'} \implies$ yields 0 paths. For two digits: $s[0 \dots 1] \in \{10, 20\} \implies$ valid mapping to 'J' and 'T'. Total ways: $\text{dp}[2] = 0 + \text{dp}[0] = 1$.*  
  > *- **'30' (Invalid)**: For single digit: $s[1] = \text{'0'} \implies 0$ paths. For two digits: $30 > 26 \implies 0$ paths. $\text{dp}[2] = 0 + 0 = 0$. The sequence terminates as unparseable.*  
  > *- **'06' (Invalid)**: First character is '0', which matches no letter. The string cannot be read as a 2-digit number because '06' has a leading zero (numeric token values must be $\ge 10$). Total ways = 0."*

### Scenario 4: Base Case Foundation
- **Bot Prompt**: *"What should the DP base case be, and why is dp[0] = 1 rather than 0?"*
- **Candidate Answer**:
  > *"$\text{dp}[0] = 1$ models the empty string prefix. This is essential for structural consistency in dynamic programming:*  
  > *1. **Combinatorial interpretation**: There is exactly one way to decode an empty message: select the empty set $\emptyset$.*  
  > *2. **Algebraic necessity**: Consider string `"12"`. At index $i = 2$, two-digit slice `"12"` is valid (`'L'`). The recurrence computes:*  
  > $$\text{dp}[2] = \text{dp}[1] (\text{from single digit '2'}) + \text{dp}[0] (\text{from double digit '12'})$$  
  > *Here, $\text{dp}[1] = 1$ (`"AB"`). To count the valid decoding `"L"`, the two-digit lookup must contribute $1$. If $\text{dp}[0] = 0$, that entire branch collapses:*  
  > $$\text{dp}[2] = 1 + 0 = 1 \quad (\text{Incorrect: drops 'L'})$$  
  > *Setting $\text{dp}[0] = 1$ ensures that any initial 2-digit token seeds a valid path."*

### Scenario 5: Full Execution Trace
- **Bot Prompt**: *"Can you walk through the DP calculation of '106' and '226' step-by-step?"*
- **Candidate Answer**:

#### Detailed Trace for `"106"`
- String: $s = \text{"106"}$, length $n = 3$.
- Initialize array: $\text{dp} = [0, 0, 0, 0]$ of size $n+1 = 4$.
- Base assignment: $\text{dp}[0] = 1$.
- First char: $s[0] = \text{'1'} \ne \text{'0'} \implies \text{dp}[1] = 1$.
```text
Step 1: i = 2 (Character '0', Substrings: Single '0', Pair '10')
- Single-digit: s[1] = '0' -> Invalid (adds 0)
- Two-digit:    s[0...1] = "10" -> 10 <= 10 <= 26 -> Valid!
  dp[2] = dp[2] + dp[0] = 0 + 1 = 1
  DP State: [1, 1, 1, 0]

Step 2: i = 3 (Character '6', Substrings: Single '6', Pair '06')
- Single-digit: s[2] = '6' != '0' -> Valid!
  dp[3] = dp[3] + dp[2] = 0 + 1 = 1
- Two-digit:    s[1...2] = "06" -> 6 < 10 -> Invalid (Leading Zero)
  dp[3] remains 1
  DP State: [1, 1, 1, 1]

Final Result: dp[3] = 1 (Decoded strictly as "JF")
```

#### Detailed Trace for `"226"`
- String: $s = \text{"226"}$, length $n = 3$.
- Base assignment: $\text{dp}[0] = 1, \text{dp}[1] = 1$ ($s[0] = \text{'2'} \to \text{'B'}$).
```text
Step 1: i = 2 (Character '2', Substrings: Single '2', Pair '22')
- Single-digit: s[1] = '2' != '0' -> Valid!
  dp[2] += dp[1] (1)
- Two-digit:    s[0...1] = "22" -> 10 <= 22 <= 26 -> Valid!
  dp[2] += dp[0] (1)
  dp[2] = 1 + 1 = 2
  Decodings: "BB", "V"
  DP State: [1, 1, 2, 0]

Step 2: i = 3 (Character '6', Substrings: Single '6', Pair '26')
- Single-digit: s[2] = '6' != '0' -> Valid!
  dp[3] += dp[2] (2)
- Two-digit:    s[1...2] = "26" -> 10 <= 26 <= 26 -> Valid!
  dp[3] += dp[1] (1)
  dp[3] = 2 + 1 = 3
  Decodings: "BBF", "VF", "BZ"
  DP State: [1, 1, 2, 3]

Final Result: dp[3] = 3
```

### Scenario 6: Memory Complexity Optimization
- **Bot Prompt**: *"Can this solution be optimized from $O(N)$ extra space to $O(1)$ space? Explain which previous states are actually required."*
- **Candidate Answer**:
  > *"Yes. The recurrence relation is:*  
  > $$\text{dp}[i] = c_1 \cdot \text{dp}[i-1] + c_2 \cdot \text{dp}[i-2]$$  
  > *Computing state $i$ only requires access to the immediate two preceding values ($i-1$ and $i-2$). Older states ($i-3, i-4, \dots$) are never read again.*  
  > *We can drop the size-$N$ array and maintain two scalar rolling variables:*  
  > *- `prev1` stores $\text{dp}[i-1]$ (starts as $\text{dp}[1]$).*  
  > *- `prev2` stores $\text{dp}[i-2]$ (starts as $\text{dp}[0]$).*  
  > *In each loop step, compute `current`, shift `prev2 = prev1`, and update `prev1 = current`. This reduces auxiliary space to $O(1)$ while keeping runtime $O(N)$."*

---

## 4. Multi-Language Production Source Code

### Java (Optimal $O(1)$ Space)
```java
class Solution {
    public int numDecodings(String s) {
        // Guard against null, empty string, or leading zero
        if (s == null || s.length() == 0 || s.charAt(0) == '0') {
            return 0;
        }

        int n = s.length();
        int prev2 = 1; // Represents dp[0] (empty string base)
        int prev1 = 1; // Represents dp[1] (first character, known != '0')

        for (int i = 1; i < n; i++) {
            int current = 0;
            char currChar = s.charAt(i);
            char prevChar = s.charAt(i - 1);

            // 1. Single-digit branch: s[i] in ['1'...'9']
            if (currChar != '0') {
                current += prev1;
            }

            // 2. Two-digit branch: s[i-1...i] in [10...26]
            int twoDigitVal = (prevChar - '0') * 10 + (currChar - '0');
            if (twoDigitVal >= 10 && twoDigitVal <= 26) {
                current += prev2;
            }

            // Early exit: impossible to decode further
            if (current == 0) {
                return 0;
            }

            // Slide the window forward
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

### C++ (Optimal $O(1)$ Space)
```cpp
#include <string>

class Solution {
public:
    int numDecodings(const std::string& s) {
        if (s.empty() || s[0] == '0') {
            return 0;
        }

        int n = s.length();
        int prev2 = 1; // dp[0]
        int prev1 = 1; // dp[1]

        for (int i = 1; i < n; ++i) {
            int current = 0;
            char currChar = s[i];
            char prevChar = s[i - 1];

            // Single digit
            if (currChar != '0') {
                current += prev1;
            }

            // Two digits
            int twoDigit = (prevChar - '0') * 10 + (currChar - '0');
            if (twoDigit >= 10 && twoDigit <= 26) {
                current += prev2;
            }

            // Dead end
            if (current == 0) {
                return 0;
            }

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
};
```

### Python 3 (Optimal $O(1)$ Space)
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == '0':
            return 0
            
        n = len(s)
        prev2 = 1  # dp[0]
        prev1 = 1  # dp[1]
        
        for i in range(1, n):
            current = 0
            curr_char = s[i]
            prev_char = s[i - 1]
            
            # Single-digit parse
            if curr_char != '0':
                current += prev1
                
            # Two-digit parse
            two_digit = int(prev_char + curr_char)
            if 10 <= two_digit <= 26:
                current += prev2
                
            if current == 0:
                return 0
                
            prev2, prev1 = prev1, current
            
        return prev1
```

---

## 5. Related Algorithmic Problems: Comparative Matrix

```text
                                  1D DYNAMIC PROGRAMMING
                                             │
      ┌──────────────────────┬───────────────┴───────────────┬──────────────────────┐
      ▼                      ▼                               ▼                      ▼
[Climbing Stairs]      [Decode Ways]                 [House Robber]          [Word Break]
LC #70                 LC #91                        LC #198                 LC #139
Unconditional Step     Constrained Transition        Max Value Exclusion     Dictionary Lookup
F(n) = F(n-1) + F(n-2) Conditional Fib (c1*P1+c2*P2) max(dp[i-1], val+dp[i-2]) dp[i] |= dp[j] & dict.contains
```

| Problem Title | LeetCode # | State Definition | Transition Recurrence | Edge Case Nuance |
| :--- | :---: | :--- | :--- | :--- |
| **Climbing Stairs** | LC #70 | $\text{dp}[i]$: Ways to reach step $i$. | $\text{dp}[i] = \text{dp}[i-1] + \text{dp}[i-2]$ | Base conditions: $\text{dp}[1]=1, \text{dp}[2]=2$. No invalid steps exist. |
| **Decode Ways** | LC #91 | $\text{dp}[i]$: Ways to decode prefix of length $i$. | $\text{dp}[i] = c_1 \text{dp}[i-1] + c_2 \text{dp}[i-2]$<br/>where $c_1, c_2 \in \{0, 1\}$. | Zero is invalid standalone. Leading zeroes collapse prefix to 0. |
| **Decode Ways II** | LC #639 | $\text{dp}[i]$: Ways to decode string with `'*'` wildcard ($1\text{–}9$). | Transitions scale by multipliers:<br/>$\text{dp}[i] = m_1 \text{dp}[i-1] + m_2 \text{dp}[i-2]$ | `'*'` can represent $1\text{–}9$. Pair `"*"` can yield up to 15 interpretations ($11\text{–}19, 21\text{–}26$). Requires modulo $10^9+7$. |
| **House Robber** | LC #198 | $\text{dp}[i]$: Maximum stolen loot up to house $i$. | $\text{dp}[i] = \max(\text{dp}[i-1], \text{dp}[i-2] + \text{nums}[i])$ | Optimization problem rather than combinatorial counting. |
| **Word Break** | LC #139 | $\text{dp}[i]$: Boolean indicating if $s[0 \dots i-1]$ can be segmented. | $\text{dp}[i] = \bigvee_{j=0}^{i-1} (\text{dp}[j] \land (s[j \dots i-1] \in \mathcal{D}))$ | Lookups depend on arbitrary lengths rather than a constant window of size 2. |

---

## 6. Advanced Practice MCQs with Step-by-Step Analysis

### MCQ 1 (AIC-022): Multi-Token Decoding Resolution
**Tag**: [MOCK-EXAM]  
Given the encoded input string $s = \text{"12121"}$, what is the total count of valid character decodings?
- A) 5
- B) 8
- C) 7
- D) 9

**Correct Answer**: **B) 8**  
**Why**:
Notice that every character is either `'1'` or `'2'`. Every single digit is non-zero ($1 \le d \le 9$), and every adjacent pair is either `"12"` or `"21"`, both of which are $\le 26$. Because both single-digit and two-digit transitions are valid at every index, the problem simplifies directly to the Fibonacci sequence:
- $\text{dp}[0] = 1$
- $\text{dp}[1] = 1$ (`"1"`: `"A"`)
- $\text{dp}[2] = \text{dp}[1] + \text{dp}[0] = 1 + 1 = 2$ (`"12"`: `"AB"`, `"L"`)
- $\text{dp}[3] = \text{dp}[2] + \text{dp}[1] = 2 + 1 = 3$ (`"121"`: `"ABA"`, `"LA"`, `"AU"`)
- $\text{dp}[4] = \text{dp}[3] + \text{dp}[2] = 3 + 2 = 5$ (`"1212"`)
- $\text{dp}[5] = \text{dp}[4] + \text{dp}[3] = 5 + 3 = 8$ (`"12121"`)
**5-Second Shortcut**: For strings consisting exclusively of `'1'`s and `'2'`s where no adjacent pair exceeds 26, the answer is $F_{n+1}$ (Fibonacci: $1, 2, 3, 5, 8$). Length 5 gives 8.  
**Trap**: Forgetting that double-digit pairs like `"21"` and `"12"` are both $\le 26$ and counting only single-digit configurations.

---

### MCQ 2 (AIC-023): Structural Trapping via Intermediate Zeroes
**Tag**: [MOCK-EXAM]  
Evaluate the output of the optimal decoder when presented with string $s = \text{"2304"}$.
- A) 1
- B) 2
- C) 0
- D) Exception thrown

**Correct Answer**: **C) 0**  
**Why**:
Compute index by index:
1. $i = 1$ ($s[0] = \text{'2'}$): $\text{dp}[1] = 1$.
2. $i = 2$ ($s[1] = \text{'3'}$): Single digit `'3'` is valid ($\text{dp} += \text{dp}[1] = 1$). Pair `"23"` is valid ($\text{dp} += \text{dp}[0] = 1$). Thus, $\text{dp}[2] = 2$.
3. $i = 3$ ($s[2] = \text{'0'}$): Single digit `'0'` is invalid ($0$ ways). Pair $s[1 \dots 2] = \text{"30"}$. Since $30 > 26$, this pair is invalid. Both branches fail: $\text{dp}[3] = 0 + 0 = 0$.
Once $\text{dp}[3] = 0$, the function triggers an early exit and returns 0.  
**5-Second Shortcut**: Check any `'0'`. If the preceding digit is $> 2$ (e.g., `'30'`, `'40'`, ..., `'90'`), it cannot be decoded. Immediate answer: 0.  
**Trap**: Assuming `"30"` can be parsed as letter 3 (`'C'`) followed by `'0'`.

---

### MCQ 3 (AIC-024): Architectural Complexity Limits
**Tag**: [MOCK-EXAM]  
Why is it impossible to compress the auxiliary space of Word Break (LC #139) to $O(1)$ in the same manner as Decode Ways (LC #91)?
- A) Word Break uses words of variable length $L \in [1, \max\_len]$, requiring transitions from states up to $\max\_len$ steps behind, whereas Decode Ways has a fixed maximum token length of 2.
- B) Word Break operates on strings while Decode Ways operates on integers.
- C) Word Break requires depth-first search rather than dynamic programming.
- D) Word Break is NP-Complete, preventing dynamic programming solutions.

**Correct Answer**: **A**  
**Why**:
The state transition of Decode Ways has a strictly bounded historical dependency: $\text{dp}[i] = f(\text{dp}[i-1], \text{dp}[i-2])$. The maximum window size is fixed at $k=2$, allowing $k=2$ rolling variables ($O(1)$ space). In Word Break, dictionary words have arbitrary lengths up to $N$. Thus, $\text{dp}[i]$ may depend on $\text{dp}[j]$ for any $0 \le j < i$. The window size is unbounded ($O(N)$ or $O(\text{max\_word\_len})$), preventing true $O(1)$ scalar compression without restricting dictionary parameters.  
**5-Second Shortcut**: Look for bounded transition window ($k=2 \implies O(1)$ space vs. variable window $\implies O(N)$ space).  
**Trap**: Confusing algorithmic paradigm differences with computational complexity classes (Word Break is $O(N^2)$, not NP-Complete).

---

### MCQ 4 (AIC-025): Base Condition Counterfactual
**Tag**: [MOCK-EXAM]  
If an engineer implements Decode Ways with the initialization $\text{dp}[0] = 0$ and $\text{dp}[1] = 1$, what will the algorithm return for the valid input string $s = \text{"26"}$?
- A) 2
- B) 0
- C) 1
- D) Runtime Exception

**Correct Answer**: **C) 1**  
**Why**:
Trace execution with the buggy base condition:
- Given $\text{dp}[0] = 0, \text{dp}[1] = 1$.
- String $s = \text{"26"}$, length $n = 2$. Loop runs for $i = 2$ ($s[1] = \text{'6'}$):
  - Single-digit branch: $s[1] = \text{'6'} \ne \text{'0'}$, so $\text{current} += \text{dp}[1] \implies \text{current} += 1$. (Represents `"B-F"`).
  - Two-digit branch: $s[0 \dots 1] = \text{"26"}$. Since $10 \le 26 \le 26$, it adds $\text{dp}[0]$. But $\text{dp}[0] = 0 \implies \text{current} += 0$. (Fails to count `"Z"`).
- Loop finishes; code returns `current = 1`. The correct answer is 2 (`"BF"`, `"Z"`).  
**5-Second Shortcut**: If $\text{dp}[0] = 0$, any two-digit initial number ($10\text{–}26$) loses its double-digit representation, decreasing the count by 1.  
**Trap**: Thinking that setting $\text{dp}[0] = 0$ returns 0 or throws an error.

---

## 7. Capgemini Exceller Question Bank: High-Frequency AI Assist Problems

In this round, an AI chatbot is provided on-screen. You receive a token budget (typically ~2000 tokens) and must restate the problem, explain your recurrence/approach, and verify edge cases before generating the code.

```text
                  FREQUENT AI ASSIST PATTERNS
                               │
     ┌─────────────────────────┼─────────────────────────┐
     ▼                         ▼                         ▼
Coin Change / Knapsack    Array Partitioning        String Reductions
(Unbounded / 0-1 DP)      (Strict Odd/Even)         (Run-Length & Hashes)
```

---

### Question 1.1 (AIC-026): Coin Change Problem (Minimum Coins)
**Tag**: [MOCK-EXAM]  
**Frequency**: High (Repeated in multiple Exceller drives).  
**Practice Link**: [LeetCode #322: Coin Change](https://leetcode.com/problems/coin-change/)

#### Problem Statement
Given an integer array `coins[]` representing coin denominations and an integer `amount`, return the fewest number of coins needed to make up that amount. If that amount cannot be made up by any combination, return `-1`. You may assume an infinite number of each kind of coin.

#### AI Assistant Cross-Examination & Model Defense
- **Bot Query**: *"Why do we initialize the DP array with `amount + 1` instead of `Integer.MAX_VALUE`?"*
  - **Candidate Defense**: If we initialize with `Integer.MAX_VALUE`, calculating `dp[i - coin] + 1` causes 32-bit signed integer overflow, wrapping around to a negative number (`Integer.MIN_VALUE`). Using `amount + 1` acts as a safe infinity because even using the smallest possible coin denomination (1), the maximum coins needed cannot exceed `amount`.
- **Bot Query**: *"What is the state transition?"*
  - **Candidate Defense**: $\text{dp}[i] = \min(\text{dp}[i], 1 + \text{dp}[i - \text{coin}])$ for all $\text{coin} \le i$.

#### Production-Ready Java Implementation
```java
public class CoinChange {
    public static int coinChange(int[] coins, int amount) {
        if (amount < 0) return -1;
        int max = amount + 1;
        int[] dp = new int[amount + 1];
        java.util.Arrays.fill(dp, max);
        dp[0] = 0; // 0 amount requires 0 coins

        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

### Question 1.2 (AIC-027): Verify Strict Alternating Odd-Even Sequence
**Tag**: [MOCK-EXAM]  
**Frequency**: Directly asked in recent Capgemini assessment sessions (Sept 28 / 29).

#### Problem Statement
Given an integer array `arr[]`, return `true` if elements follow the strict pattern $\text{Odd} \to \text{Even} \to \text{Odd} \to \text{Even} \dots$ or $\text{Even} \to \text{Odd} \to \text{Even} \to \text{Odd} \dots$. Otherwise, return `false`.

#### Edge Cases to Tell the AI
1. Array with fewer than 2 elements is trivially alternating (`true`).
2. Negative numbers: Java's `% 2` returns `-1` for negative odd numbers (e.g., `-3 % 2 == -1`). Always use `(arr[i] & 1)` or `(arr[i] ^ arr[i - 1]) & 1` for parity checks.

#### Production-Ready Java Implementation
```java
public class AlternatingParity {
    public static boolean isAlternating(int[] arr) {
        if (arr == null || arr.length <= 1) return true;

        for (int i = 1; i < arr.length; i++) {
            // If both are even or both are odd, bitwise XOR of their lowest bit will be 0
            if (((arr[i] ^ arr[i - 1]) & 1) == 0) {
                return false;
            }
        }
        return true;
    }
}
```

---

### Question 1.3 (AIC-028): Move All Hashes to Front
**Tag**: [MOCK-EXAM]  
**Frequency**: Classic Capgemini Exceller staple.

#### Problem Statement
Given a string containing `'#'` characters mixed with alphanumeric characters, move all `'#'` characters to the beginning while maintaining the relative order of the remaining characters.
- **Example**: Input: `"Move#Hash#to#Front"` $\to$ Output: `"###MoveHashtoFront"`.

#### Prompting Strategy
Request an in-place or single-pass $O(N)$ solution using `StringBuilder` rather than naive string concatenation. In Java, naive string concatenation creates $O(N^2)$ memory allocations.

#### Production-Ready Java Implementation
```java
public class MoveHashes {
    public static String moveHash(String str, int n) {
        if (str == null || n == 0) return "";
        
        StringBuilder hashes = new StringBuilder();
        StringBuilder letters = new StringBuilder();
        
        for (int i = 0; i < n; i++) {
            char ch = str.charAt(i);
            if (ch == '#') {
                hashes.append('#');
            } else {
                letters.append(ch);
            }
        }
        return hashes.append(letters).toString();
    }
}
```

---

### Question 1.4 (AIC-029): Run-Length Compression (Consecutive Character Count)
**Tag**: [MOCK-EXAM]  
**Frequency**: High (Appears in almost every batch).

#### Problem Statement
Given a string with consecutively repeating characters, compress it such that each character is followed by its run length. If a character appears only once, do not append 1.
- **Example**: `"abbccccc"` $\to$ `"ab2c5"`.

#### Production-Ready Java Implementation
```java
public class RunLengthCompression {
    public static String compress(String s) {
        if (s == null || s.length() == 0) return "";
        
        StringBuilder sb = new StringBuilder();
        int n = s.length();
        int count = 1;
        
        for (int i = 0; i < n; i++) {
            if (i + 1 < n && s.charAt(i) == s.charAt(i + 1)) {
                count++;
            } else {
                sb.append(s.charAt(i));
                if (count > 1) {
                    sb.append(count);
                }
                count = 1; // Reset counter
            }
        }
        return sb.toString();
    }
}
```

---

## 8. Actionable Playbook for the Capgemini Assessment

```text
                      EXAM STRATEGY CHECKLIST
                                 │
     ┌───────────────────────────┼───────────────────────────┐
     ▼                           ▼                           ▼
[Minute 00-02]              [Minute 02-06]              [Minute 06-10]
Classify & Formalize        Drive Bot Interaction       Audit, Optimize & Run
- Identify DP pattern       - Use Prompt Template       - Verify O(1) space
- Write down constraints    - Answer bot questions      - Dry run edge cases
- Note edge cases (0s)      - Request strict code       - Execute test suite
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
- [ ] The solution handles an initial `'0'` immediately (`s.charAt(0) == '0' -> return 0`).
- [ ] Space complexity is strictly $O(1)$ using two rolling pointers, not an array of size $N+1$.
- [ ] Substring parsing does not cause `StringIndexOutOfBoundsException` at $i=1$.
- [ ] Early termination triggers if both single-digit and two-digit checks fail (`current == 0`).

---

Previous: [04_graph_bipartite_course_schedule_islands.md](04_graph_bipartite_course_schedule_islands.md) | Next: [06_grid_bfs_shortest_path_obstacles.md](06_grid_bfs_shortest_path_obstacles.md)
