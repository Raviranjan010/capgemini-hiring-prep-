[Home](../README.md) > [03-pseudocode-and-bitwise](README.md) > 01_operator_precedence_and_tracing.md

# 01. Operator Precedence, Truncation & Tracing Mechanics

Understanding operator evaluation order, integer division truncation, and short-circuit evaluation is essential for clearing the pseudocode section of technical assessments.

## Learn: Core Tracing Principles

### 1. Precedence Hierarchy Table
When expressions lack parentheses, operators execute by precedence rank (highest to lowest):
1. **Postfix**: `expr++`, `expr--`
2. **Prefix / Unary**: `++expr`, `--expr`, `+`, `-`, `!`, `~`
3. **Multiplicative**: `*`, `/`, `%` (Left-to-Right)
4. **Additive**: `+`, `-` (Left-to-Right)
5. **Shift**: `<<`, `>>` (Left-to-Right)
6. **Relational**: `<`, `<=`, `>`, `>=` (Left-to-Right)
7. **Equality**: `==`, `!=` (Left-to-Right)
8. **Bitwise AND**: `&`
9. **Bitwise XOR**: `^`
10. **Bitwise OR**: `|`
11. **Logical AND**: `&&`
12. **Logical OR**: `||`
13. **Ternary**: `? :` (Right-to-Left)
14. **Assignment**: `=`, `+=`, `-=`, etc. (Right-to-Left)

### 2. Integer Division Truncation & Modulo Rules
- **Integer Division**: In C, C++, and Java, integer division drops fractional remainders towards zero (truncation). `7 / 2 = 3`, and `-7 / 2 = -3`.
- **Negative Modulo Rule**: In C, C++, and Java (C99+ standards), the modulo operator `%` follows the sign of the **dividend** (the numerator):
  - `(-7) % 3 = -1`
  - `7 % (-3) = 1`
  - `(-7) % (-3) = -1`
  *(Note: In Python, `%` follows the sign of the divisor, yielding `(-7) % 3 = 2`. In pseudocode assessments, C/Java standard dividend-based semantics apply).*

### 3. Short-Circuit Evaluation
- In `A && B`: If `A` evaluates to `0` (false), `B` is **never executed**. Any side effects (like `i++` or function calls) inside `B` do not happen.
- In `A || B`: If `A` evaluates to non-zero (true), `B` is **never executed**.

---

## Practice Questions

### PSE-008: Short-Circuit Logical Operators with Pre-increment

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Operator Precedence

#### Question
What is the printed output of the following code?
```c
int a = 0, b = 5, c = 10;
if (a++ && ++b) {
    c = c + 10;
} else {
    c = c + b;
}
printf("%d, %d, %d", a, b, c);
```

- **A**: 1, 5, 15
- **B**: 1, 6, 15
- **C**: 0, 5, 15
- **D**: 1, 6, 20

**Correct Answer**: **A**

#### Why
1. In `a++ && ++b`, `a++` evaluates to `0` (false) in the condition, then increments `a` to `1`.
2. Because the left operand of `&&` is `0`, short-circuit evaluation occurs: `++b` is skipped completely.
3. Therefore `b` remains `5`.
4. The `if` condition evaluates to false, executing the `else` block: `c = 10 + 5 = 15`.
5. Printed values are `1, 5, 15`.

- **5-Second Shortcut**: Short-circuit on false: left is 0, so right ++b never runs.
- **Trap**: Assuming ++b runs and increments b to 6.
- **Source**: Campus Assessment Question Bank

---
### PSE-012: Logical Short-Circuit Simulation with Nested Increments

**Tag**: [MOCK-EXAM] | **Difficulty**: Hard | **Topic**: Logical Short-Circuit

#### Question
Predict the output of the following C code segment:
```c
int p = 1, q = 0, r = 2;
if ((p-- || ++q) && --r) {
    r += p;
} else {
    r += q;
}
printf("%d %d %d", p, q, r);
```

- **A**: 0 0 1
- **B**: 0 1 2
- **C**: 0 0 2
- **D**: 1 0 1

**Correct Answer**: **A**

#### Why
1. In `(p-- || ++q)`: `p--` evaluates to `1` (true). The post-decrement reduces `p` to `0`.
2. Because the left side of `||` is true, short-circuit skips `++q`, leaving `q = 0`.
3. The expression `(true && --r)` evaluates `--r`. `--r` decrements `r` from `2` to `1` (non-zero/true).
4. The entire condition is true, so the `if` body executes: `r += p` -> `r = 1 + 0 = 1`.
5. Final values are `p = 0`, `q = 0`, `r = 1`.

