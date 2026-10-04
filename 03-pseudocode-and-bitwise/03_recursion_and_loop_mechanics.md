[Home](../README.md) > [03-pseudocode-and-bitwise](README.md) > 03_recursion_and_loop_mechanics.md

# 03. Recursion Trees, Loop Mechanics & Call Stacks

Tracing execution flow, tracking call stacks, and detecting infinite loops are essential skills tested in technical pseudocode assessments.

## Learn: Core Tracing Techniques

### 1. Loop Boundaries & Step Increments
- **Inclusive vs Exclusive**: In pseudocode, loops typically run inclusive of endpoints (`for i = 1 to n` runs $n$ times).
- **Step Modification**: If the step variable is modified inside the loop body, trace the index carefully at the end of each iteration.
- **Variable Shadowing**: Inner loop block variables with the same name as outer variables hide (shadow) the outer variable for the duration of the block.

### 2. Recursion Call Stack Tracing
- **Base Case**: The condition where recursion halts. Missing base cases cause infinite recursion (Call Stack Overflow).
- **Tree Recursion**: When a function branches into two or more recursive calls (`f(n-1) + f(n-2)`), draw the recursion tree from root to leaves.
- **Static / Global Variables in Recursion**: Static variables maintain their state across all activation frames rather than being re-allocated on each call.

---

## Practice Questions

### PSE-001: Sum of Even Array Elements Tracing

**Tag**: [MOCK-EXAM] | **Difficulty**: Easy | **Topic**: Loop Tracing

#### Question
What is the output of the following pseudocode?
```text
Integer arr[5] = {12, 7, 9, 14, 20}
Integer sum = 0
For each element x in arr:
    If x % 2 == 0 then:
        sum = sum + x
    End If
End For
Print sum
```

- **A**: 46
- **B**: 34
- **C**: 62
- **D**: 26

**Correct Answer**: **A**

#### Why
1. The array contains elements `{12, 7, 9, 14, 20}`.
2. The condition `x % 2 == 0` filters for even numbers.
3. The even numbers are `12`, `14`, and `20`.
4. Adding them: `12 + 14 + 20 = 46`.
5. Printed sum is `46`.

- **5-Second Shortcut**: Filter even numbers and sum: 12 + 14 + 20 = 46.
- **Trap**: Including odd numbers or missing 14.
- **Source**: Campus Assessment Question Bank

---
### PSE-002: Missing Loop Terminator & Infinite Loop Detection

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Loop Mechanics

#### Question
What happens when executing the following pseudocode?
```text
Integer i = 1, sum = 0
While i <= 10 do:
    sum = sum + i
    If sum > 20 then:
        Break
    End If
End While
Print sum
```

- **A**: 21
- **B**: Infinite Loop
- **C**: 55
- **D**: 28

**Correct Answer**: **B**

#### Why
1. In the `While i <= 10` loop, `i` is initialized to `1`.
2. Inside the loop, `sum = sum + i`, but `i` is never incremented (`i++` is missing).
3. In each iteration, `sum` increases by 1 until it hits 21, then breaks... wait!
4. Wait, if `i` is never incremented, `i` remains 1 forever!
5. In each iteration, `sum` increases by `1` (`0 + 1 = 1`, `1 + 1 = 2`, ..., `20 + 1 = 21`).
6. When `sum` reaches `21`, the condition `sum > 20` evaluates to true and executes `Break`!
7. Therefore, the loop terminates after 21 iterations and prints `21`!
8. If the break statement were absent, it would be an infinite loop. But here `Break` exits when `sum > 20`.

- **5-Second Shortcut**: Trace sum: increases by 1 each step until sum > 20 triggers Break at 21.
- **Trap**: Assuming i not incrementing always means infinite loop, ignoring the break condition on sum.
- **Source**: Campus Assessment Question Bank

---
### PSE-007: Nested Loop Complexity & Execution Counter

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Nested Loops

#### Question
What is the printed value of `count`?
```text
Integer count = 0, n = 16
for i = 1 to n step i = i * 2
    for j = 1 to i
        count = count + 1
    end for
end for
Print count
```

- **A**: 16
- **B**: 31
- **C**: 64
- **D**: 32

**Correct Answer**: **B**

#### Why
1. Outer loop steps through powers of 2 up to 16: `i` takes values `1, 2, 4, 8, 16`.
2. For each outer iteration `i`, the inner loop runs exactly `i` times.
3. Total iterations = sum of geometric series:
   `count = 1 + 2 + 4 + 8 + 16 = 31`.
4. Output is `31`.

- **5-Second Shortcut**: Powers of 2 sum: 1 + 2 + 4 + 8 + 16 = 2^5 - 1 = 31.
- **Trap**: Assuming inner loop runs 16 times in every outer pass.
- **Source**: Campus Assessment Question Bank

