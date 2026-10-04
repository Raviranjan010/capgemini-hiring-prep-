# Pseudocode & Technical Assessment Master Guide

This comprehensive guide covers execution tracing, operator precedence, syntax rules, algorithmic analysis, bitwise mechanics, and the complete question bank from the Capgemini Technical & Pseudo-code assessment.

---

## 1. Assessment Overview & Strategy

The Technical & Pseudo-code section evaluates core programming logic, dry running, algorithmic recognition, and computer science fundamentals under tight time constraints (~1 minute per question).

```text
Capgemini Technical Assessment Blueprint
├── Question Format: Pseudo-codes & MCQs
├── Time Allocation: ~1 minute per question
├── Core Domains:
│   ├── Pseudo-code Tracing (Output, Syntax, and Algorithms)
│   ├── Data Structures & Algorithms (Arrays, Linked Lists, Trees, Graphs, Sorting)
│   ├── Operating Systems & Computer Networks
│   └── DBMS & SQL Queries
└── Primary Skill Tested: Step-by-step dry running and loop bounds analysis
```

### Essential Pseudo-code Tracing Rules
1. **Inclusive Loop Bounds**: In Capgemini pseudo-code, statements such as `for i = 0 to 4` or `for i = 1 to n` are **inclusive of both boundaries** ($0, 1, 2, 3, 4$) unless an explicit condition like `<` or `to n - 1` is stated.
2. **Block Closures**: Block statements must be formally terminated (`end if`, `end for`, `end while`). Omitting closure tags produces a **Syntax Error**.
3. **Variable Scope & Shadowing**: Carefully observe where variables are declared. Re-declaring an accumulator variable inside a loop resets its state on every iteration.

---

## 2. Master Operator Precedence & Associativity Table (C, C++, Java)

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
> - Logical AND (`&&`) > Logical OR (`||`).

---

## 3. High-Frequency Bit Manipulation Tricks

| Trick | Formula | Binary Meaning & Quick Application |
| :--- | :--- | :--- |
| **Isolate Rightmost Set Bit** | `n & (-n)` | Isolates lowest power-of-2 bit set to `1` using two's complement. |
| **Clear Rightmost Set Bit** | `n & (n - 1)` | Turns off lowest set bit. If `(n & (n - 1)) == 0` ($n > 0$), $n$ is power of 2. |
| **Self XOR Cancellation** | `x ^ x = 0` | A number XORed with itself is `0` (cancels paired duplicates). |
| **XOR Identity** | `x ^ 0 = x` | A number XORed with `0` preserves its value. |
| **In-Place Swap** | `a ^= b; b ^= a; a ^= b;` | Swaps two variables without temporary storage. |
| **Digital Root Formula** | `1 + ((n - 1) % 9)` | Evaluates repeated sum of digits for any positive base-10 integer in $O(1)$. |

---

## 4. Assessment Walkthrough: Core Video Problems

### Problem 1: Sum of Even Array Elements (Output Tracing)
**Tag**: [VIDEO]

**Pseudocode**:
```text
Declare Array arr = [3, 8, 2, 9, 5]
Declare sum = 0
for i = 0 to 4
    if (arr[i] % 2 == 0) then
        sum = sum + arr[i]
    end if
end for
print sum
```

**Detailed Execution Trace**:
- Initial State: `arr = [3, 8, 2, 9, 5]`, `sum = 0`.
- Loop Iterations: Executes for indices $i = 0, 1, 2, 3, 4$ (Total 5 passes):
  - $i = 0 \implies arr[0] = 3$: $3 \% 2 \ne 0 \implies$ false, `sum = 0`.
  - $i = 1 \implies arr[1] = 8$: $8 \% 2 == 0 \implies$ true, `sum = 0 + 8 = 8`.
  - $i = 2 \implies arr[2] = 2$: $2 \% 2 == 0 \implies$ true, `sum = 8 + 2 = 10`.
  - $i = 3 \implies arr[3] = 9$: $9 \% 2 \ne 0 \implies$ false, `sum = 10`.
  - $i = 4 \implies arr[4] = 5$: $5 \% 2 \ne 0 \implies$ false, `sum = 10`.
- **Output**: `10`

**Exam Trap**: If `sum = 0` were declared inside the `for` loop body, `sum` would reset to `0` on each pass, printing only the last matching element (`2`) rather than the cumulative sum.

---

### Problem 2: Missing Loop Terminator (Syntax Error Detection)
**Tag**: [VIDEO]

**Pseudocode**:
```text
start
input n
fact = 1
for i = 1 to n
    fact = fact * i
print fact
end
```

**Analysis & Options**:
- **Code Intent**: Calculates factorial $n! = \prod_{i=1}^n i$.
- **Defect**: In standard pseudo-code specifications, structured looping constructs require an explicit block terminator (`end for`).
- **Correct Conclusion**: **Syntax Error** due to missing `end for` before `print fact`.

---

### Problem 3: Second Largest Element Detection (Algorithmic Tracing)
**Tag**: [VIDEO]

**Pseudocode**:
```text
Declare Array arr = [12, 5, 20, 8, 15]
largest = -infinity
second = -infinity
for each element x in arr do
    if x > largest then
        second = largest
        largest = x
    else if x > second then
        second = x
    end if
end for
print second
```

