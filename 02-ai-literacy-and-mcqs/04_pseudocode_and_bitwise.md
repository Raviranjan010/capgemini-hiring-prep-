# Pseudocode & Bitwise Operator Mechanics

## Core Bitwise & Pseudocode Mechanics

### Master Operator Precedence & Associativity Table (C, C++, Java)

In C, C++, and Java, expression parsing depends strictly on precedence and associativity:

| Precedence Rank | Operators | Description | Associativity |
| :---: | :--- | :--- | :---: |
| **1 (Highest)** | `()`, `[]`, `->`, `.` | Grouping, Array subscript, Member access | Left-to-Right |
| **2** | `++`, `--`, `!`, `~`, `+`, `-` (unary), `(type)`, `*` (deref), `&` (addr), `sizeof` | Unary pre-increment/decrement, logic negation, bitwise NOT | Right-to-Left |
| **3** | `*`, `/`, `%` | Multiplicative (Multiplication, Division, Modulo) | Left-to-Right |
| **4** | `+`, `-` | Additive (Addition, Subtraction) | Left-to-Right |
| **5** | `<<`, `>>` | Bitwise Shift Left, Bitwise Shift Right | Left-to-Right |
| **6** | `<`, `<=`, `>`, `>=` | Relational operators | Left-to-Right |
| **7** | `==`, `!=` | Equality operators | Left-to-Right |
| **8** | `&` | Bitwise AND | Left-to-Right |
| **9** | `^` | Bitwise XOR | Left-to-Right |
| **10** | `\|` | Bitwise OR | Left-to-Right |
| **11** | `&&` | Logical AND (Short-circuit) | Left-to-Right |
| **12** | `\|\|` | Logical OR (Short-circuit) | Left-to-Right |
| **13 (Lowest)** | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `<<=`, `>>=`, `&=`, `^=`, `\|=` | Assignment operators | Right-to-Left |

> [!IMPORTANT]
> **Key Spotting Rules**:
> - Multiplicative (`*`, `/`, `%`) binds tighter than Additive (`+`, `-`).
> - Additive (`+`, `-`) binds tighter than Bitwise Shifts (`<<`, `>>`).
> - Relational / Equality (`<`, `==`) binds tighter than Bitwise Logic (`&`, `^`, `|`).
> - Bitwise AND (`&`) > Bitwise XOR (`^`) > Bitwise OR (`|`).

---

### High-Frequency Bit Manipulation Tricks

| Trick | Formula | Binary Meaning & Quick Application |
| :--- | :--- | :--- |
| **Isolate Rightmost Set Bit** | `n & (-n)` | Isolates the lowest power-of-2 bit set to `1` using two's complement. |
| **Clear Rightmost Set Bit** | `n & (n - 1)` | Turns off the lowest set bit. If `(n & (n - 1)) == 0` for $n > 0$, $n$ is a power of 2. |
| **Self XOR Cancellation** | `x ^ x = 0` | A number XORed with itself evaluates to `0` (cancels duplicates in arrays). |
| **XOR Identity** | `x ^ 0 = x` | A number XORed with `0` preserves its value unchanged. |
| **In-Place Swap** | `a ^= b; b ^= a; a ^= b;` | Swaps two integer variables without requiring temporary storage. |

---

## High-Yield Questions from Assessment

### Question 1: Operator Precedence & Bit Shifts
**Tag**: [CHAT]

**Pseudocode**:
```text
Integer a = 3, b = 2
Integer res = a << b + 1
Print res
```

**Options**:
- **A)** 13
- **B)** 24
- **C)** 7
- **D)** 12

**Correct Answer**: Option B

**Hand-Verified Trace**:
1. Check operator precedence: Arithmetic addition (`+`) has higher precedence (Rank 4) than bitwise left shift (`<<`, Rank 5).
2. Therefore, `b + 1` evaluates first: $2 + 1 = 3$.
3. Next, `a << 3` evaluates: $3 \ll 3$.
4. Bit shift calculation: $3 \times 2^3 = 3 \times 8 = \mathbf{24}$.