---
### PSE-009: Recursive Call Stack Tracing with Step Decrement

**Tag**: [MOCK-EXAM] | **Difficulty**: Easy | **Topic**: Recursion

#### Question
What is the output of `solve(7)`?
```text
function solve(n):
    if n <= 1 then
        return 1
    end if
    return n + solve(n - 2)

Print solve(7)
```

- **A**: 15
- **B**: 16
- **C**: 28
- **D**: 12

**Correct Answer**: **B**

#### Why
1. Trace recursive calls:
   - `solve(7) = 7 + solve(5)`
   - `solve(5) = 5 + solve(3)`
   - `solve(3) = 3 + solve(1)`
   - `solve(1) = 1` (base case: $n \le 1$).
2. Backtrack and sum:
   `7 + 5 + 3 + 1 = 16`.
3. Output is `16`.

- **5-Second Shortcut**: Sum of odd numbers down to 1: 7 + 5 + 3 + 1 = 16.
- **Trap**: Stopping at solve(3) or forgetting base case return 1.
- **Source**: Campus Assessment Question Bank

---
### PSE-010: Recursion Call Stack Tracing: Factorial Function

**Tag**: [MOCK-EXAM] | **Difficulty**: Easy | **Topic**: Recursion

#### Question
What is the output of `solve(4)`?
```text
function solve(n)
    if n == 0 then
        return 1
    end if
    return n * solve(n - 1)
end function

print solve(4)
```

- **A**: 24
- **B**: 12
- **C**: 16
- **D**: 4

**Correct Answer**: **A**

#### Why
1. `solve(4) = 4 * solve(3)`
2. `solve(3) = 3 * solve(2)`
3. `solve(2) = 2 * solve(1)`
4. `solve(1) = 1 * solve(0)`
5. `solve(0) = 1` (base case).
6. Total product: `4 * 3 * 2 * 1 * 1 = 24`.

- **5-Second Shortcut**: 4! = 4 * 3 * 2 * 1 = 24.
- **Trap**: Returning 0 from base case.
- **Source**: Campus Assessment Question Bank

---
### PSE-011: Recursion: Mirrored Head-and-Tail Calls

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Recursion Flow

#### Question
What sequence of characters is printed by calling `print_pattern(3)`?
```text
function print_pattern(n)
    if n == 0 then
        return
    end if
    print n
    print_pattern(n - 1)
    print n
end function
```

- **A**: 3 2 1 1 2 3
- **B**: 3 2 1 2 3
- **C**: 1 2 3 3 2 1
- **D**: 3 2 1

**Correct Answer**: **A**

#### Why
1. For `n = 1`: prints `1`, calls `f(0)` (returns), prints `1` -> outputs `1 1`.
2. For `n = 2`: prints `2`, executes `f(1)` (`1 1`), prints `2` -> outputs `2 1 1 2`.
3. For `n = 3`: prints `3`, executes `f(2)` (`2 1 1 2`), prints `3` -> outputs `3 2 1 1 2 3`.
4. Result is `3 2 1 1 2 3`.

- **5-Second Shortcut**: Symmetric unwind: pre-order prints descending, post-order prints ascending around base.
- **Trap**: Missing the second print statement after recursive call.
- **Source**: Campus Assessment Question Bank

---
### PSE-013: Variable Scope Inside Nested Loop Shadowing

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Scope & Shadowing

#### Question
What is the output of the following pseudocode?
```text
Integer x = 10
For i = 1 to 2 do:
    Integer x = 5
    x = x + 2
End For
Print x
```

- **A**: 10
- **B**: 7
- **C**: 14
- **D**: 5

**Correct Answer**: **A**

#### Why
1. An outer variable `x` is initialized to `10`.
2. Inside the `For` loop, a new local variable `x` is declared, shadowing the outer `x`.
3. The inner `x` becomes `7`, but when the loop iteration ends, the inner `x` goes out of scope and is destroyed.
4. The outer variable `x` remains unaffected at `10`.
5. Printed value is `10`.

- **5-Second Shortcut**: Inner block declaration shadows outer variable; outer x is unchanged at 10.
- **Trap**: Assuming inner reassignment mutates outer x.
- **Source**: Campus Assessment Question Bank

---
### PSE-014: Step-Variable Modification Inside Loop Body

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Loop Mechanics

#### Question
Trace the following pseudocode:
```text
Integer sum = 0
For i = 1 to 5 do:
    If i % 2 == 1 then:
        i = i + 1
    End If
    sum = sum + i
End For
Print sum
```

- **A**: 12
- **B**: 18
- **C**: 14
- **D**: 20

**Correct Answer**: **A**

