# CS Fundamentals: Networking, SQL & DBMS

## High-Yield Questions & Mechanics

### Question 1: Usable Host Calculation in Subnetting
**Tag**: [CHAT]

**Question**:  
Given the IPv4 subnet mask `255.255.255.192`, how many usable host IP addresses are available in this subnet?

- **A)** 64
- **B)** 62
- **C)** 32
- **D)** 30

**Correct Answer**: Option B

**Calculation & Binary Steps**:
1. Focus on the interesting (last) octet: `192`.
2. Convert to 8-bit binary: $192 = 128 + 64 = 11000000_2$.
3. Count the host bits ($H$ = number of zero bits): 6 zero bits ($H = 6$).
4. Total IP addresses = $2^H = 2^6 = 64$.
5. Subtract Network ID and Broadcast IP: Usable host IPs = $2^6 - 2 = 64 - 2 = \mathbf{62}$.

**Why**:  
Every subnet reserves the very first address as the Network Identifier (all host bits 0) and the very last address as the Subnet Broadcast Identifier (all host bits 1). These two addresses cannot be assigned to network interfaces.

**5-Second Shortcut**: $\text{Usable hosts} = 2^{(\text{zero bits})} - 2 = 2^6 - 2 = 62$.  
**Trap**: Selecting 64 without subtracting the network and broadcast addresses.

---

### Question 2: SQL Three-Valued Logic (`NULL` Comparison)
**Tag**: [CHAT]

**Question**:  
Consider a table named `Employees` where exactly 10 rows have `Salary = NULL` and 10 rows have `Salary = 50000`. What is the result returned by the following query?

```sql
SELECT COUNT(*) FROM Employees WHERE Salary = NULL;
```

- **A)** 10
- **B)** 20
- **C)** 0
- **D)** Syntax Error

**Correct Answer**: Option C

**Why**:  
SQL uses three-valued logic (TRUE, FALSE, UNKNOWN). `NULL` represents an unknown/missing value. Any equality or inequality comparison against `NULL` using `=`, `!=`, or `<>` evaluates to `UNKNOWN`. In a `WHERE` clause, only conditions that evaluate to `TRUE` are returned. Because `UNKNOWN` is falsy for filtering, zero rows match.

**5-Second Shortcut**: `NULL` needs `IS NULL`; `Salary = NULL` always returns 0 rows.  
**Trap**: Expecting `Salary = NULL` to return the 10 rows containing null values.

---

### Question 3: Aggregation Filtering: WHERE vs HAVING Clause
**Tag**: [CHAT]

**Query Execution Mechanics**:
```sql
SELECT DepartmentID, AVG(Salary)
FROM Employees
WHERE Status = 'Active'
GROUP BY DepartmentID
HAVING AVG(Salary) > 60000;
```
- **WHERE**: Evaluated *before* grouping and aggregation; filters individual base rows. Using aggregate functions like `AVG()` in a `WHERE` clause causes a compile/syntax error.
- **HAVING**: Evaluated *after* grouping and aggregation; filters summarized groups.

**Question**:  
Which of the following queries correctly filters departments where the average employee compensation is strictly greater than 75,000?

- **A)** `SELECT DeptID, AVG(Salary) FROM Emp WHERE AVG(Salary) > 75000 GROUP BY DeptID;`
- **B)** `SELECT DeptID, AVG(Salary) FROM Emp GROUP BY DeptID HAVING AVG(Salary) > 75000;`
- **C)** `SELECT DeptID, AVG(Salary) FROM Emp GROUP BY DeptID WHERE AVG(Salary) > 75000;`
- **D)** `SELECT DeptID, AVG(Salary) FROM Emp HAVING AVG(Salary) > 75000;`

**Correct Answer**: Option B

**Why**:  
The `WHERE` clause filters individual rows prior to group creation and cannot process aggregate functions like `AVG()`. Aggregate conditions must be placed in the `HAVING` clause, which executes post-aggregation on the grouped records.

**5-Second Shortcut**: Aggregates (`AVG`, `SUM`, `COUNT`) belong strictly in `HAVING`, never in `WHERE`.  
**Trap**: Option A throws a syntax error because `AVG(Salary)` is placed inside `WHERE`.

---

### Question 4: Second Highest Salary in SQL
**Tag**: [CHAT]