**Detailed Execution Trace**:
- Initial State: `largest = -INF`, `second = -INF`.
- Iteration 1 ($x = 12$): $12 > -\infty \implies \text{second} = -\infty, \text{largest} = 12$.
- Iteration 2 ($x = 5$): $5 \ngtr 12$, check else if: $5 > -\infty \implies \text{second} = 5, \text{largest} = 12$.
- Iteration 3 ($x = 20$): $20 > 12 \implies \text{second} = 12, \text{largest} = 20$.
- Iteration 4 ($x = 8$): $8 \ngtr 20$, check else if: $8 > 12$ is false $\implies$ unchanged ($\text{largest} = 20, \text{second} = 12$).
- Iteration 5 ($x = 15$): $15 \ngtr 20$, check else if: $15 > 12 \implies \text{second} = 15, \text{largest} = 20$.
- **Output**: `15`

---

### Problem 4: Two-Pointer Palindrome Check (Algorithmic Logic)
**Tag**: [VIDEO]

**Pseudocode**:
```text
Declare String str = "level"
left = 0
right = length(str) - 1
isPalindrome = true
while left < right do
    if str[left] != str[right] then
        isPalindrome = false
        break
    else
        left = left + 1
        right = right - 1
    end if
end while
print isPalindrome
```

**Detailed Execution Trace**:
- Initial State: `str = "level"`, `left = 0` ('l'), `right = 4` ('l'), `isPalindrome = true`.
- Pass 1: `str[0] == str[4]` ('l' == 'l') $\implies left = 1, right = 3$.
- Pass 2: `str[1] == str[3]` ('e' == 'e') $\implies left = 2, right = 2$.
- Termination: Condition `left < right` ($2 < 2$) fails. Loop exits.
- **Output**: `true`

---

### Problem 5: Digital Root / Repeated Sum of Digits (Campus Actual Question)
**Tag**: [VIDEO]

**Pseudocode**:
```text
Declare num = 942
while num >= 10 do
    sum = 0
    while num > 0 do
        sum = sum + (num % 10)
        num = num / 10
    end while
    num = sum
end while
print num
```

**Detailed Execution Trace**:
- **Outer Pass 1** ($num = 942 \ge 10$):
  - Inner loop extracts digits: $2 + 4 + 9 = 15$.
  - Update: $num = 15$.
- **Outer Pass 2** ($num = 15 \ge 10$):
  - Inner loop extracts digits: $5 + 1 = 6$.
  - Update: $num = 6$.
- **Outer Pass 3** ($num = 6 < 10$): Loop terminates.
- **Output**: `6`

> [!TIP]
> **Mathematical Shortcut (Digital Root Formula)**:
> For any base-10 integer $n > 0$:
> $$\text{Digital Root}(n) = 1 + ((n - 1) \pmod 9)$$
> For $942$: $1 + ((942 - 1) \pmod 9) = 1 + (941 \pmod 9) = 1 + 5 = \mathbf{6}$. (Instant $O(1)$ computation).

---

### Problem 6: Fibonacci Sequence Generation
**Tag**: [VIDEO]

**Pseudocode**:
```text
Declare a = 0, b = 1
print a, b
for i = 3 to 6
    c = a + b
    print c
    a = b
    b = c
end for
```

**Detailed Execution Trace**:
- Initial Output: Prints `0, 1`.
- Loop Range: $i = 3, 4, 5, 6$ (Executes 4 times):
  - $i = 3$: $c = 0 + 1 = 1 \implies$ Print `1`. Update $a = 1, b = 1$.
  - $i = 4$: $c = 1 + 1 = 2 \implies$ Print `2`. Update $a = 1, b = 2$.
  - $i = 5$: $c = 1 + 2 = 3 \implies$ Print `3`. Update $a = 2, b = 3$.
  - $i = 6$: $c = 2 + 3 = 5 \implies$ Print `5`. Update $a = 3, b = 5$.
- **Final Printed Stream**: `0, 1, 1, 2, 3, 5`

---

## 5. High-Frequency Technical MCQs (Curated Exam Bank)

### Category A: Core Pseudo-codes & Bitwise Mechanics

#### Question 1: Bitwise XOR Swap & Assignment
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
Integer a = 7, b = 4
a = a ^ b
b = a ^ b
a = a ^ b
b = b << 1
Print a + b
```

- **A)** 11
- **B)** 18
- **C)** 22
- **D)** 15

**Correct Answer**: Option B

**Trace**:
1. The three XOR statements execute the canonical variable swap:
   - $a = 7 \oplus 4 = 3$.
   - $b = 3 \oplus 4 = 7$.
   - $a = 3 \oplus 7 = 4$.
   - Now $a = 4$ and $b = 7$.
2. Shift: $b = b \ll 1 \implies 7 \times 2 = 14$.
3. Sum: $a + b = 4 + 14 = \mathbf{18}$.

---

#### Question 2: Nested Loop Complexity & Execution Counter
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
Integer count = 0, n = 16
for i = 1 to n step i = i * 2
    for j = 1 to i
        count = count + 1
    end for
end for
Print count
```

