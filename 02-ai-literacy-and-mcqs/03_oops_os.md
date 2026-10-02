# CS Fundamentals: OOPs & Operating Systems

## OOPs Core Concepts & Mechanics

### Question 1: C++ Diamond Problem & Virtual Inheritance
**Tag**: [CHAT]

**Code Scenario**:
```cpp
class A {
public:
    int val;
};

class B : virtual public A { };
class C : virtual public A { };
class D : public B, public C { };
```

**What Happens Without `virtual`**:
If classes `B` and `C` inherit from `A` non-virtually (`class B : public A`, `class C : public A`), then class `D` contains two distinct copies of `A` (one via `B` and one via `C`). Calling `D d; d.val = 10;` results in a compilation error:
`error: request for member 'val' is ambiguous`

**What Happens With `virtual public A`**:
The `virtual` keyword instructs the C++ compiler to create a single shared subobject of class `A` in the memory layout of derived class `D`. Both `B` and `C` share this instance via a virtual base table pointer (`vbptr`), resolving ambiguity.

**Question**:  
In C++, if class `D` inherits from classes `B` and `C`, and both `B` and `C` inherit from class `A`, how do you prevent duplicate subobjects of `A` inside `D` and eliminate ambiguity?

- **A)** Make class `A` abstract by defining a pure virtual function.
- **B)** Use `virtual public A` inheritance when declaring classes `B` and `C`.
- **C)** Declare member variable `val` in `A` as `static`.
- **D)** Mark class `D` with the `final` keyword.

**Correct Answer**: Option B

**Why**:  
Virtual inheritance ensures that only one shared instance of the base class `A` exists in any derived class further down the hierarchy. This avoids multiple copies and prevents ambiguity when accessing base class members.

**5-Second Shortcut**: Diamond problem resolution = `virtual public` inheritance on intermediate classes.  
**Trap**: Making `val` static does not fix inheritance layout duplication.

---

### Question 2: Shallow Copy vs Deep Copy Pointer Mechanics
**Tag**: [CHAT]

**Scenario**:
```cpp
class Buffer {
public:
    int* data;
    Buffer(int val) {
        data = new int(val);
    }
    ~Buffer() {
        delete data; // Destructor frees allocated heap memory
    }
};
```

**Question**:  
If a developer relies on the default copy constructor to copy an object of class `Buffer` (`Buffer b2 = b1;`), what critical runtime bug occurs when both objects exit scope?

- **A)** Memory Leak because heap memory is never freed.
- **B)** Dangling pointer followed by a Double Free Segmentation Fault.
- **C)** Stack overflow due to infinite recursive copy constructor calls.
- **D)** Deadlock on the shared object mutex.

**Correct Answer**: Option B

**Why**:  
The default copy constructor performs a shallow (bitwise) copy. It copies the pointer address `data` directly into `b2.data`, meaning both objects point to the identical heap address. When `b1` is destroyed, its destructor deletes the memory. The pointer in `b2` becomes a dangling pointer. When `b2` is destroyed, it attempts to delete the already-freed memory, triggering a double free crash.

**5-Second Shortcut**: Shallow copy of raw heap pointer = Dangling pointer + Double Free crash.  
**Trap**: Thinking shallow copy causes a memory leak (it causes double free, which is an illegal memory deallocation).

---

### Question 3: Virtual Functions & VTable Late Binding
**Tag**: [ADDED]

**Question**:  
In C++, how does the runtime engine determine which overridden method to execute when invoking a virtual function through a base class pointer (`Base* ptr = new Derived(); ptr->show();`)?

- **A)** The compiler inlines the method during the compilation phase.
- **B)** The runtime uses the hidden `vptr` inside the object to look up the function pointer in the `vtable`.
- **C)** The operating system performs an interrupt-driven dynamic system call.
- **D)** The linker rewrites the virtual method call at load time.

**Correct Answer**: Option B

**Why**:  
Every class with at least one virtual function has a compiler-generated Virtual Method Table (`vtable`) containing function pointers. Each object contains a hidden pointer (`vptr`) pointing to its class's `vtable`. At runtime, `ptr->show()` resolves the function address dynamically through `vptr->vtable`, enabling runtime polymorphism (late binding).

**5-Second Shortcut**: Dynamic dispatch / virtual function call = `vptr` lookup in `vtable`.  
**Trap**: Assuming virtual function resolution occurs at compile time or link time.

---

### Question 4: Abstract Class vs Interface
**Tag**: [ADDED]

