[Home](../README.md) > 03-pseudocode-and-bitwise

# 03. Pseudocode & Bitwise Mechanics

This module prepares candidates for **Stage 1.3: Pseudocode & Bitwise Assessment** of the Capgemini recruitment drive (typically 15-20 MCQs within 15-20 minutes, or a dedicated 25-30 question round).

---

## Module Overview

Pseudocode questions evaluate your ability to trace procedural control flow, evaluate operator precedence without an IDE, perform mental bit-manipulation, and track recursive call stacks with static/global state mutations.

```mermaid
graph TD
    A[Pseudocode & Bitwise Mastery] --> B[01. Operator Precedence & Tracing]
    A --> C[02. Bitwise Operators & Tricks]
    A --> D[03. Recursion & Loop Mechanics]
    A --> E[04. Pseudocode Question Bank]
    
    B --> B1[Precedence Hierarchy: PUMA]
    B --> B2[Short-Circuit Evaluation: && and ||]
    B --> B3[Truncation & Modulo Signs]
    
    C --> C1[XOR Cancellation: x ^ x = 0]
    C --> C2[Brian Kernighan: x & x-1]
    C --> C3[Shifts: << Multiply, >> Divide]
    
    D --> D1[Call Stack Push/Pop Frames]
    D --> D2[Static vs Local Variable State]
    D --> D3[Head vs Tail Recursion]
    
    E --> E1[Comprehensive 40+ Mixed Drill]
```

---

## Folder Contents & Study Plan

| File | Core Focus | Questions Inside | Est. Time |
| :--- | :--- | :---: | :---: |
| [01_operator_precedence_and_tracing.md](01_operator_precedence_and_tracing.md) | Precedence ranking, integer division truncation, modulo sign rules, short-circuit logic | 12 MCQs | 35 mins |
| [02_bitwise_operators_and_tricks.md](02_bitwise_operators_and_tricks.md) | Bitwise AND, OR, XOR, NOT, left/right shifts, parity checks, power of 2, set-bit counting | 24 MCQs | 45 mins |
| [03_recursion_and_loop_mechanics.md](03_recursion_and_loop_mechanics.md) | Call stack unwinding, static/global persistence, nested recursion, loop boundaries | 15 MCQs | 35 mins |
| [04_pseudocode_question_bank.md](04_pseudocode_question_bank.md) | Full mixed pseudocode question bank matching actual recent exam patterns | 20 MCQs | 40 mins |

---

## 💡 Top 5 Exam-Winning Tricks & Traps

### 1. The Short-Circuit Trap
In expressions with `&&` and `||`:
- `0 && (++x)`: The left operand is `0` (false), so `++x` is **never executed**. `x` does NOT change!
- `1 || (++x)`: The left operand is non-zero (true), so `++x` is **never executed**.

### 2. Modulo Sign Rule (C/C++/Java Standard)
- Modulo `%` takes the sign of the **numerator (dividend)**, never the divisor!
  - `(-11) % 3 = -2`
  - `11 % (-3) = 2`
  - `(-11) % (-3) = -2`

### 3. Bitwise XOR Identity Powerhouse
- `x ^ x = 0` (Self-cancellation)
- `x ^ 0 = x` (Neutral element)
- `x ^ y ^ x = y` (Restoration: used to find unique non-paired elements in $O(1)$ space)

### 4. Brian Kernighan’s Bit-Clearing Formula
- `n & (n - 1)` clears the **lowest set bit (rightmost 1)** of `n`.
- If `n & (n - 1) == 0` (and `n > 0`), then `n` is an **exact power of 2**!
- Loop `while(n) { n &= (n - 1); count++; }` counts total set bits in $O(\text{set bits})$ iterations.

### 5. Static Variable Recursion Trap
- Local `int x = 0`: re-initialized to 0 on every recursive call frame.
- `static int x = 0`: allocated once in data segment; changes persist across all recursive stack pushes and unwinds!

---

## Verification & External Practice
- [Video: Capgemini Technical Assessment Tutorial (Pseudo-code & MCQs)](https://www.youtube.com/watch?v=bjnVDOvgSLk)
- [Video: Capgemini Technical Assessment Part-2 Analysis (Trees, Maps, Recursion)](https://www.youtube.com/watch?v=LqBSkF37FFg)
- [GeeksforGeeks Pseudocode Practice](https://www.geeksforgeeks.org/pseudocode-questions-for-campus-placements/)

---

Previous: [02-cs-fundamentals/README.md](../02-cs-fundamentals/README.md) | Next: [01_operator_precedence_and_tracing.md](01_operator_precedence_and_tracing.md)