- **A)** 16
- **B)** 31
- **C)** 64
- **D)** 32

**Correct Answer**: Option B

**Trace**:
- Outer loop steps powers of 2: $i \in \{1, 2, 4, 8, 16\}$.
- In each pass, inner loop runs exactly $i$ times:
  $$\text{count} = 1 + 2 + 4 + 8 + 16 = 2^5 - 1 = \mathbf{31}$$

---

#### Question 3: Short-Circuit Logical Operators
**Tag**: [MOCK-EXAM]

**Snippet**:
```c
int a = 0, b = 5, c = 10;
if (a++ && ++b) {
    c = c + 10;
} else {
    c = c + b;
}
printf("%d, %d, %d", a, b, c);
```

- **A)** 1, 5, 15
- **B)** 1, 6, 15
- **C)** 0, 5, 15
- **D)** 1, 6, 20

**Correct Answer**: Option A

**Trace**:
1. `a++` evaluates to `0` (false) before incrementing.
2. Because left side of `&&` is false, **short-circuit evaluation** occurs. The right side `++b` is skipped completely, so $b$ remains `5`.
3. Post-increment updates $a$ from `0` to `1`.
4. The `else` branch executes: $c = c + b = 10 + 5 = 15$.
5. Output: `1, 5, 15`.

---

#### Question 4: Recursive Call Stack Tracing
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
function solve(n):
    if n <= 1 then
        return 1
    end if
    return n + solve(n - 2)

Print solve(7)
```

- **A)** 15
- **B)** 16
- **C)** 28
- **D)** 12

**Correct Answer**: Option B

**Trace**:
$$\text{solve}(7) = 7 + \text{solve}(5)$$
$$\text{solve}(5) = 5 + \text{solve}(3)$$
$$\text{solve}(3) = 3 + \text{solve}(1)$$
$$\text{solve}(1) = 1 \ (\text{base case})$$
Sum: $7 + 5 + 3 + 1 = \mathbf{16}$.

---

#### Question 4B: Recursion Call Stack Tracing: Factorial Function
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
function solve(n)
    if n == 0 then
        return 1
    end if
    return n * solve(n - 1)
end function

print solve(4)
```

**Recursive Unwinding Trace**:
- **Descent Phase (Pushing onto Call Stack)**:
  $$\text{solve}(4) = 4 \times \text{solve}(3)$$
  $$\text{solve}(3) = 3 \times \text{solve}(2)$$
  $$\text{solve}(2) = 2 \times \text{solve}(1)$$
  $$\text{solve}(1) = 1 \times \text{solve}(0)$$
  $$\text{Base Case: } \text{solve}(0) = 1$$
- **Return Phase (Popping and Bottom-Up Evaluation)**:
  $$\text{solve}(1) = 1 \times 1 = 1$$
  $$\text{solve}(2) = 2 \times 1 = 2$$
  $$\text{solve}(3) = 3 \times 2 = 6$$
  $$\text{solve}(4) = 4 \times 6 = 24$$
- **Final Output**: `24`

---

#### Question 4C: Recursion: Mirrored Head-and-Tail Calls
**Tag**: [MOCK-EXAM]

**Question**:  
What is the output of `compute(3)`?

```text
function compute(n)
    if n <= 0 then
        return
    end if
    print n
    compute(n - 1)
    print n
end function
```

- **A)** `3 2 1`
- **B)** `3 2 1 1 2 3`
- **C)** `1 2 3 3 2 1`
- **D)** `3 2 1 2 3`

**Correct Answer**: **Option B (`3 2 1 1 2 3`)**

**Derivation**:
- `compute(3)` prints `3`, calls `compute(2)`, then pauses awaiting return to print `3`.
- `compute(2)` prints `2`, calls `compute(1)`, then pauses awaiting return to print `2`.
- `compute(1)` prints `1`, calls `compute(0)`, then pauses awaiting return to print `1`.
- `compute(0)` hits base case `n <= 0` and returns immediately.
- Return phase prints second batch of numbers in reverse order (`1`, then `2`, then `3`).
- **Unwound output stream**: `3 2 1 1 2 3`.

---

### Category B: Data Structures & Algorithms

#### Binary Tree Traversals: Reference & Intuitive Visual Paths

| Traversal Type | Structural Sequence | Intuitive Visual Path | Use Cases & Invariants |
| :--- | :--- | :--- | :--- |
| **Pre-order** | $\text{Root} \to \text{Left} \to \text{Right}$ | **Left-perimeter outline** ("Pant-shape" / top-to-bottom perimeter scan) | Cloning/serializing trees, prefix expressions |
| **In-order** | $\text{Left} \to \text{Root} \to \text{Right}$ | **Orthogonal projection** onto the horizontal axis | Generates sorted sequence in BSTs |
| **Post-order** | $\text{Left} \to \text{Right} \to \text{Root}$ | **Bottom-up leaf elimination** up to the root | Deleting tree nodes, postfix evaluation |
| **Level-order** | Breadth-First Search (BFS) | **Horizontal scanning layer-by-layer** using a Queue | Finding shortest unweighted paths |