**Implementation Patterns**:
- **Method A (MySQL / PostgreSQL - LIMIT & OFFSET)**:
  ```sql
  SELECT DISTINCT Salary 
  FROM Employees 
  ORDER BY Salary DESC 
  LIMIT 1 OFFSET 1;
  ```
- **Method B (ANSI SQL - Subquery / Portable)**:
  ```sql
  SELECT MAX(Salary) 
  FROM Employees 
  WHERE Salary < (SELECT MAX(Salary) FROM Employees);
  ```

**Question**:  
Which query reliably retrieves the second highest unique salary across any ANSI SQL-compliant database, even if multiple employees share the highest salary?

- **A)** `SELECT Salary FROM Employees ORDER BY Salary DESC LIMIT 2;`
- **B)** `SELECT MAX(Salary) FROM Employees WHERE Salary < (SELECT MAX(Salary) FROM Employees);`
- **C)** `SELECT Salary FROM Employees WHERE Salary = (MAX(Salary) - 1);`
- **D)** `SELECT TOP 2 Salary FROM Employees ORDER BY Salary ASC;`

**Correct Answer**: Option B

**Why**:  
The inner subquery `(SELECT MAX(Salary) FROM Employees)` finds the absolute highest salary. The outer query filters out any salary equal to or greater than that maximum, and finds the maximum of the remaining salaries, which is guaranteed to be the second highest unique salary.

**5-Second Shortcut**: Second max = `MAX(Salary) WHERE Salary < (SELECT MAX(Salary) FROM Employees)`.  
**Trap**: `LIMIT 1 OFFSET 1` without `DISTINCT` fails if two employees tie for the top salary.

---

### Question 5: Self Join Mechanics & Employee-Manager Hierarchy
**Tag**: [ADDED]

**Concept**:  
A Self Join is a regular join where a table is joined with itself using table aliases. It is commonly used to query hierarchical or recursive relationships stored within a single table.

```sql
SELECT E.EmpName AS Employee, M.EmpName AS Manager
FROM Employees E
LEFT JOIN Employees M ON E.ManagerID = M.EmpID;
```

**Question**:  
Given an `Employees` table with columns `(EmpID, EmpName, ManagerID)`, which join type ensures that employees who do not report to any manager (e.g., the CEO with `ManagerID = NULL`) still appear in the output?

- **A)** `INNER JOIN`
- **B)** `LEFT JOIN`
- **C)** `CROSS JOIN`
- **D)** `NATURAL JOIN`

**Correct Answer**: Option B

**Why**:  
An `INNER JOIN` drops rows where the join condition fails, which excludes any employee whose `ManagerID` is `NULL`. A `LEFT JOIN` retains all rows from the left table (`Employees E`) and fills the manager columns with `NULL` when there is no matching manager record.

**5-Second Shortcut**: Hierarchy queries keeping top-level / CEO records require a `LEFT JOIN`.  
**Trap**: `INNER JOIN` silently excludes the CEO.

---

### Question 6: SQL Three-Valued Logic: NULL Arithmetic & Expressions
**Tag**: [CHAT]

**Question**:  
What are the resulting evaluations of the expressions (1) `SELECT NULL + 5;` and (2) `SELECT NULL = NULL;` in standard SQL?

- **A)** (1) `5`, (2) `TRUE`
- **B)** (1) `NULL`, (2) `UNKNOWN`
- **C)** (1) `0`, (2) `TRUE`
- **D)** (1) `NULL`, (2) `TRUE`

**Correct Answer**: Option B

**Why**:  
In SQL, arithmetic on an unknown value produces an unknown value: `NULL + 5 = NULL`. Furthermore, two unknown values cannot be asserted as equal to each other: `NULL = NULL` yields `UNKNOWN` (not `TRUE`).

**5-Second Shortcut**: Arithmetic with `NULL` gives `NULL`; comparing `NULL = NULL` yields `UNKNOWN`.  
**Trap**: Assuming `NULL = NULL` evaluates to `TRUE` (it does not).

---

## Extra Practice (Added MCQs)

### Question 7: Subnetting Host Capacity for Given Network Size
**Tag**: [ADDED]

**Question**:  
A network administrator needs to assign a subnet that supports at least 28 workstations while wasting the fewest possible IP addresses. Which CIDR subnet mask should be assigned?