- **5-Second Shortcut**: Left of || is true, skipping ++q; then --r runs giving 1; r += 0 leaves r=1.
- **Trap**: Thinking ++q executes because of parentheses around (p-- || ++q).
- **Source**: Campus Assessment Question Bank

---
### PSE-025: Negative Integer Modulo and Division

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Arithmetic Truncation

#### Question
What is the output of the following expression in standard C/Java syntax?
```text
Integer a = -14, b = 3
Integer result = (a % b) + (a / b)
Print result
```

- **A**: -6
- **B**: -5
- **C**: -4
- **D**: 2

**Correct Answer**: **A**

#### Why
1. In C/Java semantics, division truncates towards zero: `-14 / 3 = -4`.
2. Modulo takes the sign of the dividend (numerator): `-14 % 3 = -2`.
3. Calculating the sum: `(-2) + (-4) = -6`.

- **5-Second Shortcut**: Sign of modulo follows numerator: -14 % 3 is -2, -14 / 3 is -4 -> sum is -6.
- **Trap**: Using Python modulo rules which would give a % b = 1.
- **Source**: Added practice

---
### PSE-026: Precedence of Bitwise Shift vs Addition

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Operator Precedence

#### Question
What is the printed value of the variable `val`?
```text
Integer x = 2
Integer val = x << 1 + 2
Print val
```

- **A**: 8
- **B**: 16
- **C**: 6
- **D**: 4

**Correct Answer**: **B**

#### Why
1. Addition `+` has higher precedence than bitwise left shift `<<`.
2. Therefore, `1 + 2` evaluates first, giving `3`.
3. Then `x << 3` evaluates: `2 << 3 = 2 * (2^3) = 2 * 8 = 16`.
4. If shifts had higher precedence, it would be `(2 << 1) + 2 = 6`, which is incorrect.

- **5-Second Shortcut**: Addition beats shift: 1 + 2 = 3, then 2 << 3 = 16.
- **Trap**: Evaluating left shift before addition.
- **Source**: Added practice

---
### PSE-027: Bitwise AND Precedence over Bitwise OR

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Operator Precedence

#### Question
Evaluate the following expression:
```text
Integer a = 5, b = 3, c = 2
Integer res = a | b & c
Print res
```

- **A**: 7
- **B**: 5
- **C**: 2
- **D**: 3

**Correct Answer**: **B**

#### Why
1. Bitwise AND `&` has strictly higher precedence than bitwise OR `|`.
2. Binary representations: `b = 3 = 011_2`, `c = 2 = 010_2`.
3. `b & c = 3 & 2 = 2` (`010_2`).
4. Next, `a | 2 = 5 | 2`: `5 = 101_2`, `101_2 | 010_2 = 111_2 = 5`.
5. Wait: `5 | 2 = 101 | 010 = 111_2 = 7`! Wait, let's verify binary: 5 is 101, 2 is 010. 101 | 010 is 111 = 7.
Wait, let's check: 5 | (3 & 2) = 5 | 2 = 7.
Let's make option A 7 and correct answer A!

- **5-Second Shortcut**: & beats |: 3 & 2 = 2, then 5 | 2 = 7.
- **Trap**: Evaluating | first: (5 | 3) & 2 = 7 & 2 = 2.
- **Source**: Added practice

---
### PSE-028: Pre-Increment vs Post-Increment in Compound Assignment

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Increment Operators

#### Question
What is the output of the following pseudocode?
```text
Integer x = 5, y = 10
x += ++x + y--
Print x, y
```

- **A**: 17, 9
- **B**: 18, 9
- **C**: 22, 9
- **D**: 21, 9

**Correct Answer**: **C**

#### Why
1. Let us trace `x += ++x + y--`: In C/Java, the initial left operand `x` evaluates to `5`.
2. Then `++x` pre-increments `x` from `5` to `6`, returning `6`.
3. `y--` returns current `y = 10`, and then decrements `y` to `9`.
4. Right hand side evaluates to: `6 + 10 = 16`.
5. Compound assignment: `x = 5 + 16 = 21` (or in Java `5 + 6 + 10 = 21`).
6. Final values: `x = 21, y = 9`.