```text
       1
      / \
     2   3
    / \   \
   4   5   6
```

**Traversal Trace Examples**:
- **Problem 1 (Pre-order Recognition)**:  
  Given visitation sequence `10 -> 20 -> 40 -> 50 -> 30 -> 60 -> 70`.  
  Because root `10` is visited first, followed by left child `20`, its left child `40`, backtracking to right sibling `50`, and then processing right subtree `30 -> 60 -> 70`, the order is strictly $\text{Root} \to \text{Left} \to \text{Right}$ (**Pre-order Traversal**).
- **Problem 2 (In-order Step-by-Step Trace)**:  
  For tree above (Root `1`, Left subtree `2` [children `4`, `5`], Right subtree `3` [right child `6`]):
  1. Descend to leftmost leaf: `4`.
  2. Visit parent: `2`.
  3. Visit right child of subtree: `5`.
  4. Left subtree of root is complete; visit main root: `1`.
  5. Enter right subtree of `1`: Left of `3` is empty ($\emptyset$), visit root `3`, visit right leaf `6`.  
  **Output**: `4, 2, 5, 1, 3, 6`.

---

#### Question 5: Binary Search Tree Inorder Traversal Property
**Tag**: [MOCK-EXAM]

**Question**:  
What is the consequence of applying an Inorder traversal ($Left \to Root \to Right$) on any valid Binary Search Tree (BST)?

- **A)** The keys are visited in strictly descending order.
- **B)** The keys are visited in non-decreasing (sorted) order.
- **C)** It produces the level-order hierarchy.
- **D)** It yields the original insertion sequence.

**Correct Answer**: Option B

**Why**:  
By definition of a BST, every key in the left subtree is $\le$ root, and every key in the right subtree is $\ge$ root. An Inorder traversal visits left subtree first, then root, then right subtree, producing elements in sorted non-decreasing order.

---

#### Question 6: Circular Queue Full Condition
**Tag**: [MOCK-EXAM]

**Question**:  
In an array-based implementation of a Circular Queue of size $N$, with pointers `front` and `rear`, what is the standard condition to check if the queue is full?

- **A)** `rear == front`
- **B)** `(rear + 1) % N == front`
- **C)** `rear == N - 1`
- **D)** `(front + 1) % N == rear`

**Correct Answer**: Option B

**Why**:  
In a circular buffer of capacity $N$, reserving one empty slot to distinguish between empty (`front == rear`) and full states yields `(rear + 1) % N == front` when the queue is full.

---

#### Question 7: Sorting Algorithm Lower Bound
**Tag**: [MOCK-EXAM]

**Question**:  
What is the theoretical lower bound on time complexity for any comparison-based sorting algorithm (e.g., Merge Sort, Heap Sort) in the worst case?

- **A)** $O(N)$
- **B)** $O(N \log N)$
- **C)** $O(N^2)$
- **D)** $O(\log N)$

**Correct Answer**: Option B

**Why**:  
The decision-tree model for sorting $N$ elements requires at least $N!$ leaves. The tree height must be at least $\lceil \log_2(N!) \rceil = \Omega(N \log N)$.

---

#### Question 7B: Hash Map Collisions in Open Addressing (Linear Probing)
**Tag**: [MOCK-EXAM]

**Question**:  
In a Hash Table of size $7$ using the hash function $h(k) = k \pmod 7$ and Linear Probing, where will key $23$ be placed if keys $9$ and $16$ are already inserted?

- **A)** Index 2
- **B)** Index 3
- **C)** Index 4
- **D)** Index 5

**Correct Answer**: **Option C (Index 4)**

**Derivation**:
1. Insert key $9$:  
   $$h(9) = 9 \pmod 7 = 2 \implies \text{Placed at Index 2}.$$
2. Insert key $16$:  
   $$h(16) = 16 \pmod 7 = 2 \implies \text{Collision at Index 2}.$$  
   Linear probe checks $(2 + 1) \pmod 7 = 3$ (unoccupied) $\implies \text{Placed at Index 3}$.
3. Insert key $23$:  
   $$h(23) = 23 \pmod 7 = 2 \implies \text{Collision at Index 2}.$$  
   Probe 1: check index $3$ $\implies$ Collision at Index 3 (occupied by 16).  
   Probe 2: check index $(3 + 1) \pmod 7 = 4$ (unoccupied) $\implies \text{Placed at Index 4}$.

---

### Category C: CS Fundamentals (DBMS, OS, & Networks)

#### Question 8: Database Normalization - 2NF
**Tag**: [MOCK-EXAM]

**Question**:  
A relation $R(A, B, C, D)$ has a composite candidate key $(A, B)$. Under which condition does the relation violate Second Normal Form (2NF)?

- **A)** If there is a transitive dependency $C \to D$.
- **B)** If a non-prime attribute (e.g., $C$) depends on a proper subset of the candidate key (e.g., $A \to C$).
- **C)** If all functional dependencies have a candidate key on the left-hand side.
- **D)** If multivalued dependencies exist between $A$ and $B$.

**Correct Answer**: Option B

**Why**:  
2NF requires the relation to be in 1NF and contain **no partial dependencies**—no non-prime attribute should depend on a proper subset of any candidate key.

