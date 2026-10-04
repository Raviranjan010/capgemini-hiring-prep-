[Home](../README.md) > 02-cs-fundamentals

# 02. Computer Science Fundamentals

This directory contains comprehensive theory, core concepts, formulas, and multiple-choice questions for the core Computer Science sections tested in the Capgemini assessment.

```mermaid
graph TD
    CS[Capgemini CS Fundamentals] --> Net[01. Computer Networks]
    CS --> SQL[02. SQL Queries & Joins]
    CS --> DBMS[03. DBMS Internals]
    CS --> OOP[04. OOPs & C++]
    CS --> OS[05. Operating Systems]
    CS --> DSA[06. DSA Theory]
    
    Net --> Net1[OSI 7 Layers & TCP/IP]
    Net --> Net2[Subnetting: 2^(32-n) - 2]
    
    SQL --> SQL1[Joins: Inner, Left, Full, Cross]
    SQL --> SQL2[GROUP BY, HAVING & NULLs]
    
    DBMS --> DBMS1[ACID & Schedules: Conflict Serializability]
    DBMS --> DBMS2[Normalization: 1NF -> 2NF -> 3NF -> BCNF]
    
    OOP --> OOP1[Vtable & Runtime Polymorphism]
    OOP --> OOP2[Virtual Base Class & Diamond Problem]
    
    OS --> OS1[Process States & CPU Scheduling]
    OS --> OS2[Banker's Algorithm & Page Tables]
    
    DSA --> DSA1[Asymptotic Bounds: O, Omega, Theta]
    DSA --> DSA2[BST vs AVL Balance Factor {-1,0,+1}]
```

## Module Structure

| File | What You Learn | Questions Inside | Time to Finish |
| :--- | :--- | :---: | :---: |
| [01_networking.md](01_networking.md) | OSI vs TCP/IP models, TCP 3-way handshake, UDP, DNS resolution, HTTP/HTTPS, subnet masking, CIDR host calculations | 30 | 35 mins |
| [02_sql.md](02_sql.md) | Inner/Outer joins, GROUP BY vs HAVING, 3-valued NULL logic, Correlated subqueries, Indexes (B-Tree/Hash), Window functions | 30 | 35 mins |
| [03_dbms.md](03_dbms.md) | Relational keys (Candidate/Super/Foreign), Normal forms (1NF to BCNF), ACID guarantees, Serializability, Concurrency & Locking | 30 | 35 mins |
| [04_oops.md](04_oops.md) | 4 Pillars, Virtual tables (vtable/vptr), Diamond problem, Deep vs Shallow copy, Friend functions, Abstract classes | 35 | 40 mins |
| [05_os.md](05_os.md) | CPU scheduling algorithms, Deadlock conditions & Banker's algorithm, Paging vs Segmentation, Page replacement, Mutex vs Semaphore | 35 | 40 mins |
| [06_dsa_theory.md](06_dsa_theory.md) | Big-O/Omega/Theta analysis, BST properties, Balance factor in AVL, Sorting comparison bounds, Circular queue conditions, Hashing collisions | 30 | 35 mins |

**Total Questions**: 190 questions across all 6 core disciplines.

---

## 💡 Top 6 CS Fundamentals Exam Shortcuts

1. **Subnetting Usable Host Formula**:
   - For CIDR `/n`: Total IPs = $2^{32-n}$.
   - **Usable Host IPs** = $2^{32-n} - 2$ (subtract 1 for Network ID, 1 for Broadcast ID).
   - E.g. `/27` $\implies 2^{32-27} - 2 = 2^5 - 2 = 30$ usable hosts.
2. **SQL 3-Valued Logic & NULLs**:
   - `NULL = NULL` is **UNKNOWN (False)**. Always use `IS NULL` or `IS NOT NULL`.
   - `COUNT(*)` counts all rows including NULLs; `COUNT(column_name)` ignores NULL rows!
3. **Normalization Cheat Rule**:
   - **1NF**: Atomic values (no repeating groups/multivalued attributes).
   - **2NF**: 1NF + No **partial dependency** (no non-prime attribute depends on a proper subset of candidate key).
   - **3NF**: 2NF + No **transitive dependency** ($X \to Y$ requires $X$ is superkey or $Y$ is prime attribute).
   - **BCNF**: For every functional dependency $X \to Y$, $X$ MUST be a **superkey**.
4. **OOPs Diamond Problem**:
   - Solved in C++ using `virtual public Base` inheritance so only one copy of `Base` is shared.
5. **Deadlock Banker's Need Matrix**:
   - $\text{Need}[i][j] = \text{Max}[i][j] - \text{Allocation}[i][j]$.
   - A process request can only be granted if $\text{Request} \le \text{Need}$ AND $\text{Request} \le \text{Available}$, and remaining state is safe.
6. **AVL Tree Balance Factor**:
   - $\text{BF} = \text{Height}(\text{Left Subtree}) - \text{Height}(\text{Right Subtree}) \in \{-1, 0, +1\}$.
   - If $|BF| > 1$, rebalance via Single Rotation (LL, RR) or Double Rotation (LR, RL).

---

Previous: [01-ai-literacy/README.md](../01-ai-literacy/README.md) | Next: [01_networking.md](01_networking.md)