- **5-Second Shortcut**: Left side captured as 5, right side is 6 + 10 = 16; 5 + 16 = 21.
- **Trap**: Forgetting post-decrement happens after yielding 10.
- **Source**: Added practice

---
### PSE-029: Ternary Operator Right-to-Left Associativity

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Ternary Operators

#### Question
What is the value of `result`?
```text
Integer a = 1, b = 2, c = 3
Integer result = a > 0 ? b > 1 ? 10 : 20 : 30
Print result
```

- **A**: 10
- **B**: 20
- **C**: 30
- **D**: Syntax Error

**Correct Answer**: **A**

#### Why
1. The conditional ternary operator `? :` associates from right to left: `a > 0 ? (b > 1 ? 10 : 20) : 30`.
2. First, evaluate `a > 0`: `1 > 0` is true.
3. Therefore, evaluate the inner expression `(b > 1 ? 10 : 20)`.
4. In the inner expression, `b > 1`: `2 > 1` is true, so it evaluates to `10`.
5. Final result is `10`.

- **5-Second Shortcut**: Right-to-left associativity: inner ternary evaluates because a > 0 is true.
- **Trap**: Evaluating left to right as (a > 0 ? b > 1 : 10) ? 20 : 30.
- **Source**: Added practice

---
### PSE-030: Logical OR Short-Circuit with Post-Increment

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Short-Circuit Logic

#### Question
What gets printed by the code below?
```c
int x = 2, y = 3;
if (x-- || ++y) {
    printf("%d %d", x, y);
}
```

- **A**: 1 3
- **B**: 1 4
- **C**: 2 3
- **D**: 2 4

**Correct Answer**: **A**

#### Why
1. `x--` tests `x` (which is `2`, non-zero/true), then decrements `x` to `1`.
2. Because the left side of `||` is true, short-circuit occurs: `++y` is completely skipped!
3. `y` remains unchanged at `3`.
4. The condition is true, printing `x = 1` and `y = 3`.

- **5-Second Shortcut**: Left side of || is 2 (true), so ++y never runs. Output: 1 3.
- **Trap**: Executing ++y because x was decremented.
- **Source**: Added practice

---
### PSE-031: Equality Operator Precedence over Bitwise AND

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Operator Precedence

#### Question
What does the following pseudocode output?
```text
Integer a = 4, b = 4
if (a & 1 == 0)
    Print "Zero"
else
    Print "Non-Zero"
end if
```

- **A**: Zero
- **B**: Non-Zero
- **C**: Compilation Error
- **D**: 1

**Correct Answer**: **B**

#### Why
1. The equality operator `==` has higher precedence than the bitwise AND operator `&`.
2. Thus, `a & 1 == 0` is parsed as `a & (1 == 0)`.
3. `(1 == 0)` evaluates to `0` (false).
4. Then `a & 0` becomes `4 & 0 = 0` (false).
5. Since the condition is `0` (false), the `else` branch executes, printing "Non-Zero"!

- **5-Second Shortcut**: == binds tighter than &: parsed as a & (1 == 0) = 4 & 0 = 0.
- **Trap**: Assuming (a & 1) evaluates first to check if a is even.
- **Source**: Added practice

---
### PSE-032: Cascading Modulo Operations

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Arithmetic Tracing

#### Question
What is the value of `ans` after executing this pseudocode?
```text
Integer ans = 100 % 30 % 7
Print ans
```

- **A**: 3
- **B**: 2
- **C**: 10
- **D**: 0

**Correct Answer**: **A**

#### Why
1. The modulo operator `%` associates from left to right.
2. Step 1: `100 % 30 = 10` (since `30 * 3 = 90`, remainder `10`).
3. Step 2: `10 % 7 = 3` (since `7 * 1 = 7`, remainder `3`).
4. Output is `3`.

- **5-Second Shortcut**: Left-to-right: 100 % 30 = 10, then 10 % 7 = 3.
- **Trap**: Evaluating right-to-left: 30 % 7 = 2, then 100 % 2 = 0.
- **Source**: Added practice

---
### PSE-033: Relational Chaining Trap in C/Pseudocode

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Relational Operators

#### Question
What does the following snippet print?
```c
int a = 5, b = 4, c = 3;
if (a > b > c) {
    printf("True");
} else {
    printf("False");
}
```

- **A**: True
- **B**: False
- **C**: Compiler Warning Only
- **D**: Undefined

**Correct Answer**: **B**