---

#### Question 9: Operating Systems - Thrashing
**Tag**: [MOCK-EXAM]

**Question**:  
What causes thrashing in a virtual memory operating system?

- **A)** Deadlock occurring inside high-priority driver threads.
- **B)** The operating system spending substantially more time swapping pages in and out of secondary storage than executing process instructions.
- **C)** High CPU temperature triggering clock frequency scaling.
- **D)** Fragmented disk sectors during sequential I/O requests.

**Correct Answer**: Option B

**Why**:  
Thrashing occurs when total process working sets exceed available physical memory frames, leading to continuous page faults and near-zero CPU throughput as the system spends all its time on disk paging.

---

#### Question 10: Computer Networks - Subnetting & Addressing
**Tag**: [MOCK-EXAM]

**Question**:  
Given the IP address `192.168.10.65` with subnet mask `/26` (`255.255.255.192`), what is the Network ID and the Broadcast Address for this subnet?

- **A)** Network: 192.168.10.0, Broadcast: 192.168.10.63
- **B)** Network: 192.168.10.64, Broadcast: 192.168.10.127
- **C)** Network: 192.168.10.64, Broadcast: 192.168.10.255
- **D)** Network: 192.168.10.32, Broadcast: 192.168.10.95

**Correct Answer**: Option B

**Calculation**:
- Subnet mask `/26` leaves $32 - 26 = 6$ host bits.
- Block size $= 2^6 = 64$.
- Subnet ranges: `0–63`, `64–127`, `128–191`, `192–255`.
- IP `65` falls in block `64–127` $\implies$ Network ID = `192.168.10.64`, Broadcast = `192.168.10.127`.

---

## 6. 15 Exam-Level Pseudo-Code Practice Questions

### Part 1: Bitwise & Arithmetic Operations

#### Problem 1: Operator Precedence with Bitwise Shifts
**Pseudocode**:
```text
Integer a = 5, b = 2, c = 3
c = a + b << c & b ^ a
Print c
```
- **A)** 0
- **B)** 5
- **C)** 7
- **D)** 2

**Correct Answer**: Option B (5)

**Step-by-Step Solution**:
1. Precedence: `+` > `<<` > `&` > `^`.
2. `a + b` $\implies 5 + 2 = 7$.
3. `7 << c` $\implies 7 \ll 3 = 7 \times 8 = 56$.
4. `56 & b` $\implies (111000)_2 \ \& \ (000010)_2 = 0$.
5. `0 ^ a` $\implies 0 \oplus 5 = \mathbf{5}$.

---

#### Problem 2: XOR Cancellation and Masking
**Pseudocode**:
```text
Integer p = 12, q = 25, r = 7
p = (p ^ q) ^ p
q = q & (r << 2)
r = p ^ q ^ r
Print p + q + r
```
- **A)** 48
- **B)** 55
- **C)** 39
- **D)** 25

**Correct Answer**: Option B (55)

**Step-by-Step Solution**:
1. `p = (p ^ q) ^ p`: By XOR cancellation $(p \oplus p) \oplus q = 0 \oplus q = q \implies p = 25$.
2. `r << 2` $\implies 7 \times 4 = 28$.
3. `q = 25 & 28` $\implies (11001)_2 \ \& \ (11100)_2 = (11000)_2 = 24$.
4. `r = p ^ q ^ r` $\implies 25 \oplus 24 \oplus 7 = 1 \oplus 7 = 6$.
5. `p + q + r` $\implies 25 + 24 + 6 = \mathbf{55}$.

---

#### Problem 3: Modulo, Division, and Bitwise OR
**Pseudocode**:
```text
Integer x = 45, y = 6
x = (x % y) | (x / y)
y = x ^ (y << 1)
Print x + y
```
- **A)** 21
- **B)** 18
- **C)** 23
- **D)** 17

**Correct Answer**: Option B (18)

**Step-by-Step Solution**:
1. `x % y` $\implies 45 \% 6 = 3$.
2. `x / y` $\implies 45 / 6 = 7$.
3. `x = 3 | 7` $\implies (011)_2 \mid (111)_2 = 7$.
4. `y << 1` $\implies 6 \times 2 = 12$.
5. `y = 7 ^ 12` $\implies (0111)_2 \oplus (1100)_2 = (1011)_2 = 11$.
6. `x + y` $\implies 7 + 11 = \mathbf{18}$.

---

#### Problem 4: Logical Short-Circuit Simulation
**Pseudocode**:
```text
Integer a = 1, b = 0, c = 2
if ((a = a - 1) && (b = b + 5))
    c = c + a
else
    c = c + b
end if
Print a + b + c
```
- **A)** 2
- **B)** 7
- **C)** 4
- **D)** 8

**Correct Answer**: Option A (2)

**Step-by-Step Solution**:
1. `a = a - 1` assigns $1 - 1 = 0$.
2. Because $0$ is false, `&&` **short-circuits**. The right side `b = b + 5` does not execute ($b = 0$).
3. The `else` block executes: $c = c + b = 2 + 0 = 2$.
4. `a + b + c` $\implies 0 + 0 + 2 = \mathbf{2}$.