**Why**:  
Arithmetic operators (`+`, `-`) bind tighter than bitwise shift operators (`<<`, `>>`). The expression evaluates as `a << (b + 1)` rather than `(a << b) + 1`.

**5-Second Shortcut**: Arithmetic `+` beats bitwise shift `<<` $\implies 3 \ll (2 + 1) = 3 \times 8 = 24$.  
**Trap**: Evaluating left-to-right as `(3 << 2) + 1 = (12) + 1 = 13`.

---

### Question 2: Static Recursion Unwinding
**Tag**: [CHAT]

**Pseudocode**:
```text
Integer fun(Integer n)
    Static Integer x = 0
    if (n <= 0) 
        return 1
    End if
    x = x + 1
    return fun(n - 1) + x
End function
```

**Question**:  
What integer value is returned by the initial function call `fun(3)`?

- **A)** 7
- **B)** 10
- **C)** 6
- **D)** 12

**Correct Answer**: Option B

**Hand-Verified Step-by-Step Trace**:
1. **Winding Phase (Stack Push & State Update)**:
   - Initial call `fun(3)`: `x` incremented to $1$ ($x = 1$); pauses to evaluate `fun(2) + x`.
   - Call `fun(2)`: `x` incremented to $2$ ($x = 2$); pauses to evaluate `fun(1) + x`.
   - Call `fun(1)`: `x` incremented to $3$ ($x = 3$); pauses to evaluate `fun(0) + x`.
   - Base call `fun(0)`: Condition $n \le 0$ matches; returns `1`.
2. **Memory State**:
   - Because `x` is declared `Static`, a single shared memory location is used across all recursive invocations. At the end of winding, $x = 3$.
3. **Unwinding Phase (Stack Pop & Evaluation)**:
   - `fun(1)` unwinds: $\text{fun}(0) + x = 1 + 3 = \mathbf{4}$.
   - `fun(2)` unwinds: $\text{fun}(1) + x = 4 + 3 = \mathbf{7}$.
   - `fun(3)` unwinds: $\text{fun}(2) + x = 7 + 3 = \mathbf{10}$.

**Why**:  
Local variables get distinct copies per stack frame, but static variables persist across all recursive calls in the data segment. During unwinding, every returning level adds the final accumulated value of $x$ ($3$), not the intermediate value at the time the call was made.

**5-Second Shortcut**: Find final static $x = 3$; base returns $1$; unwinds 3 times: $1 + 3 + 3 + 3 = 10$.  
**Trap**: Adding dynamic local values ($1 + 3 + 2 + 1 = 7$) instead of the static shared value $3$.

---

### Question 3: Isolating Rightmost Set Bit
**Tag**: [CHAT]

**Pseudocode**:
```text
Integer n = 40
Integer res = n & (-n)
Print res
```

**Options**:
- **A)** 0
- **B)** 8
- **C)** 32
- **D)** 1

**Correct Answer**: Option B

**Hand-Verified Trace**:
1. Positive integer $n = 40$ in 8-bit binary:
   $40 = 32 + 8 = 00101000_2$.
2. Negative integer $-40$ using two's complement representation:
   - Invert all bits: $\sim 40 = 11010111_2$.
   - Add $1$: $-40 = 11010111_2 + 1 = 11011000_2$.
3. Bitwise AND:
   ```text
     0010 1000  ( 40)
   & 1101 1000  (-40)
   ------------------
     0000 1000  (  8 in decimal)
   ```

**Why**:  
In two's complement, negating a number inverts all bits up to the rightmost set bit, keeps the rightmost set bit as `1`, and inverts all bits to its left. Performing `n & (-n)` zeroes out all bits except the lowest set bit, isolating it.

