[Home](../README.md) > [10-quick-revision](README.md) > formulas-and-traps.md

# Comprehensive Formulas & Trap Reference

A detailed reference compilation of all critical mathematical identities, bitwise masks, operating systems matrix formulas, and logic rules tested in Capgemini assessments.

## 1. Bitwise Manipulation Formulas

| Identity | Formula | Application |
| :--- | :--- | :--- |
| **Clear Lowest Set Bit** | `x & (x - 1)` | Count set bits in $O(K)$ time (Brian Kernighan) |
| **Isolate Lowest Set Bit** | `x & (-x)` | Extract lowest 1-bit |
| **Power of 2 Test** | `(x > 0) && ((x & (x - 1)) == 0)` | Validates powers of 2 ($1, 2, 4, 8, \dots$) |
| **Two's Complement NOT**| `~x = -(x + 1)` | `~5 = -6`, `~0 = -1`, `~(-1) = 0` |
| **Turn On K-th Bit** | `x \| (1 << k)` | Sets bit at index $k$ to 1 |
| **Turn Off K-th Bit** | `x & ~(1 << k)` | Clears bit at index $k$ to 0 |
| **Toggle K-th Bit** | `x ^ (1 << k)` | Inverts bit at index $k$ |
| **Check K-th Bit** | `(x & (1 << k)) != 0` | Tests if bit at index $k$ is set |
| **Multiply by $2^k$** | `x << k` | `x << 3` equals $8x$ |
| **Divide by $2^k$** | `x >> k` | Arithmetic shift preserves sign bit |
| **XOR Swap** | `a^=b; b^=a; a^=b;` | Swaps $a$ and $b$ without temp variable |

---

## 2. Computer Networks & Subnetting Table

| CIDR Prefix | Subnet Mask | Total Addresses | Usable Hosts ($2^H - 2$) |
| :---: | :--- | :---: | :---: |
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/29` | `255.255.255.248` | 8 | 6 |
| `/30` | `255.255.255.252` | 4 | 2 (Point-to-point links) |

---

## 3. Database Normalization & ACID Rules

- **1NF**: Atomic values only (no repeating groups or multi-valued attributes).
- **2NF**: In 1NF + No partial functional dependencies (all non-prime attributes fully dependent on whole primary key).
- **3NF**: In 2NF + No transitive functional dependencies ($X \to Y$ where $Y$ is non-prime).
- **BCNF**: In 3NF + For every functional dependency $X \to Y$, $X$ must be a superkey.
- **SQL 3-Valued Logic**: `NULL = NULL` is UNKNOWN. `NULL AND FALSE` is FALSE. `NULL OR TRUE` is TRUE.

---

## 4. Operating Systems & Deadlock Formulas

- **Banker's Algorithm Formula**:
  $$\text{Need}[i][j] = \text{Max}[i][j] - \text{Allocation}[i][j]$$
  A state is safe if there exists a process execution sequence where $\text{Need}_i \le \text{Available}$.
- **Coffman Deadlock Conditions**:
  1. Mutual Exclusion
  2. Hold and Wait
  3. No Preemption
  4. Circular Wait

---

## 5. Deductive Reasoning & Syllogism Rules

- **Universal Affirmative (A)**: "All S are P" converts only to "Some P are S" (never "All P are S").
- **Universal Negative (E)**: "No S is P" converts to "No P is S".
- **Particular Affirmative (I)**: "Some S are P" converts to "Some P are S".
  - *Golden Rule*: "Some S are P" does NOT guarantee "Some S are not P".
- **Double Negative Fallacy**: From two negative premises (e.g. "No A is B; No B is C"), NO definite conclusion follows.
- **Complementary Pair (Either/Or)**: Between unlinked subject and predicate:
  1. "Some A are B" AND "No A is B" (I + E)
  2. "All A are B" AND "Some A are not B" (A + O)

---

Previous: [CHEATSHEET.md](CHEATSHEET.md) | Next: [last-24-hours.md](last-24-hours.md)