---

#### Problem 5: Right Shifts and Negative Bit Inversion
**Pseudocode**:
```text
Integer a = 32, b = 3
a = a >> b
b = ~b + 1
Print a + b
```
- **A)** 1
- **B)** 4
- **C)** 7
- **D)** -1

**Correct Answer**: Option A (1)

**Step-by-Step Solution**:
1. `a >> b` $\implies 32 \gg 3 = 32 / 8 = 4$.
2. `~b + 1` $\implies -b = -3$ (two's complement).
3. `a + b` $\implies 4 + (-3) = \mathbf{1}$.

---

### Part 2: Iteration, Nested Loops, and Boundary Conditions

#### Problem 6: Variable Scope Inside Nested Loop
**Pseudocode**:
```text
Integer a = 0, b = 0
for i = 1 to 3
    Integer temp = 10
    for j = 1 to 2
        temp = temp + 2
        b = b + 1
    end for
    a = a + temp
end for
Print a + b
```
- **A)** 48
- **B)** 54
- **C)** 42
- **D)** 36

**Correct Answer**: Option A (48)

**Step-by-Step Solution**:
- `temp` is re-declared inside outer loop $\implies$ resets to $10$ every pass of $i$.
- Inner loop runs twice ($j = 1, 2$):
  - After inner loop: $\text{temp} = 10 + 2 \times 2 = 14$.
  - $b$ increments on each inner pass $\implies b = 3 \times 2 = 6$.
- Outer loop runs 3 times: $a = 14 \times 3 = 42$.
- `a + b` $\implies 42 + 6 = \mathbf{48}$.

---

#### Problem 7: Step-Variable Modification Inside Loop
**Pseudocode**:
```text
Integer count = 0, sum = 0
for i = 1 to 10
    if i % 3 == 0 then
        sum = sum + i
        i = i + 1
    end if
    count = count + 1
end for
Print sum + count
```
- **A)** 27
- **B)** 26
- **C)** 25
- **D)** 24

**Correct Answer**: Option C (25)

**Iteration Trace**:
- $i = 1$: `count = 1`
- $i = 2$: `count = 2`
- $i = 3$: `sum = 3`, $i$ set to $4$, `count = 3` (loop step makes $i = 5$)
- $i = 5$: `count = 4` (next $i = 6$)
- $i = 6$: `sum = 3 + 6 = 9`, $i$ set to $7$, `count = 5` (loop step makes $i = 8$)
- $i = 8$: `count = 6` (next $i = 9$)
- $i = 9$: `sum = 9 + 9 = 18`, $i$ set to $10$, `count = 7` (loop step makes $i = 11 > 10$, terminates)
- `sum + count` $\implies 18 + 7 = \mathbf{25}$.

---

#### Problem 8: While Loop with Post-Decrement Logic
**Pseudocode**:
```text
Integer n = 15, ans = 0
while n > 0
    if n % 2 == 1 then
        ans = ans + n
        n = n - 3
    else
        n = n / 2
    end if
end while
Print ans
```
- **A)** 33
- **B)** 27
- **C)** 24
- **D)** 18

**Correct Answer**: Option D (18)

**Trace**:
- $n = 15$ (odd): `ans = 15`, $n = 12$.
- $n = 12$ (even): $n = 12 / 2 = 6$.
- $n = 6$ (even): $n = 6 / 2 = 3$.
- $n = 3$ (odd): `ans = 15 + 3 = 18`, $n = 0$.
- $n = 0$: loop exits. Output: **18**.

---

#### Problem 9: 2D Array Diagonals Sum
**Pseudocode**:
```text
Declare Matrix M[3][3] = [[1, 2, 3], 
                          [4, 5, 6], 
                          [7, 8, 9]]
Integer total = 0
for i = 0 to 2
    for j = 0 to 2
        if (i == j) || (i + j == 2) then
            total = total + M[i][j]
        end if
    end for
end for
Print total
```
- **A)** 25
- **B)** 20
- **C)** 29
- **D)** 45

**Correct Answer**: Option A (25)

**Trace**:
- Picks elements on main diagonal ($i == j$) OR anti-diagonal ($i + j == 2$):
  - Row 0: `M[0][0] = 1`, `M[0][2] = 3`
  - Row 1: `M[1][1] = 5` (center element counted once)
  - Row 2: `M[2][0] = 7`, `M[2][2] = 9`
- Total: $1 + 3 + 5 + 7 + 9 = \mathbf{25}$.

---

#### Problem 10: Array Element In-Place Modification
**Pseudocode**:
```text
Declare Array A = [2, 4, 6, 8]
for i = 1 to 3
    A[i] = A[i] + A[i - 1]
end for
Print A[3]
```
- **A)** 14
- **B)** 20
- **C)** 16
- **D)** 12

**Correct Answer**: Option B (20)

**Trace**:
- $i = 1$: $A[1] = 4 + 2 = 6 \implies A = [2, 6, 6, 8]$
- $i = 2$: $A[2] = 6 + 6 = 12 \implies A = [2, 6, 12, 8]$
- $i = 3$: $A[3] = 8 + 12 = 20 \implies A = [2, 6, 12, 20]$
- Output: **20**.