**5-Second Shortcut**: The lowest set bit in $40$ ($32 + 8$) is $8 = 2^3$, so $40 \ \& \ (-40) = 8$.  
**Trap**: Selecting 0 assuming positive and negative bitwise AND cancel each other out.

---

### Question 4: Dynamic Rule Evaluation & Arithmetic Operator Precedence
**Tag**: [MOCK-EXAM]

**Snippet**:
```cpp
int a = 10, b = 5, c = 2, d = 4;
int result = a + b * c / d - a % c;
```

**Question**:  
During testing, a junior developer assumed that the expression would evaluate strictly from left to right. What will be the actual result produced by a standard compilation engine?

- **A)** 2
- **B)** 12
- **C)** 10
- **D)** 0

**Correct Answer**: Option B

**Step-by-Step Evaluation**:
1. Multiplicative operators (`*`, `/`, `%`) take precedence (Rank 3) over addition and subtraction (`+`, `-`, Rank 4).
2. Within the same precedence level, associativity is Left-to-Right:
   - `b * c` $\implies 5 \times 2 = 10$.
   - `10 / d` $\implies 10 / 4 = 2$ (integer division truncates decimal part).
   - `a % c` $\implies 10 \% 2 = 0$.
3. Substitute back into the expression:
   $$\text{result} = a + 2 - 0 = 10 + 2 - 0 = \mathbf{12}$$

**5-Second Shortcut**: Group multiplicative blocks first: $10 + (5 \times 2 / 4) - (10 \% 2) = 10 + 2 - 0 = 12$.  
**Trap**: Evaluating left to right: $(10 + 5) \times 2 / 4 \dots = 30 / 4 = 7 \dots$

---

### Question 5: Compound Bitwise & Shift Precedence
**Tag**: [MOCK-EXAM]

**Snippet**:
```cpp
int x = 8, y = 3;
int out = x ^ y + x & y << 1;
cout << out;
```

**Question**:  
What is the terminal output of this C++ snippet?

- **A)** 0
- **B)** 10
- **C)** 14
- **D)** 8

**Correct Answer**: Option B (or D depending on standard operator grouping: evaluates to 10)

**Step-by-Step Evaluation**:
1. Operator Precedence Order:
   - Bitwise Shift (`<<`, Rank 5)
   - Additive (`+`, Rank 4)
   - Bitwise AND (`&`, Rank 8)
   - Bitwise XOR (`^`, Rank 9)
2. Sub-expression evaluations:
   - `y << 1` $\implies 3 \ll 1 = 6$.
   - `y + x` $\implies 3 + 8 = 11$.
3. Expression reduces to: `x ^ 11 & 6`.
4. Bitwise AND (`&`) has higher precedence than XOR (`^`):
   - `11 & 6` $\implies (1011)_2 \ \& \ (0110)_2 = (0010)_2 = 2$.
5. Bitwise XOR (`^`):
   - `x ^ 2` $\implies 8 \oplus 2 = (1000)_2 \oplus (0010)_2 = (1010)_2 = \mathbf{10}$.

**5-Second Shortcut**: `<<` first ($6$), `+` second ($11$), `&` third ($11 \& 6 = 2$), `^` last ($8 \oplus 2 = 10$).  
**Trap**: Evaluating `^` before `&` or assuming `+` binds after bit shifts.

---

### Question 6: Stack Postfix Evaluation
**Tag**: [MOCK-EXAM]

**Question**:  
What is the terminal value computed by parsing the postfix token stream using an evaluation stack:
$$\text{Tokens: } [12, 4, /, 3, *, 7, 2, -, +]$$

- **A)** 14
- **B)** 12
- **C)** 18
- **D)** 9

**Correct Answer**: Option A