| Feature | Abstract Class (C++ / Java) | Interface (Java) |
| :--- | :--- | :--- |
| **Methods** | Can have concrete methods and pure virtual / abstract methods | Historically pure abstract; modern Java allows default/static methods |
| **State / Variables** | Can declare instance fields, states, and constructors | Only `public static final` constants; no instance state |
| **Inheritance** | Single inheritance (class extends only one abstract class) | Multiple inheritance (class implements multiple interfaces) |
| **Speed** | Direct/offset lookup | Lookups through interface table (`itable`) |

**Question**:  
When should a software architect choose an Abstract Class over an Interface in an object-oriented system?

- **A)** When the class needs to participate in multiple inheritance of implementation.
- **B)** When subclasses must share common mutable instance state and protected non-static member fields.
- **C)** When only method signatures need to be defined with zero shared code.
- **D)** When all methods must be declared as static.

**Correct Answer**: Option B

**Why**:  
Interfaces define pure behavioral contracts without holding state or instance fields. An abstract class allows developers to define internal member variables, constructor logic, and shared reusable method implementations that derived classes inherit.

**5-Second Shortcut**: Shared state / member variables = Abstract Class; Pure behavioral contract = Interface.  
**Trap**: Assuming interfaces can store mutable non-static instance fields.

---

## Operating Systems Core Concepts & Mechanics

### Question 5: Semaphore: Binary vs Counting
**Tag**: [ADDED]

**Mechanics**:
- **Binary Semaphore**: Can only take integer values `0` or `1`. Acts similarly to a mutual exclusion lock (Mutex) for synchronizing critical section access.
- **Counting Semaphore**: Can take non-negative integer values $\ge 0$. Initialized to integer $N$ representing the number of available shared resource instances.

**Question**:  
A system has 4 identical database connections available in a connection pool. Which synchronization primitive is most suitable to manage access so that at most 4 threads access the pool concurrently?

- **A)** Binary Semaphore initialized to 1.
- **B)** Counting Semaphore initialized to 4.
- **C)** Spinlock with a busy-waiting loop.
- **D)** Condition variable without a mutex.

**Correct Answer**: Option B

**Why**:  
A counting semaphore initialized to $N = 4$ allows up to 4 threads to decrement the semaphore (`wait()` / `P()`) and access resources simultaneously. A 5th thread will block until one of the active threads releases a connection (`signal()` / `V()`).

**5-Second Shortcut**: Managing $N$ identical resource units = Counting Semaphore initialized to $N$.  
**Trap**: A binary semaphore restricts access to 1 thread, starving the other 3 available connections.

---

### Question 6: Deadlock Coffman Conditions
**Tag**: [CHAT]

**The 4 Simultaneous Coffman Conditions**:
1. **Mutual Exclusion**: At least one resource must be held in a non-shareable mode (e.g., a printer cannot print two jobs simultaneously).
2. **Hold and Wait**: A process holds at least one resource while waiting to acquire additional resources held by other processes (e.g., Process holding printer requests scanner).
3. **No Preemption**: Resources cannot be forcibly seized from a process; they can only be released voluntarily (e.g., OS cannot yank the printer from a running job).
4. **Circular Wait**: A closed chain of processes exists such that each process holds a resource that the next process in the chain requests ($P_0 \to P_1 \to P_2 \to P_0$).

**Question**:  
Which strategy prevents deadlock by violating the **Hold and Wait** condition?

- **A)** Enforcing a global linear order on all resource acquisitions.
- **B)** Requiring processes to request and be allocated all necessary resources at once before beginning execution.
- **C)** Making all system resources strictly non-shareable.
- **D)** Permitting processes to preempt resources held by higher-priority threads.

**Correct Answer**: Option B

**Why**:  
Hold and Wait occurs when a process holds resources while waiting for more. Forcing a process to request all required resources simultaneously upfront (or release all held resources before requesting new ones) completely eliminates Hold and Wait.

**5-Second Shortcut**: Violating Hold & Wait = Request all resources at once before execution.  
**Trap**: Linear resource ordering violates Circular Wait, not Hold and Wait.

---

### Question 7: Banker's Algorithm Safe State Determination
**Tag**: [CHAT]

> Corrected: Banker's Algorithm uses resource matrices (Available, Max, Allocation, Need = Max - Allocation) to check for a safe sequence, NOT a resource-allocation graph.

**Tiny Worked Example**:
- **Processes**: $P_0, P_1, P_2$
- **Resource Type**: Single resource type $R$ with Total instances = $10$.
- **Allocation Vector**:
  - $P_0$: $2$
  - $P_1$: $3$
  - $P_2$: $2$
  - *Total Allocated* = $2 + 3 + 2 = 7$.
  - *Available* = $10 - 7 = \mathbf{3}$.
- **Max Demand Matrix**:
  - $P_0$: $5$
  - $P_1$: $5$
  - $P_2$: $7$