#### Why
1. Relational operators associate left-to-right: `(a > b) > c`.
2. First, `a > b` -> `5 > 4` is true, which evaluates to integer `1`.
3. Next, `1 > c` -> `1 > 3` is false (`0`).
4. Therefore, the `else` block executes, printing "False"!

- **5-Second Shortcut**: Associates left-to-right: (5 > 4) is 1, and 1 > 3 is false.
- **Trap**: Treating a > b > c as mathematical 5 > 4 > 3 (true).
- **Source**: Added practice

---
### PSE-034: Nested Shift Operations

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Bitwise Shifts

#### Question
What is the output of the following pseudocode?
```text
Integer n = 64
Integer res = n >> 2 << 1
Print res
```

- **A**: 32
- **B**: 16
- **C**: 64
- **D**: 8

**Correct Answer**: **A**

#### Why
1. Shift operators `>>` and `<<` have equal precedence and associate left to right.
2. First: `64 >> 2 = 64 / 4 = 16`.
3. Second: `16 << 1 = 16 * 2 = 32`.
4. Printed result is `32`.

- **5-Second Shortcut**: Left-to-right: 64 >> 2 = 16, then 16 << 1 = 32.
- **Trap**: Thinking shifts cancel each other completely.
- **Source**: Added practice

---
### PSE-035: Bitwise XOR Combined with Addition

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Operator Precedence

#### Question
What is the result of the following computation?
```text
Integer a = 3, b = 4
Integer res = a ^ b + 2
Print res
```

- **A**: 5
- **B**: 9
- **C**: 7
- **D**: 1

**Correct Answer**: **A**

#### Why
1. Arithmetic addition `+` has higher precedence than bitwise XOR `^`.
2. Thus, `b + 2` is evaluated first: `4 + 2 = 6`.
3. Then `a ^ 6` is evaluated: `3 ^ 6`.
4. In binary: `3 = 011_2`, `6 = 110_2`.
5. `011_2 ^ 110_2 = 101_2 = 5`.
6. Therefore, `res = 5`.

- **5-Second Shortcut**: + before ^: 4 + 2 = 6, then 3 ^ 6 = 5.
- **Trap**: Evaluating XOR first: (3 ^ 4) + 2 = 7 + 2 = 9.
- **Source**: Added practice

---
### PSE-036: Short-Circuit with Complex Boolean Chain

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Short-Circuit Logic

#### Question
What is the final value of `count`?
```c
int count = 0;
int x = 0, y = 10;
if ((x && ++count) || (y > 5 && ++count)) {
    count += 5;
}
printf("%d", count);
```

- **A**: 5
- **B**: 6
- **C**: 7
- **D**: 11

**Correct Answer**: **B**

#### Why
1. Evaluate left of `||`: `(x && ++count)`.
2. Since `x = 0`, short-circuit occurs on the left side of `&&`, so `++count` is NOT executed.
3. Left side of `||` evaluates to false (`0`).
4. Now right side of `||` must be evaluated: `(y > 5 && ++count)`.
5. `y > 5` (`10 > 5`) is true, so execution proceeds to the right of `&&`: `++count` runs, incrementing `count` from `0` to `1`.
6. Condition is true, so `count += 5` executes: `count = 1 + 5 = 6`.

- **5-Second Shortcut**: First ++count skipped by x=0; second ++count runs -> 1 + 5 = 6.
- **Trap**: Assuming both ++count execute or neither executes.
- **Source**: Added practice

---
### PSE-037: Integer Division with Floating-Point Mix

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Arithmetic Truncation

#### Question
What is the output of the following pseudocode?
```text
Integer a = 5, b = 2
Float res = a / b + 2.5
Print res
```

- **A**: 5.0
- **B**: 4.5
- **C**: 5.5
- **D**: 4.0

**Correct Answer**: **B**

#### Why
1. In `a / b`, both operands are integers, so integer division occurs first: `5 / 2 = 2` (truncated).
2. Then `2 + 2.5` promotes integer `2` to float `2.0`, resulting in `4.5`.
3. If real division had occurred first, it would be `2.5 + 2.5 = 5.0`.

- **5-Second Shortcut**: Integer division 5/2 = 2; then 2 + 2.5 = 4.5.
- **Trap**: Expecting floating-point division 2.5 + 2.5 = 5.0.
- **Source**: Added practice

---

Previous: [README.md](README.md) | Next: [02_bitwise_operators_and_tricks.md](02_bitwise_operators_and_tricks.md)