- **A)** `/26` (`255.255.255.192`)
- **B)** `/27` (`255.255.255.224`)
- **C)** `/28` (`255.255.255.240`)
- **D)** `/25` (`255.255.255.128`)

**Correct Answer**: Option B

**Why**:  
For `/28`, host bits $H = 32 - 28 = 4$. Usable hosts = $2^4 - 2 = 14$ (too small). For `/27`, host bits $H = 32 - 27 = 5$. Usable hosts = $2^5 - 2 = 30$, which accommodates 28 workstations with minimum waste.

**5-Second Shortcut**: 28 hosts needs $2^H - 2 \ge 28 \implies H = 5 \implies 32 - 5 = /27$.  
**Trap**: Forgetting the $-2$ rule and picking `/28` thinking $2^4 = 16$ is close enough.

---

### Question 8: Unmatched Rows in SQL Joins
**Tag**: [ADDED]

**Question**:  
Table `Orders` has 100 rows, and Table `Customers` has 50 rows. Every order references a `CustomerID`, but 10 orders have a `CustomerID` that does not exist in `Customers`. How many rows are returned by an `INNER JOIN` on `CustomerID`?

- **A)** 100
- **B)** 90
- **C)** 50
- **D)** 110

**Correct Answer**: Option B

**Why**:  
An `INNER JOIN` strictly returns rows where the join predicate matches in both tables. Since 10 orders have unmatched customer IDs, those 10 rows are eliminated from the result set: $100 - 10 = 90$.

**5-Second Shortcut**: `INNER JOIN` output = Total rows $-$ Unmatched rows = $100 - 10 = 90$.  
**Trap**: Selecting 100 assuming foreign keys automatically force inclusion.

---

### Question 9: DBMS Transaction Isolation & Dirty Reads
**Tag**: [ADDED]

**Question**:  
Which ANSI SQL transaction isolation level allows a transaction to read uncommitted data written by another concurrent transaction (a "Dirty Read")?

- **A)** `READ COMMITTED`
- **B)** `READ UNCOMMITTED`
- **C)** `REPEATABLE READ`
- **D)** `SERIALIZABLE`

**Correct Answer**: Option B

**Why**:  
`READ UNCOMMITTED` is the lowest isolation level. It does not enforce shared read locks or check transaction commit status, allowing dirty reads. `READ COMMITTED` prevents dirty reads by ensuring only committed changes are visible.

**5-Second Shortcut**: Dirty Read permitted only in `READ UNCOMMITTED`.  
**Trap**: Assuming `READ COMMITTED` allows dirty reads because of the word "READ".

---

### Question 10: Database B+ Tree Index Complexity
**Tag**: [ADDED]

**Question**:  
Why do relational database engines (e.g., MySQL InnoDB, PostgreSQL) utilize B+ Trees instead of standard Binary Search Trees for disk-based table indexing?

- **A)** B+ Trees have $O(1)$ worst-case search complexity.
- **B)** B+ Trees have high fan-out (reducing disk I/O depth) and linked leaf nodes for efficient range scans.
- **C)** Binary Search Trees cannot store string data types.
- **D)** B+ Trees eliminate the need for primary keys.

**Correct Answer**: Option B

**Why**:  
Disk reads are expensive. A B+ Tree has high branching factor (fan-out), keeping tree depth small (typically 3 to 4 levels for millions of rows) and minimizing disk page accesses. All data pointers reside in leaf nodes, which are sequentially linked for fast $O(\log N + K)$ range queries.

**5-Second Shortcut**: B+ Tree indexing = low disk I/O depth + linked leaf nodes for range scans.  
**Trap**: Thinking B+ Trees are chosen for memory compression rather than disk I/O reduction.

---

### Question 11: Database Normalization - Second Normal Form (2NF)
**Tag**: [MOCK-EXAM]

**Question**:  
A relation $R(A, B, C, D)$ has a composite candidate key $(A, B)$. Under which condition does the relation violate Second Normal Form (2NF)?

- **A)** If there is a transitive dependency $C \to D$.
- **B)** If a non-prime attribute (e.g., $C$) depends on a proper subset of the candidate key (e.g., $A \to C$).
- **C)** If all functional dependencies have a candidate key on the left-hand side.
- **D)** If multivalued dependencies exist between $A$ and $B$.