- **Need Matrix ($\text{Need} = \text{Max} - \text{Allocation}$)**:
  - $P_0$: $5 - 2 = \mathbf{3}$
  - $P_1$: $5 - 3 = \mathbf{2}$
  - $P_2$: $7 - 2 = \mathbf{5}$

**Execution Trace**:
1. Current $\text{Available} = 3$.
2. Check $P_1$: $\text{Need}_1 (2) \le \text{Available} (3)$. $P_1$ runs to completion and releases its allocation ($3$).
   $\text{New Available} = 3 + 3 = 6$.
3. Check $P_0$: $\text{Need}_0 (3) \le \text{Available} (6)$. $P_0$ runs to completion and releases its allocation ($2$).
   $\text{New Available} = 6 + 2 = 8$.
4. Check $P_2$: $\text{Need}_2 (5) \le \text{Available} (8)$. $P_2$ runs to completion and releases its allocation ($2$).
   $\text{New Available} = 8 + 2 = 10$.
5. A safe sequence exists: $\langle P_1, P_0, P_2 \rangle$ (or $\langle P_0, P_1, P_2 \rangle$). System is in a **SAFE** state.

**Question**:  
In the Banker's Algorithm, what guarantees that a system state is considered "Safe"?

- **A)** The system currently has zero processes requesting resources.
- **B)** There exists at least one order (safe sequence) in which all processes can finish without entering deadlock.
- **C)** The Available vector is strictly greater than the sum of all Max demands.
- **D)** The Resource Allocation Graph contains at least one cycle.

**Correct Answer**: Option B

**Why**:  
A state is safe if there exists a safe sequence $\langle P_1, P_2, \dots, P_n \rangle$ such that for each $P_i$, the resources that $P_i$ can still request can be satisfied by current available resources plus resources held by all prior processes $P_j$ ($j < i$).

**5-Second Shortcut**: Safe state = at least one execution sequence exists where all processes finish.  
**Trap**: Confusing a safe state with having surplus resources for all processes simultaneously.

---

### Question 8: Page Replacement Policies: FIFO, LRU & Optimal
**Tag**: [ADDED]

**Worked Example**:
- **Page Frames**: $3$
- **Reference String**: `7, 0, 1, 2, 0, 3, 0, 4`

| Step | Page | FIFO (3 Frames) | LRU (3 Frames) | Optimal (3 Frames) |
| :---: | :---: | :---: | :---: | :---: |
| 1 | **7** | `[7]` (Miss) | `[7]` (Miss) | `[7]` (Miss) |
| 2 | **0** | `[7, 0]` (Miss) | `[7, 0]` (Miss) | `[7, 0]` (Miss) |
| 3 | **1** | `[7, 0, 1]` (Miss) | `[7, 0, 1]` (Miss) | `[7, 0, 1]` (Miss) |
| 4 | **2** | Replace 7 $\to$ `[2, 0, 1]` (Miss) | Replace 7 $\to$ `[2, 0, 1]` (Miss) | Replace 7 $\to$ `[2, 0, 1]` (Miss) |
| 5 | **0** | `[2, 0, 1]` (**Hit**) | `[2, 0, 1]` (**Hit**) | `[2, 0, 1]` (**Hit**) |
| 6 | **3** | Replace 0 $\to$ `[2, 3, 1]` (Miss) | Replace 1 $\to$ `[2, 0, 3]` (Miss) | Replace 1 $\to$ `[2, 0, 3]` (Miss) |
| 7 | **0** | Replace 1 $\to$ `[2, 3, 0]` (Miss) | `[2, 0, 3]` (**Hit**) | `[2, 0, 3]` (**Hit**) |
| 8 | **4** | Replace 2 $\to$ `[4, 3, 0]` (Miss) | Replace 2 $\to$ `[4, 0, 3]` (Miss) | Replace 2 $\to$ `[4, 0, 3]` (Miss) |
| **Total** | | **7 Page Faults (1 Hit)** | **6 Page Faults (2 Hits)** | **5 Page Faults (3 Hits)** |

**Question**:  
Why can the Optimal Page Replacement algorithm (OPT / Belady's Min) not be implemented in general-purpose operating systems?

- **A)** It causes Belady's Anomaly when frame allocation increases.
- **B)** It requires perfect future knowledge of page reference strings, which is impossible at runtime.
- **C)** It has exponential $O(2^N)$ time complexity.
- **D)** It can only be used on single-core processors.

**Correct Answer**: Option B

**Why**:  
The Optimal algorithm replaces the page that will not be used for the longest period in the future. Because an operating system cannot predict future program branching and user inputs, OPT is physically impossible to implement and serves only as a benchmark against which real algorithms like LRU are measured.

