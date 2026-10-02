# Pseudocode & Bitwise Operator Mechanics

## Core Bitwise & Pseudocode Mechanics

### Mini Table: High-Frequency Bit Manipulation Tricks

| Trick | Formula | Binary Meaning & Quick Application |
| :--- | :--- | :--- |
| **Isolate Rightmost Set Bit** | `n & (-n)` | Isolates the lowest power-of-2 bit set to `1` using two's complement. |
| **Clear Rightmost Set Bit** | `n & (n - 1)` | Turns off the lowest set bit. If `(n & (n - 1)) == 0` for $n > 0$, $n$ is a power of 2. |
| **Self XOR Cancellation** | `x ^ x = 0` | A number XORed with itself evaluates to `0` (cancels duplicates in arrays). |
| **XOR Identity** | `x ^ 0 = x` | A number XORed with `0` preserves its value unchanged. |
| **In-Place Swap** | `a ^= b; b ^= a; a ^= b;` | Swaps two integer variables without requiring temporary storage. |

---

## High-Yield Questions from Chat

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
1. Check operator precedence: Arithmetic addition (`+`) has higher precedence than bitwise left shift (`<<`).
2. Therefore, `b + 1` evaluates first: $2 + 1 = 3$.
3. Next, `a << 3` evaluates: $3 \ll 3$.
4. Bit shift calculation: $3 \times 2^3 = 3 \times 8 = \mathbf{24}$.

**Why**:  
In almost all languages (C, C++, Java) and standard pseudocode conventions, arithmetic operators (`+`, `-`) bind tighter than bitwise shift operators (`<<`, `>>`). The expression evaluates as `a << (b + 1)` rather than `(a << b) + 1`.

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

## Extra Practice (Added MCQs)

### Question 4: Power of Two Identification
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

### Question 5: Operator Precedence Trap: Bitwise AND vs Equality
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
Relational equality (`==`) has higher operator precedence than bitwise AND (`&`). Therefore, `1 == 0` is evaluated first, which yields `0` (or `false`). Then `x & 0` evaluates to `0`, which triggers the `else` branch, printing "Branch B". (In Java, this causes a compile type-mismatch error; in C/C++, it prints "Branch B").

**5-Second Shortcut**: `==` binds tighter than `&`; `x & 1 == 0` evaluates as `x & (1 == 0)`.  
**Trap**: Assuming `(x & 1)` evaluates first to check if the number is even.

---

### Question 6: Unique Element Isolation via XOR
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

### Question 7: Efficient Multiplication via Shift Operators
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