**Correct Answer**: Option B

**Why**:  
Second Normal Form (2NF) requires the relation to be in 1NF and contain **no partial dependencies**. A partial dependency occurs when a non-prime attribute (an attribute not part of any candidate key) depends functionally on a proper subset of a composite candidate key (e.g., $A \to C$ when the key is $(A, B)$).

**5-Second Shortcut**: 2NF violation = Partial dependency on subset of candidate key.  
**Trap**: Confusing 2NF (no partial dependency) with 3NF (no transitive dependency $C \to D$).

---

### Question 12: Subnetting Network ID & Broadcast Address Calculation
**Tag**: [MOCK-EXAM]

**Question**:  
Given the IP address `192.168.10.65` with subnet mask `/26` (`255.255.255.192`), what is the Network ID and the Broadcast Address for this subnet?

- **A)** Network: 192.168.10.0, Broadcast: 192.168.10.63
- **B)** Network: 192.168.10.64, Broadcast: 192.168.10.127
- **C)** Network: 192.168.10.64, Broadcast: 192.168.10.255
- **D)** Network: 192.168.10.32, Broadcast: 192.168.10.95

**Correct Answer**: Option B

**Calculation**:
1. Subnet mask `/26` has $32 - 26 = 6$ host bits.
2. Block size (subnet increment) $= 2^6 = 64$.
3. Subnet ranges in the last octet:
   - Subnet 0: `0` to `63` (Network: `.0`, Broadcast: `.63`)
   - Subnet 1: `64` to `127` (Network: `.64`, Broadcast: `.127`)
   - Subnet 2: `128` to `191`
   - Subnet 3: `192` to `255`
4. The host IP `192.168.10.65` falls in Subnet 1:
   - **Network ID**: `192.168.10.64`
   - **Broadcast Address**: `192.168.10.127`

**5-Second Shortcut**: Block size $= 256 - 192 = 64$; $65$ lies in $[64, 127] \implies$ Net: `.64`, Broadcast: `.127`.  
**Trap**: Picking `.255` as broadcast; subnetting divides the octet into smaller broadcast domains.

---

### Question 13: Database Transaction Isolation Levels & Concurrency Phenomena
**Tag**: [MOCK-EXAM]

**Scenario**:  
Transaction $T_1$ reads a row where `account_balance = 500`. Before $T_1$ finishes, Transaction $T_2$ updates the same row to `account_balance = 800` and commits. Transaction $T_1$ reads the exact same row again and observes `account_balance = 800`.  
Which concurrency phenomenon occurred, and what minimum SQL isolation level is required to prevent it?

- **A)** Dirty Read; Prevented by READ COMMITTED
- **B)** Non-Repeatable Read; Prevented by REPEATABLE READ
- **C)** Phantom Read; Prevented by READ UNCOMMITTED
- **D)** Lost Update; Prevented by SERIALIZABLE only

**Correct Answer**: **Option B**

**Deep Explanation**:
- **Non-Repeatable Read (Fuzzy Read)** occurs when a transaction reads the same row twice, but a concurrent transaction modifies that row and commits between the two reads, causing $T_1$ to see changed values.
- **Minimum Isolation Level**: **REPEATABLE READ**. Under REPEATABLE READ, the DBMS maintains shared read locks until transaction completion (or uses multi-version concurrency control / MVCC snapshot isolation), guaranteeing that any row read by $T_1$ remains constant throughout $T_1$'s execution.

#### SQL Isolation Levels vs. Phenomena Matrix

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
| :--- | :---: | :---: | :---: |
| **READ UNCOMMITTED** | Allowed | Allowed | Allowed |
| **READ COMMITTED** | **Prevented** | Allowed | Allowed |
| **REPEATABLE READ** | **Prevented** | **Prevented** | Allowed |
| **SERIALIZABLE** | **Prevented** | **Prevented** | **Prevented** |

**5-Second Shortcut**: Same existing row modified and committed between two reads = **Non-Repeatable Read** $\implies$ Prevented by **REPEATABLE READ**.  
**Trap**: Confusing with Dirty Read (which reads *uncommitted* changes) or Phantom Read (which adds/deletes *new rows* matching a range).