**5-Second Shortcut**: Optimal page replacement = theoretical benchmark (requires future knowledge).  
**Trap**: Optimal algorithm never experiences Belady's anomaly (only FIFO does).

---

## Extra Practice (Added MCQs)

### Question 9: Belady's Anomaly in Paging
**Tag**: [ADDED]

**Question**:  
Which page replacement algorithm can experience **Belady's Anomaly** (where increasing the number of physical page frames results in an increase in the number of page faults)?

- **A)** Least Recently Used (LRU)
- **B)** First-In, First-Out (FIFO)
- **C)** Optimal Page Replacement (OPT)
- **D)** Most Recently Used (MRU)

**Correct Answer**: Option B

**Why**:  
LRU and Optimal belong to a class of algorithms called "stack algorithms," where the set of pages in memory for $N$ frames is always a subset of the pages in memory for $N+1$ frames. FIFO is not a stack algorithm, so increasing the frame count can displace pages needed soon after, causing more total faults.

**5-Second Shortcut**: Belady's Anomaly = FIFO algorithm.  
**Trap**: Believing LRU or Optimal can exhibit Belady's Anomaly.

---

### Question 10: Process vs Thread Memory Layout
**Tag**: [ADDED]

**Question**:  
When multiple threads execute within the same process, which memory segment is private to each individual thread rather than shared?

- **A)** Heap memory
- **B)** Global and static data segment
- **C)** Program code (text) segment
- **D)** Stack memory and register set

**Correct Answer**: Option D

**Why**:  
Threads within the same process share the process's address space, including the code segment, data segment, open file handles, and heap. However, each thread must have its own private stack and CPU register set (including the program counter) to track independent function execution.

**5-Second Shortcut**: Private to thread = Stack & Registers; Shared across threads = Heap, Code, Data.  
**Trap**: Assuming the heap is allocated per-thread.

---

### Question 11: Dynamic Polymorphism & Object Slicing
**Tag**: [ADDED]

**Question**:  
In C++, what occurs when a derived class object `Derived d` is assigned to a base class object by value (`Base b = d;`) rather than by reference or pointer?

- **A)** Virtual method dispatch functions normally.
- **B)** Object Slicing occurs: the derived-specific member variables are sliced off, and `b` behaves as a pure `Base` instance.
- **C)** Compilation error: assigning derived objects to base types is forbidden.
- **D)** A runtime `bad_cast` exception is thrown.

**Correct Answer**: Option B

**Why**:  
When assigning by value, the base copy constructor allocates memory only for `sizeof(Base)`. All additional members and virtual table bindings of `Derived` are omitted ("sliced off"), discarding runtime polymorphism.

**5-Second Shortcut**: Pass/assign derived by value to base = Object Slicing (polymorphism lost).  
**Trap**: Expecting virtual methods to be invoked polymorphically on an object sliced by value.

---

### Question 12: Preemptive CPU Scheduling: SRTF Overhead
**Tag**: [ADDED]

**Question**:  
Shortest Remaining Time First (SRTF) is the preemptive version of SJF and achieves minimum average waiting time. What is its primary practical drawback in real-time operating systems?

- **A)** High context-switching overhead and potential starvation of long CPU-burst processes.
- **B)** Susceptibility to priority inversion without semaphores.
- **C)** Requirement for non-maskable hardware interrupts.
- **D)** Inability to execute on multi-threaded processors.

**Correct Answer**: Option A

**Why**:  
Frequent preemption whenever a shorter process arrives causes high context-switching CPU overhead. Furthermore, if short processes continuously enter the ready queue, long jobs may experience perpetual starvation.

**5-Second Shortcut**: SRTF drawback = High context switches + Starvation of long processes.  
**Trap**: Thinking SRTF causes deadlocks.

---

### Question 13: Virtual Memory Operating Systems - Thrashing
**Tag**: [MOCK-EXAM]

**Question**:  
What causes thrashing in a virtual memory operating system?

- **A)** Deadlock occurring inside high-priority driver threads.
- **B)** The operating system spending substantially more time swapping pages in and out of secondary storage than executing process instructions.
- **C)** High CPU temperature triggering clock frequency scaling.
- **D)** Fragmented disk sectors during sequential I/O requests.

**Correct Answer**: Option B

**Why**:  
Thrashing occurs when the aggregate working sets of all active processes exceed the physical memory (RAM) frames available. This triggers a cascading chain of page faults where pages are continuously evicted to disk and immediately faulted back in, causing near-zero CPU throughput and high disk I/O queuing.

**5-Second Shortcut**: Thrashing = System spends more time page swapping than executing code.  
**Trap**: Confusing thrashing with process deadlock or CPU throttling.