#### Why
1. Iteration 1: `i = 1`. Since `1 % 2 == 1`, `i` becomes `2`. `sum = 0 + 2 = 2`. Loop increment advances `i` to `3`.
2. Iteration 2: `i = 3`. Since `3 % 2 == 1`, `i` becomes `4`. `sum = 2 + 4 = 6`. Loop increment advances `i` to `5`.
3. Iteration 3: `i = 5`. Since `5 % 2 == 1`, `i` becomes `6`. `sum = 6 + 6 = 12`. Loop increment advances `i` to `7`.
4. `i = 7 > 5`, loop terminates.
5. Final `sum` is `12`.

- **5-Second Shortcut**: Trace manual loop steps: i values added are 2, 4, 6 -> sum = 12.
- **Trap**: Forgetting loop counter auto-increments at the end of each iteration.
- **Source**: Campus Assessment Question Bank

---
### PSE-015: While Loop with Post-Decrement Logic

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Loop Logic

#### Question
What is the output of the following pseudocode?
```text
Integer a = 5, count = 0
While (a-- > 0) do:
    If a % 2 == 0 then:
        count = count + a
    End If
End While
Print count
```

- **A**: 6
- **B**: 8
- **C**: 4
- **D**: 10

**Correct Answer**: **A**

#### Why
1. Iteration 1: `a = 5 > 0` (true), `a` decrements to `4`. `4 % 2 == 0`, `count = 0 + 4 = 4`.
2. Iteration 2: `a = 4 > 0` (true), `a` decrements to `3`. `3 % 2 != 0`, `count` remains `4`.
3. Iteration 3: `a = 3 > 0` (true), `a` decrements to `2`. `2 % 2 == 0`, `count = 4 + 2 = 6`.
4. Iteration 4: `a = 2 > 0` (true), `a` decrements to `1`. `1 % 2 != 0`, `count` remains `6`.
5. Iteration 5: `a = 1 > 0` (true), `a` decrements to `0`. `0 % 2 == 0`, `count = 6 + 0 = 6`.
6. Iteration 6: `a = 0 > 0` (false), `a` decrements to `-1`. Loop terminates.
7. Final printed `count` is `6`.

- **5-Second Shortcut**: Post-decrement decrements before loop body: values checked are 4, 3, 2, 1, 0 -> 4 + 2 + 0 = 6.
- **Trap**: Using value before decrement inside body (5, 4, 3...).
- **Source**: Campus Assessment Question Bank

---
### PSE-018: Multi-Branch Tree Recursion

**Tag**: [MOCK-EXAM] | **Difficulty**: Hard | **Topic**: Tree Recursion

#### Question
What is the return value of `tree(3)`?
```text
function tree(n):
    if n <= 0 then:
        return 1
    end if
    return tree(n - 1) + tree(n - 2)
```

- **A**: 5
- **B**: 3
- **C**: 4
- **D**: 6

**Correct Answer**: **A**

#### Why
1. Base cases: For $n \le 0$, `tree(n) = 1`.
2. `tree(1) = tree(0) + tree(-1) = 1 + 1 = 2`.
3. `tree(2) = tree(1) + tree(0) = 2 + 1 = 3`.
4. `tree(3) = tree(2) + tree(1) = 3 + 2 = 5`.
5. Return value is `5`.

- **5-Second Shortcut**: Notice recurrence with base value 1 for n <= 0: yields 1, 2, 3, 5.
- **Trap**: Confusing with standard 0-indexed Fibonacci where fib(0)=0, fib(1)=1.
- **Source**: Campus Assessment Question Bank

---
### PSE-019: Nested Recursive Function (McCarthy 91 Pattern)

**Tag**: [MOCK-EXAM] | **Difficulty**: Hard | **Topic**: Nested Recursion

#### Question
Evaluate the return value of `mc(95)`:
```text
function mc(n):
    if n > 100 then:
        return n - 10
    else:
        return mc(mc(n + 11))
    end if
```

- **A**: 91
- **B**: 95
- **C**: 101
- **D**: 85

**Correct Answer**: **A**

#### Why
1. This is McCarthy's 91 function. For all inputs $n \le 100$, it famously returns `91`.
2. Trace `mc(95)`:
   - `mc(95) = mc(mc(106))`
   - `106 > 100`, so `mc(106) = 106 - 10 = 96`.
   - Now evaluate `mc(96) = mc(mc(107)) = mc(97) = ... = mc(101) = 91`.
3. Output is `91`.

- **5-Second Shortcut**: McCarthy 91 property: for all n <= 100, mc(n) = 91.
- **Trap**: Tracing deep nested calls manually and losing stack track.
- **Source**: Campus Assessment Question Bank