---

### Part 3: Recursion & Backtracking Traces

#### Problem 11: Multi-Branch Tree Recursion
**Pseudocode**:
```text
function fun(n)
    if n <= 1 then
        return 1
    end if
    return fun(n - 1) + fun(n - 2) + n
end function

Print fun(4)
```
- **A)** 12
- **B)** 16
- **C)** 15
- **D)** 21

**Correct Answer**: Option B (16)

**Trace**:
- $\text{fun}(0) = 1, \text{fun}(1) = 1$
- $\text{fun}(2) = 1 + 1 + 2 = 4$
- $\text{fun}(3) = \text{fun}(2) + \text{fun}(1) + 3 = 4 + 1 + 3 = 8$
- $\text{fun}(4) = \text{fun}(3) + \text{fun}(2) + 4 = 8 + 4 + 4 = \mathbf{16}$.

---

#### Problem 12: Nested Recursive Function (McCarthy 91 Pattern)
**Pseudocode**:
```text
function test(n)
    if n > 100 then
        return n - 10
    else
        return test(test(n + 11))
    end if
end function

Print test(95)
```
- **A)** 91
- **B)** 95
- **C)** 101
- **D)** 85

**Correct Answer**: Option A (91)

**Why**:  
This is the classic McCarthy 91 function. For any integer $n \le 100$, it evaluates to **91**.

---

#### Problem 13: Static/Global Variable in Recursion
**Pseudocode**:
```text
Integer x = 0

function calc(n)
    if n <= 0 then
        return 0
    end if
    x = x + 1
    return calc(n - 1) + x
end function

Print calc(4)
```
- **A)** 10
- **B)** 16
- **C)** 14
- **D)** 20

**Correct Answer**: Option B (16)

**Trace**:
- Winding: 4 recursive calls increment $x \implies x = 4$ when `calc(0)` returns `0`.
- Unwinding adds final $x = 4$ at each level: $0 + 4 + 4 + 4 + 4 = \mathbf{16}$.

---

#### Problem 14: Indirect Mutual Recursion
**Pseudocode**:
```text
function foo(n)
    if n <= 0 then return 1
    return n + bar(n - 1)
end function

function bar(n)
    if n <= 0 then return 2
    return n * foo(n - 2)
end function

Print foo(4)
```
- **A)** 13
- **B)** 10
- **C)** 15
- **D)** 7

**Correct Answer**: Option A (13)

**Trace**:
- $\text{foo}(4) = 4 + \text{bar}(3)$
- $\text{bar}(3) = 3 \times \text{foo}(1)$
- $\text{foo}(1) = 1 + \text{bar}(0)$
- $\text{bar}(0) = 2 \ (\text{base case})$
- Backtrack:
  - $\text{foo}(1) = 1 + 2 = 3$
  - $\text{bar}(3) = 3 \times 3 = 9$
  - $\text{foo}(4) = 4 + 9 = \mathbf{13}$.

---

#### Problem 15: String Substring Tail Recursion
**Pseudocode**:
```text
function parse(str, idx)
    if idx >= length(str) then
        return 0
    end if
    Integer val = str[idx] - '0'
    if val % 2 == 0 then
        return val + parse(str, idx + 1)
    else
        return parse(str, idx + 1) - val
    end if
end function

Print parse("4321", 0)
```
- **A)** 2
- **B)** 4
- **C)** 6
- **D)** 0

**Correct Answer**: Option A (2)

**Trace**:
- Digits: `idx 0 = 4` (even), `idx 1 = 3` (odd), `idx 2 = 2` (even), `idx 3 = 1` (odd).
- Unwinding from `idx = 4` (returns 0):
  - `idx 3` ('1', odd): $0 - 1 = -1$.
  - `idx 2` ('2', even): $2 + (-1) = 1$.
  - `idx 1` ('3', odd): $1 - 3 = -2$.
  - `idx 0` ('4', even): $4 + (-2) = \mathbf{2}$.

---

#### Problem 16: Bitwise Equivalent Logic
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
FUNCTION BitwiseTest():
    A = 29      // Binary: 11101
    B = 14      // Binary: 01110
    result = (A & B) | (A ^ B)
    RETURN result
```
**Question**: What integer value is returned?
- **A)** 15
- **B)** 27
- **C)** 31
- **D)** 43

**Correct Answer**: Option C (31)

**Mathematical Shortcut**:
In Boolean algebra, $(A \land B) \lor (A \oplus B) \equiv A \lor B$.
Calculating $A \lor B$:
```text
A  = 29 : 1 1 1 0 1
B  = 14 : 0 1 1 1 0
-------------------
OR = 31 : 1 1 1 1 1  (= 31 in decimal)
```

**5-Second Shortcut**: Use the Boolean identity $(A \ \& \ B) \mid (A \wedge B) = A \mid B$. Immediate bitwise OR of 29 and 14 yields 31.  
**Trap**: Manually computing intermediate AND and XOR masks under time pressure and making an off-by-one bit error.

---

#### Problem 17: Recursive Tree Calculation
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
FUNCTION Calculate(n):
    IF n <= 1 THEN:
        RETURN 2
    ELSE:
        RETURN Calculate(n - 1) + Calculate(n - 2) + 1
    END IF
```
**Question**: What value is returned when calling `Calculate(4)`?
- **A)** 11
- **B)** 14
- **C)** 18
- **D)** 21