**Evaluation Stack Trace**:
1. Push `12`, Push `4` $\implies$ Stack: `[12, 4]`
2. Token `/`: Pop `4`, Pop `12`. Compute $12 / 4 = 3$. Push `3` $\implies$ Stack: `[3]`
3. Push `3` $\implies$ Stack: `[3, 3]`
4. Token `*`: Pop `3`, Pop `3`. Compute $3 \times 3 = 9$. Push `9` $\implies$ Stack: `[9]`
5. Push `7`, Push `2` $\implies$ Stack: `[9, 7, 2]`
6. Token `-`: Pop `2`, Pop `7`. Compute $7 - 2 = 5$. Push `5` $\implies$ Stack: `[9, 5]`
7. Token `+`: Pop `5`, Pop `9`. Compute $9 + 5 = 14$. Push `14` $\implies$ Stack: `[14]`

**Terminal Result**: `14`

**5-Second Shortcut**: Sub-evaluations: $(12 / 4) \times 3 + (7 - 2) = (3 \times 3) + 5 = 9 + 5 = 14$.  
**Trap**: Popping in wrong order for subtraction/division (e.g., $2 - 7$ instead of $7 - 2$).

---

### Question 7: Binary Tree Traversal Reconstruction
**Tag**: [MOCK-EXAM]

**Given**:
- Inorder: `[D, B, E, A, F, C]`
- Preorder: `[A, B, D, E, C, F]`

**Question**:  
What is the corresponding Postorder sequence?

- **A)** D, E, B, F, C, A
- **B)** D, B, E, F, C, A
- **C)** E, D, B, F, C, A
- **D)** A, B, D, E, C, F

**Correct Answer**: Option A

**Step-by-Step Derivation**:
1. **Root Identification**: The first element in Preorder is always the root: `A`.
2. **Subtree Partitioning**:
   - Locate `A` in Inorder:
     - Left Subtree Inorder: `[D, B, E]`
     - Right Subtree Inorder: `[F, C]`
3. **Left Subtree Construction**:
   - Preorder for left subtree: `[B, D, E]` $\implies$ Root is `B`.
   - Inorder `[D, B, E]` $\implies$ Left is `D`, Right is `E`.
   - Postorder of left subtree (Left $\to$ Right $\to$ Root): `D, E, B`.
4. **Right Subtree Construction**:
   - Preorder for right subtree: `[C, F]` $\implies$ Root is `C`.
   - Inorder `[F, C]` $\implies$ Left is `F`, Right is empty.
   - Postorder of right subtree: `F, C`.
5. **Combine Full Postorder**:
   $$\text{Left Subtree} + \text{Right Subtree} + \text{Root} = \mathbf{D, E, B, F, C, A}$$

**5-Second Shortcut**: Root `A` must be the final element in Postorder. Between A and B, `D` and `E` are children of `B`, so `D, E, B` precedes `B`.  
**Trap**: Confusing Preorder traversal with Inorder boundary slicing.

---

### Question 8: Pseudo-code Loop Invariant & Hamming Weight
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
function solve(n):
    count = 0
    while n > 0:
        if n % 2 != 0:
            count = count + 1
        n = n / 2
    return count