---
### PSE-020: Static/Global Variable in Recursion

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Static Scope

#### Question
What is the output of `func(5)`?
```c
int func(int n) {
    static int x = 0;
    if (n <= 0) return 0;
    x++;
    return func(n - 1) + x;
}
```

- **A**: 25
- **B**: 15
- **C**: 10
- **D**: 20

**Correct Answer**: **A**

#### Why
1. `static int x` persists across all function calls.
2. In the unwinding phase, `x` is incremented 5 times: on calls `n=5, 4, 3, 2, 1`.
3. When base case `func(0)` is hit, `x = 5`.
4. Now unwinding returns:
   - Each returning frame adds the current value of `x` (which is `5` permanently!).
   - `func(0) = 0`
   - `func(1) = func(0) + 5 = 5`
   - `func(2) = func(1) + 5 = 10`
   - `func(3) = func(2) + 5 = 15`
   - `func(4) = func(3) + 5 = 20`
   - `func(5) = func(4) + 5 = 25`.
5. Output is `25`.

- **5-Second Shortcut**: x reaches 5 and remains 5 during stack unwind: 5 calls * 5 = 25.
- **Trap**: Assuming x reverts to its value at call time.
- **Source**: Campus Assessment Question Bank

---
### PSE-021: Indirect Mutual Recursion

**Tag**: [MOCK-EXAM] | **Difficulty**: Hard | **Topic**: Mutual Recursion

#### Question
What is the output of `fA(4)`?
```text
function fA(n):
    if n <= 0 then return 1
    return n + fB(n - 1)

function fB(n):
    if n <= 0 then return 0
    return n + fA(n - 2)
```

- **A**: 7
- **B**: 8
- **C**: 9
- **D**: 6

**Correct Answer**: **B**

#### Why
1. Trace alternating calls:
   - `fA(4) = 4 + fB(3)`
   - `fB(3) = 3 + fA(1)`
   - `fA(1) = 1 + fB(0)`
   - `fB(0) = 0` (base case: $n \le 0$ returns 0).
2. Substitute back:
   - `fA(1) = 1 + 0 = 1`.
   - `fB(3) = 3 + 1 = 4`.
   - `fA(4) = 4 + 4 = 8`.
3. Output is `8`.

- **5-Second Shortcut**: Trace chain: fA(4) = 4 + fB(3) = 4 + 3 + fA(1) = 4 + 3 + 1 + 0 = 8.
- **Trap**: Applying fA's base return (1) to fB.
- **Source**: Campus Assessment Question Bank

---
### PSE-022: String Substring Tail Recursion

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: String Recursion

#### Question
What is returned by `rev("DATA")`?
```text
function rev(s):
    if length(s) <= 1 then:
        return s
    end if
    return rev(substring(s, 1)) + s[0]
```

- **A**: ATAD
- **B**: DATA
- **C**: ATAD
- **D**: TAAD

**Correct Answer**: **A**

#### Why
1. `rev("DATA") = rev("ATA") + 'D'`
2. `rev("ATA") = rev("TA") + 'A'`
3. `rev("TA") = rev("A") + 'T'`
4. `rev("A") = "A"` (length 1).
5. Unwinding:
   - `"A" + 'T' = "AT"`
   - `"AT" + 'A' = "ATA"`
   - `"ATA" + 'D' = "ATAD"`.
6. Return value is `"ATAD"` (reversed string).

- **5-Second Shortcut**: Recursive string reversal: moves first character to the end.
- **Trap**: Thinking s[0] remains at the start.
- **Source**: Campus Assessment Question Bank

---
### PSE-023: Recursive Tree Calculation

**Tag**: [MOCK-EXAM] | **Difficulty**: Medium | **Topic**: Tree Traversal

#### Question
What is the output of `calc(4)`?
```text
function calc(n):
    if n <= 1 then:
        return 1
    end if
    return 2 * calc(n - 1)
```

- **A**: 8
- **B**: 16
- **C**: 4
- **D**: 32

**Correct Answer**: **A**

#### Why
1. `calc(4) = 2 * calc(3)`
2. `calc(3) = 2 * calc(2)`
3. `calc(2) = 2 * calc(1)`
4. `calc(1) = 1` (base case).
5. Combining: `2 * 2 * 2 * 1 = 8` (calculates $2^{n-1}$).
6. Output is `8`.

- **5-Second Shortcut**: Powers of 2: 2^(4-1) = 2^3 = 8.
- **Trap**: Calculating 2^4 = 16.
- **Source**: Campus Assessment Question Bank

---

Previous: [02_bitwise_operators_and_tricks.md](02_bitwise_operators_and_tricks.md) | Next: [04_pseudocode_question_bank.md](04_pseudocode_question_bank.md)