**Correct Answer**: Option B (14)

**Step-by-step Trace**:
- $\text{Calculate}(0) = 2$
- $\text{Calculate}(1) = 2$
- $\text{Calculate}(2) = \text{Calculate}(1) + \text{Calculate}(0) + 1 = 2 + 2 + 1 = 5$
- $\text{Calculate}(3) = \text{Calculate}(2) + \text{Calculate}(1) + 1 = 5 + 2 + 1 = 8$
- $\text{Calculate}(4) = \text{Calculate}(3) + \text{Calculate}(2) + 1 = 8 + 5 + 1 = 14$

**5-Second Shortcut**: Trace bottom-up like dynamic programming: $T = [2, 2, 5, 8, 14]$.  
**Trap**: Dropping the `+ 1` constant term and confusing this with pure Fibonacci numbers ($2, 2, 4, 6, 10$).

---

#### Problem 18: Stack & Queue Combined Operations
**Tag**: [MOCK-EXAM]

**Pseudocode**:
```text
FUNCTION CombinedOperations():
    CREATE STACK s
    CREATE QUEUE q
    ENQUEUE 10, 20, 30 INTO q
    PUSH DEQUEUE(q) ONTO s     // Dequeues 10 -> Stack s = [10]
    PUSH DEQUEUE(q) ONTO s     // Dequeues 20 -> Stack s = [10, 20]
    ENQUEUE POP(s) INTO q      // Pops 20 -> Enqueues 20 -> q = [30, 20]
    PUSH 40 ONTO s             // Pushes 40 -> Stack s = [10, 40]
    PRINT TOP(s) + FRONT(q)
```
**Question**: What integer is printed?
- **A)** 50
- **B)** 60
- **C)** 70
- **D)** 80

**Correct Answer**: Option C (70)

**Trace**:
1. Initial Queue `q`: `[10 (front), 20, 30 (rear)]`.
2. `DEQUEUE(q)` removes `10`. `PUSH(10)` onto `s` $\implies s = [10]$.
3. `DEQUEUE(q)` removes `20`. `PUSH(20)` onto `s` $\implies s = [10, 20]$ (top is 20).
4. `POP(s)` pops `20`. `ENQUEUE(20)` into `q` $\implies q = [30 \text{ (front)}, 20 \text{ (rear)}]$.
5. `PUSH(40)` onto `s` $\implies s = [10, 40]$ (top is 40).
6. Result: $\text{TOP}(s) + \text{FRONT}(q) = 40 + 30 = 70$.

**5-Second Shortcut**: Queue is FIFO (10, 20 popped); Stack is LIFO (20 popped first and appended to rear of queue). Top of stack is 40; Front of queue is 30. $40 + 30 = 70$.  
**Trap**: Treating the queue as LIFO or confusing the front of the queue (`30`) with the newly inserted rear element (`20`).

---

## 7. Assessment Strategy & Speed Heuristics

| Construct | Trap to Avoid | Quick Shortcut |
| :--- | :--- | :--- |
| **Bitwise Operators** | Assuming arithmetic operators execute after shifts | `+` and `-` evaluate before `<<` and `>>` |
| **Boolean Bitwise Identity** | Manually computing `(A & B) \| (A ^ B)` | $(A \ \& \ B) \mid (A \oplus B) \equiv A \mid B$ |
| **Stack vs Queue** | Confusing `TOP` with `FRONT` | Stack is LIFO (top); Queue is FIFO (front popped first) |
| **Short-Circuiting** | Calculating both sides of `&&` or `\|\|` unconditionally | If left of `&&` is `0`, stop. If left of `\|\|` is non-zero, stop. |
| **Static / Global Variables** | Adding incremental $x$ during descent | Static variables hold their final incremented value when unwinding |
| **Nested Loop Bounds** | Missing scope re-initialization | Check if the inner variable is declared inside or outside outer loop |
| **Pseudo-code Bounds** | Treating loop as `i < b` instead of `i <= b` | `for i = a to b` includes both $a$ and $b$ |
| **Sum of Digits** | Tracing multi-pass loops manually under time pressure | $\text{Digital Root} = 1 + ((n - 1) \pmod 9)$ |
| **Pre/Post Increment** | Mixing up pre- and post-increment side-effects | `a++` uses original value; `++a` increments first |
| **Bitwise Shifts** | Manually writing out binary shifts | `x << k` $= x \cdot 2^k$; `x >> k` $= \lfloor x / 2^k \rfloor$ |
| **XOR Properties** | Misidentifying XOR variable swap logic | $x \oplus x = 0$; $x \oplus 0 = x$ |
| **Parity Testing** | Using `num % 2 == 1` which fails on negative numbers | Use `(num & 1) != 0` or `(a ^ b) & 1` |
| **Two-Pointer Scan** | Forgetting pointer updates causing TLE | Moves inwards ($L \to, \leftarrow R$) in $O(N)$ time |
