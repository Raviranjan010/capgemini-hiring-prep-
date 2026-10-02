# Deductive Reasoning & Formal Logic

**Tag**: [VIDEO]  
**Video Reference**: [Capgemini Deductive Logic Live Walkthrough](https://youtu.be/o5TbT3kzEnA)

---

## 1. Actual Exam Questions from Video

### Question 1: Conversion Logic (Syllogism)
**Tag**: [VIDEO]

**Premise**:  
*"Some of the employees who work day shifts also work double shifts."*

**Options**:
- **A)** All employees who work day shifts also work double shifts.
- **B)** Some employees who work day shifts do not work double shifts.
- **C)** None of the employees who work day shifts also work double shifts.
- **D)** Some of the employees who work double shifts also work day shifts.

**Correct Answer**: Option D

**Why Each Wrong Option is False**:
- **Option A is False**: The premise asserts an existential claim ("Some"), which cannot be generalized into a universal claim ("All").
- **Option B is False**: Known Correction 7: In formal deductive logic, *"Some A are B"* guarantees only set intersection ($A \cap B \ne \emptyset$). It does **not** guarantee that *"Some A are not B"*, because it remains logically possible that all day shift workers work double shifts. Common sense assumes "some implies not all", but formal logic strictly rejects this assumption.
- **Option C is False**: Directly contradicts the premise that at least some day shift employees do work double shifts.

**Logical Derivation**:  
The statement format is *"Some A are B"*. In set theory, intersection is commutative:
$$A \cap B = B \cap A \iff \text{"Some B are A"}$$
Therefore, *"Some of the employees who work double shifts also work day shifts"* is strictly and unconditionally true.

**5-Second Shortcut**: *"Some A are B"* converts symmetrically to *"Some B are A"*.  
**Trap**: Picking Option B based on colloquial everyday speech rather than formal propositional logic.

---

### Question 2: Conditional Logic & The Contrapositive Trap
**Tag**: [VIDEO]

**Premise**:  
*"All senior managers are required to attend a quarterly strategic meeting."*

**Options**:
- **A)** Everyone attending the strategic meeting is a senior manager.
- **B)** Anyone who is not a senior manager is not required to attend.
- **C)** Anyone who is required to attend the meeting is a senior manager.
- **D)** Anyone who is not required to attend the quarterly strategy meeting is not a senior manager.

**Correct Answer**: Option D

**Why Each Wrong Option is False**:
- **Option A is False**: Other employees (e.g., directors, executive assistants, presenters) may also attend.
- **Option B is False**: Commits the **Inverse Fallacy** ($\neg P \implies \neg Q$). Denying the antecedent does not deny the consequent; non-managers might still be required to attend.
- **Option C is False**: Commits the **Converse Fallacy** ($Q \implies P$). Affirming the consequent is an invalid logical deduction.

**Logical Derivation**:  
The statement format is a conditional implication:
$$P \implies Q \quad (\text{If Senior Manager } \implies \text{Required to attend})$$
In formal logic:
- The Converse ($Q \implies P$) is **NOT** necessarily true.
- The Inverse ($\neg P \implies \neg Q$) is **NOT** necessarily true.
- The **Contrapositive** ($\neg Q \implies \neg P$) is logically equivalent to $P \implies Q$ and is **strictly TRUE**.
If an individual is not required to attend ($\neg Q$), they cannot be a senior manager ($\neg P$).

**5-Second Shortcut**: Conditional $P \implies Q$ has only one guaranteed inference: the contrapositive $\neg Q \implies \neg P$.  
**Trap**: Falling for the converse (Option C) or inverse (Option B).

---

## 2. Deductive Reasoning Rules Cheat Table

| Given Premise | Valid Deductive Conclusion | Invalid Fallacies to Avoid |
| :--- | :--- | :--- |
| **All A are B** | • **Some B are A**<br>• **If not B, then not A** ($\neg B \implies \neg A$) | • *All B are A* (Converse error)<br>• *If not A, then not B* (Inverse error) |
| **No A are B** | • **No B are A**<br>• *Some A are not B* (valid under traditional logic assuming A is non-empty) | • *Some A are B* (Direct contradiction)<br>• *No B are not A* |
| **Some A are B** | • **Some B are A** (Conversion) | • *Some A are not B* (Cannot guarantee; All A could be B)<br>• *All A are B* |
| **$P \implies Q$** | • **$\neg Q \implies \neg P$** (Contrapositive) | • $Q \implies P$ (Converse fallacy)<br>• $\neg P \implies \neg Q$ (Inverse fallacy) |

