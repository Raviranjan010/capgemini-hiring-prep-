[Home](../README.md) > [08-recent-patterns](README.md) > 02_cs_and_pseudocode.md

# 02. CS Fundamentals & Pseudocode Reported Patterns

This document analyzes the recurring traps in pseudocode tracing, operator precedence, and core CS questions.

## Reported Pattern Archetypes

### 1. Integer Division Truncation & Negative Modulo
- **Reported In**: Campus Reports & Student Chat
- **Teached By**: `PSE-005`, `PSE-013`, `PSE-018`, `PSE-025`
- **Pattern Details**: In C/Java pseudocode, integer division truncates strictly towards zero: `-14 / 3 = -4`. The modulo operator `%` takes the sign of the dividend (numerator): `(-14) % 3 = -2`. Candidates accustomed to Python's floor modulo rule (`-14 % 3 = 1`) pick incorrect options.
- **Variants**: Nested expressions mixing `/`, `%`, and `*`: `(a % b) + (a / b) * c`.

### 2. Operator Precedence: Bitwise vs Relational vs Arithmetic
- **Reported In**: Campus Assessment Reports
- **Teached By**: `PSE-026`, `PSE-027`, `PSE-031`, `PSE-035`, `BIT-002`
- **Pattern Details**: Arithmetic operators (`+`, `-`, `*`, `/`) have higher precedence than bitwise shifts (`<<`, `>>`), which in turn have higher precedence than relational operators (`<`, `>`), which are higher than equality (`==`), which are higher than bitwise logic (`&`, `^`, `|`), which are higher than logical connectors (`&&`, `||`).
  - *Classic Trap*: `if (a & 1 == 0)` evaluates as `a & (1 == 0) = a & 0 = 0`, not `(a & 1) == 0`.
- **Variants**: `x << 1 + 2` evaluates as `x << 3`, not `(x << 1) + 2`.

### 3. Short-Circuit Evaluation with Unexecuted Side Effects
- **Reported In**: Video Debriefs
- **Teached By**: `PSE-008`, `PSE-012`, `PSE-030`, `PSE-036`
- **Pattern Details**: In `A && B`, if `A` evaluates to `0` (false), `B` is never evaluated. In `A || B`, if `A` evaluates to non-zero (true), `B` is never evaluated. If `B` contains pre/post increments (`++x`, `x--`) or function calls, those side effects do not execute.
- **Variants**: Chained expressions `(p-- || ++q) && --r` where left expression short-circuits, leaving `q` un-incremented.

### 4. Brian Kernighan & Bitmask Twists
- **Reported In**: Gemini Chat
- **Teached By**: `BIT-001`, `BIT-003`, `BIT-007`, `BIT-008`, `BIT-009`
- **Pattern Details**: `x & (x - 1)` clears the lowest set bit. `x & (-x)` isolates the lowest set bit. `(x & (x - 1)) == 0` checks if $x$ is a power of 2.
- **Variants**: Finding Hamming distance via `A ^ B` followed by set bit counting; finding missing numbers via XOR cancellation.

---

Previous: [01_ai_literacy.md](01_ai_literacy.md) | Next: [03_debugging.md](03_debugging.md)