```

**Question**:  
What is the return value of `solve(40)`?

- **A)** 1
- **B)** 2
- **C)** 3
- **D)** 4

**Correct Answer**: Option B

**Explanation & Binary Trace**:
1. Notice what the algorithm does: `n % 2 != 0` checks if the lowest bit is `1`, and `n = n / 2` shifts $n$ right by 1 bit (`n >>= 1`).
2. This is the classic algorithm to count the number of set bits (Hamming Weight) in the binary representation of $n$.
3. Express $n = 40$ in binary:
   $$40 = 32 + 8 = (101000)_2$$
4. Number of `1` bits in $(101000)_2$ is exactly **2**.

**5-Second Shortcut**: $40 = 32 + 8 \implies 2$ set bits $\implies \text{count} = 2$.  
**Trap**: Manually tracing 6 iterations and miscounting integer division.

---

### Question 9: Power of Two Identification
**Tag**: [ADDED]

**Question**:  
Which bitwise expression returns `true` if and only if a positive non-zero integer $n$ is an exact power of two?

- **A)** `(n & (n + 1)) == 0`
- **B)** `(n & (n - 1)) == 0`
- **C)** `(n ^ (n - 1)) == 0`
- **D)** `(n | (n - 1)) == n`

**Correct Answer**: Option B

**Why**:  
Powers of two have exactly one bit set to `1` in binary (e.g., $16 = 10000_2$). Subtracting $1$ flips that single bit to `0` and sets all lower bits to `1` ($15 = 01111_2$). Performing bitwise AND between $n$ and $n-1$ produces zero if and only if $n$ has a single set bit.

**5-Second Shortcut**: Power of 2 check = `(n & (n - 1)) == 0`.  
**Trap**: Choosing `(n & (n + 1)) == 0`, which fails for powers of 2.

---

### Question 10: Operator Precedence Trap: Bitwise AND vs Equality
**Tag**: [ADDED]

**Pseudocode**:
```text
Integer x = 4
if (x & 1 == 0)
    Print "Branch A"
else
    Print "Branch B"
End if
```

**Question**:  
What is the printed output of this code snippet in C, C++, or Java?

- **A)** Branch A
- **B)** Branch B
- **C)** Compilation / Type Error
- **D)** Runtime Exception

**Correct Answer**: Option B

**Why**:  
Relational equality (`==`, Rank 7) has higher operator precedence than bitwise AND (`&`, Rank 8). Therefore, `1 == 0` is evaluated first, which yields `0` (or `false`). Then `x & 0` evaluates to `0`, which triggers the `else` branch, printing "Branch B". (In Java, this causes a compile type-mismatch error; in C/C++, it prints "Branch B").

**5-Second Shortcut**: `==` binds tighter than `&`; `x & 1 == 0` evaluates as `x & (1 == 0)`.  
**Trap**: Assuming `(x & 1)` evaluates first to check if the number is even.

---

### Question 11: Unique Element Isolation via XOR
**Tag**: [ADDED]

**Question**:  
Given an array `[4, 1, 2, 1, 2]`, what is the final value of variable `res` after the loop completes?

```text
Integer arr = [4, 1, 2, 1, 2]
Integer res = 0
For each val in arr
    res = res ^ val
End for
```

- **A)** 0
- **B)** 4
- **C)** 10
- **D)** 2

**Correct Answer**: Option B

**Why**:  
XOR is commutative and associative. Duplicates cancel each other out ($x \oplus x = 0$), and any number XORed with zero remains unchanged ($x \oplus 0 = x$). Thus: $0 \oplus 4 \oplus 1 \oplus 2 \oplus 1 \oplus 2 = 4 \oplus (1 \oplus 1) \oplus (2 \oplus 2) = 4 \oplus 0 \oplus 0 = 4$.

**5-Second Shortcut**: All pairs cancel to 0 via XOR; only the unique element ($4$) survives.  
**Trap**: Adding elements or expecting a non-zero parity sum.

---

### Question 12: Efficient Multiplication via Shift Operators
**Tag**: [ADDED]

**Question**:  
Which bitwise expression calculates `7 * n` using only bit shifts and subtraction?

- **A)** `(n << 3) - n`
- **B)** `(n << 2) + n`
- **C)** `(n << 3) + n`
- **D)** `(n >> 3) - n`

**Correct Answer**: Option A

**Why**:  
Left-shifting an integer $n$ by $k$ bits multiplies it by $2^k$. Therefore, `n << 3` equals $n \times 2^3 = 8n$. Subtracting $n$ yields $8n - n = 7n$.

**5-Second Shortcut**: $7n = 8n - n = (n \ll 3) - n$.  
**Trap**: Confusing left shift (multiplication) with right shift (division).