---

## 3. Conditional Variants Breakdown (Senior Manager Example)

- **Original Statement ($P \implies Q$)**:  
  *"If an employee is a senior manager, they are required to attend."* (Premise: TRUE)
- **Converse ($Q \implies P$)**:  
  *"If an employee is required to attend, they are a senior manager."*  
  $\implies$ **INVALID / Fallacy** (A lead engineer might also be required to attend).
- **Inverse ($\neg P \implies \neg Q$)**:  
  *"If an employee is not a senior manager, they are not required to attend."*  
  $\implies$ **INVALID / Fallacy** (Non-managers are not banned from attending).
- **Contrapositive ($\neg Q \implies \neg P$)**:  
  *"If an employee is not required to attend, they are not a senior manager."*  
  $\implies$ **VALID / EQUIVALENT** (Because all senior managers must attend, anyone excused cannot be one).

---

## 4. Extra Practice Syllogism MCQs

### Question 3: Universal Negative Deduction
**Tag**: [ADDED]

**Premise**:  
*"No software bugs are acceptable in production releases."*

**Options**:
- **A)** Some acceptable items in production are software bugs.
- **B)** All unacceptable items in production are software bugs.
- **C)** Nothing that is acceptable in production releases is a software bug.
- **D)** Some software bugs are acceptable in production releases.

**Correct Answer**: Option C

**Why**:  
The proposition is a universal negative statement *"No A are B"*. By formal conversion, *"No A are B"* implies *"No B are A"*. Thus, no item acceptable in production can be a software bug.

**5-Second Shortcut**: *"No A are B"* converts symmetrically to *"No B are A"*.  
**Trap**: Selecting B, which erroneously assumes software bugs are the *only* unacceptable items.

---

### Question 4: Transitive Chain Deduction
**Tag**: [ADDED]

**Premises**:  
1. *"All microservices in Cluster A require OAuth authentication."*  
2. *"All services requiring OAuth authentication must log requests to the central audit vault."*

**Options**:
- **A)** Some microservices in Cluster A do not log requests to the central audit vault.
- **B)** All microservices in Cluster A must log requests to the central audit vault.
- **C)** All services that log requests to the central audit vault belong to Cluster A.
- **D)** Services outside Cluster A do not require OAuth authentication.

**Correct Answer**: Option B

**Why**:  
This is a standard hypothetical syllogism: $A \implies B$ and $B \implies C$, which deductively guarantees $A \implies C$. Since all Cluster A services require OAuth, and all OAuth services log to the audit vault, every Cluster A microservice must log to the vault.

**5-Second Shortcut**: Chain $A \implies B \implies C$ guarantees $A \implies C$.  
**Trap**: Selecting C (converse fallacy on the entire chain).

---

### Question 5: Some & All Combination
**Tag**: [ADDED]

**Premises**:  
1. *"All backend developers understand relational databases."*  
2. *"Some backend developers contribute to open-source compilers."*

**Options**:
- **A)** All contributors to open-source compilers understand relational databases.
- **B)** Some developers who understand relational databases contribute to open-source compilers.
- **C)** No contributors to open-source compilers understand relational databases.
- **D)** Anyone who understands relational databases is a backend developer.

**Correct Answer**: Option B

**Why**:  
Let $B$ be backend developers, $R$ be understand relational databases, and $C$ be contribute to compilers. From premise 2, there is an overlap between $B$ and $C$ ($B \cap C \ne \emptyset$). Since all $B$ are in $R$, the shared members must also be in $R$ ($R \cap C \ne \emptyset$). Hence, *"Some developers who understand relational databases contribute to open-source compilers"*.

**5-Second Shortcut**: "All $B$ are $R$" + "Some $B$ are $C$" $\implies$ "Some $R$ are $C$".  
**Trap**: Generalizing to Option A ("All contributors").
