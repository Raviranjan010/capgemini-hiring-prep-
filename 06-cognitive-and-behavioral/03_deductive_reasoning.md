[Home](../README.md) > [06-cognitive-and-behavioral](README.md) > 03_deductive_reasoning.md

# Deductive Reasoning & Formal Logic

**Tag**: [VIDEO]  
**Video Reference**: [Capgemini Deductive Logic Live Walkthrough](https://youtu.be/o5TbT3kzEnA)

---

## 1. Actual Exam Questions from Video

### Question 1 (COG-015): Conversion Logic (Syllogism)
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

### Question 2 (COG-016): Conditional Logic & The Contrapositive Trap
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

### Question 3 (COG-017): Universal Negative Deduction
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

### Question 4 (COG-018): Transitive Chain Deduction
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

### Question 5 (COG-019): Some & All Combination
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

---

### Question 6 (COG-020): Universal Negative Conversion (E-Proposition)
**Tag**: [MOCK-EXAM]

**Premise**:  
*"No certified cloud architects in company $X$ are permitted to write production database migration scripts without peer review."*

**Which statement must be strictly true?**
- **A)** Some certified cloud architects write production migration scripts without peer review.
- **B)** Anyone permitted to write production database migration scripts without peer review is not a certified cloud architect in company $X$.
- **C)** All employees who undergo peer review are certified cloud architects.
- **D)** Anyone who is not a certified cloud architect can write migration scripts without review.

**Correct Answer**: **Option B**

**Explanation**:  
The premise is a **Universal Negative** ($\mathbf{E}$-proposition): $\text{No } A \text{ are } B$ ($A \cap B = \emptyset$).  
Its contrapositive formulation ensures that if someone belongs to set $B$ (permitted to write scripts without review), they cannot belong to set $A$ (certified cloud architects in company $X$).

---

### Question 7 (COG-021): Transitive Disjoint Syllogism Chain
**Tag**: [MOCK-EXAM]

**Premises**:  
1. *"All front-end repositories using framework $Z$ are subject to automated build pipelines."*  
2. *"None of the legacy monolithic microservices use automated build pipelines."*  

**Which statement is logically certain?**
- **A)** Some legacy monolithic microservices use framework $Z$.
- **B)** No front-end repository using framework $Z$ is a legacy monolithic microservice.
- **C)** All applications subject to automated pipelines are front-end repositories.
- **D)** Framework $Z$ cannot run on microservices.

**Correct Answer**: **Option B**

**Explanation**:  
Let $F \subseteq P$ ("All $F$ are $P$") and $M \cap P = \emptyset$ ("No $M$ are $P$").  
Since $F$ is a complete subset of $P$ and $M$ shares zero overlap with $P$, $F$ and $M$ are strictly disjoint:
$$F \cap M = \emptyset \implies \text{"No front-end repository using framework } Z \text{ is a legacy monolithic microservice."}$$

---

### Question 8 (COG-022): Sub-alternation Trap: Universal vs. Particular (All vs. Some)
**Tag**: [MOCK-EXAM]

**Premise**:  
*"Every consultant on project Alpha completed their sprint commitments early this quarter."*  

**Assuming the premise is true, which of the following assertions must also be true?**
- **A)** Only consultants on project Alpha finished early this quarter.
- **B)** Some consultants on project Alpha completed their sprint commitments early this quarter.
- **C)** Consultants on other projects failed to finish their sprints early.
- **D)** Every consultant who finished early belongs to project Alpha.

**Correct Answer**: **Option B**

**Explanation**:  
In formal logic, if a universal affirmative statement ($\mathbf{A}$: *"All $S$ are $P$"*) holds true for a non-empty set, the particular affirmative statement ($\mathbf{I}$: *"Some $S$ are $P$"*) is unconditionally true by **sub-alternation** ("All implies Some").  
Options A, C, and D introduce extraneous unstated assumptions about outside consultants.

---

### Question 9 (COG-023): Complex Conditional Deduction (Modus Tollens)
**Tag**: [MOCK-EXAM]

**Premise**:  
*"If an incident is designated as a Severity-1 event, then the incident commander must page the site reliability lead within five minutes."*  

**Scenario**: During a post-mortem review, records show that the site reliability lead was **not** paged within five minutes. What is the guaranteed deduction?
- **A)** The system encountered a network communication failure.
- **B)** The incident was not designated as a Severity-1 event.
- **C)** The site reliability lead was already active on the incident call.
- **D)** The incident commander failed company operating procedures.

**Correct Answer**: **Option B**

**Explanation**:  
This is classic **Modus Tollens**:
$$\text{Given: } P \implies Q \quad \text{and} \quad \neg Q$$
$$\text{Conclusion: } \neg P$$
Since $Q$ (*paging the lead within five minutes*) was false, $P$ (*incident designated as Severity-1*) must also be false.

## Practice Additions (Deductive Reasoning Expansion)

### COG-051: Particular Affirmative Trap: Some A are B

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Syllogisms

#### Statements
Statements:
1. Some doctors are teachers.
2. All teachers are researchers.

#### Conclusions
I. Some doctors are researchers.
II. Some doctors are not researchers.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both I and II follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Statement 1 gives an intersection between Doctors and Teachers.
2. Statement 2 places all Teachers inside Researchers.
3. Therefore, the doctors who are teachers are necessarily researchers. Conclusion I follows.
4. Crucial rule: 'Some A are B' does NOT imply 'Some A are not B'. In deductive logic, it is possible that ALL doctors are researchers. Hence Conclusion II does not follow.

- **5-Second Shortcut**: 'Some + All' gives 'Some': Doctors who are Teachers are Researchers. Never assume 'Some are not'.
- **Trap**: Assuming 'Some doctors are teachers' implies the remaining doctors are definitely not teachers.
- **Source**: Added practice

---
### COG-052: Two Negative Statements Produce No Conclusion

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Syllogisms

#### Statements
Statements:
1. No bird is a mammal.
2. No mammal is a reptile.

#### Conclusions
I. No bird is a reptile.
II. Some reptiles are birds.

- **A**: Neither I nor II follows
- **B**: Only I follows
- **C**: Only II follows
- **D**: Either I or II follows

**Correct Answer**: **A**

#### Why
1. Both statements are universal negatives (E-propositions: No A is B, No B is C).
2. Standard categorical syllogism rule: No valid definite conclusion can be drawn from two negative premises.
3. Birds and reptiles could be completely disjoint, partially overlapping, or one could contain the other without violating the statements.
4. Hence neither conclusion is guaranteed.

- **5-Second Shortcut**: Two negative premises yield no definite conclusion.
- **Trap**: Assuming birds and reptiles must be disjoint because both are disjoint from mammals.
- **Source**: Added practice

---
### COG-053: Complementary Either-Or Pair

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Syllogisms

#### Statements
Statements:
1. Some pencils are pens.
2. No pen is an eraser.

#### Conclusions
I. Some pencils are erasers.
II. No pencil is an eraser.

- **A**: Either I or II follows
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Statement 1: Some Pencils are Pens. Statement 2: No Pen is Eraser.
2. Between Pencils and Erasers, no universal or particular positive relationship is definite.
3. Conclusion I is an I-proposition ('Some A are B').
4. Conclusion II is an E-proposition ('No A is B').
5. A particular affirmative (Some) and a universal negative (No) between the same subject and predicate form a complementary contradictory pair: exactly one of them must be true.
6. Therefore, 'Either I or II follows'.

- **5-Second Shortcut**: Complementary pair: 'Some X are Y' and 'No X is Y' with no definite link -> Either/Or.
- **Trap**: Choosing 'Neither follows' without checking for complementary contradictory pair.
- **Source**: Added practice

---
### COG-054: 'Only A are B' Conversion

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Syllogisms

#### Statements
Statements:
1. Only citizens are voters.
2. Some citizens are taxpayers.

#### Conclusions
I. All voters are citizens.
II. Some taxpayers are voters.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both I and II follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. 'Only A are B' translates to 'All B are A' (All voters are citizens). Conclusion I follows directly.
2. Statement 2 states some citizens are taxpayers, but provides no link between taxpayers and the voters subset.
3. Therefore Conclusion II does not necessarily follow.

- **5-Second Shortcut**: 'Only A are B' = 'All B are A'.
- **Trap**: Translating 'Only citizens are voters' to 'All citizens are voters'.
- **Source**: Added practice

---
### COG-055: Modus Tollens Contrapositive

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Conditional Deduction

#### Statements
Statements:
1. If a software build passes unit testing, it is deployed to staging.
2. Build Alpha was not deployed to staging.

#### Conclusions
I. Build Alpha did not pass unit testing.
II. Build Alpha had compilation errors.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Logical form: $P \implies Q$.
2. Given: $\neg Q$ (not deployed to staging).
3. By Modus Tollens: $P \implies Q$ and $\neg Q$ entails $\neg P$ (did not pass unit testing).
4. Conclusion II guesses a specific failure reason not stated in premises.
5. Only Conclusion I follows.

- **5-Second Shortcut**: If P then Q; Not Q -> therefore Not P (Modus Tollens).
- **Trap**: Speculating external reasons (compilation errors) not stated in the premises.
- **Source**: Added practice

---
### COG-056: Affirming the Consequent Fallacy

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Conditional Deduction

#### Statements
Statements:
1. If it rains heavily, the sports match is postponed.
2. The sports match is postponed.

#### Conclusions
I. It rained heavily.
II. It may or may not have rained heavily.

- **A**: Only II follows
- **B**: Only I follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Logical form: $P \implies Q$. Given: $Q$ (match is postponed).
2. Affirming the consequent ($Q$) does not guarantee the antecedent ($P$). The match could be postponed due to a waterlogged pitch, player illness, or curfew.
3. It is not guaranteed that it rained heavily, so Conclusion I is invalid.
4. Conclusion II ('may or may not have rained') accurately reflects the logical indeterminacy. Only II follows.

- **5-Second Shortcut**: If P then Q; given Q, P is possible but NOT guaranteed (fallacy of affirming the consequent).
- **Trap**: Concluding P (it rained heavily) must be true.
- **Source**: Added practice

---
### COG-057: Denying the Antecedent Fallacy

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Conditional Deduction

#### Statements
Statements:
1. If a candidate scores 90%, they get an interview.
2. Rahul did not score 90%.

#### Conclusions
I. Rahul will not get an interview.
II. Rahul may still get an interview.

- **A**: Only II follows
- **B**: Only I follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Logical form: $P \implies Q$. Given: $\neg P$ (did not score 90%).
2. Denying the antecedent does not mean $Q$ is false. Scoring 90% guarantees an interview, but candidates scoring 85% might also get an interview through another quota.
3. Therefore Conclusion I is invalid, and Conclusion II ('may still get an interview') is logically valid.

- **5-Second Shortcut**: If P then Q does NOT imply if not P then not Q.
- **Trap**: Assuming not P guarantees not Q.
- **Source**: Added practice

---
### COG-058: Linear 5-Person Seating Arrangement

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Arrangements

#### Statements
Statements:
Five colleagues A, B, C, D, and E sit in a row facing North.
1. C sits at the immediate right of A.
2. B is at an extreme end and has D as his only neighbor.
3. E is adjacent to C.

#### Conclusions
Conclusions:
I. A is at the left extreme end.
II. The order from left to right is A, C, E, D, B.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. B is at an extreme end and D is his only neighbor -> B and D must be at the right end (`_ _ _ D B`).
2. C sits immediately right of A -> `A C`.
3. E is adjacent to C -> E must sit right of C (`A C E`).
4. Fitting the 5 seats: Position 1: A, 2: C, 3: E, 4: D, 5: B.
5. A is at the left extreme end (Conclusion I follows).
6. Order is A, C, E, D, B (Conclusion II follows). Both follow.

- **5-Second Shortcut**: Lock end positions first: B at right end with D adjacent gives `_ _ _ D B`, remaining three must be `A C E`.
- **Trap**: Placing B at the left end without checking if C is right of A.
- **Source**: Added practice

---
### COG-059: Circular 6-Person Seating Arrangement

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Arrangements

#### Statements
Statements:
Six friends P, Q, R, S, T, and U sit in a circle facing the center.
1. P is opposite S.
2. R is to the immediate left of P.
3. T is opposite R.
4. Q is not adjacent to P.

#### Conclusions
Conclusions:
I. Q is adjacent to S.
II. U is adjacent to P.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Let P be at position 12 o'clock. Opposite P is S (6 o'clock).
2. R is immediate left of P (clockwise, position 2 o'clock).
3. T is opposite R -> T is at 8 o'clock.
4. Positions remaining: 10 o'clock (adjacent to P) and 4 o'clock (adjacent to S).
5. Q is not adjacent to P -> Q cannot be at 10 o'clock, so Q must be at 4 o'clock (adjacent to S).
6. The remaining friend U must occupy 10 o'clock (adjacent to P).
7. Both Conclusion I (Q adjacent to S) and Conclusion II (U adjacent to P) follow.

- **5-Second Shortcut**: Fix diametrical pairs (P-S, R-T), then place Q away from P into the remaining spot.
- **Trap**: Flipping clockwise and counter-clockwise directions when facing center.
- **Source**: Added practice

---
### COG-060: Transitive Height Ranking Deduction

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Order & Ranking

#### Statements
Statements:
1. Kamal is taller than Navin.
2. Navin is taller than Ojas.
3. Pawan is shorter than Kamal but taller than Navin.

#### Conclusions
Conclusions:
I. Kamal is the tallest among the four.
II. Ojas is the shortest among the four.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. From 1 and 3: Kamal > Pawan > Navin.
2. From 2: Navin > Ojas.
3. Complete chain: Kamal > Pawan > Navin > Ojas.
4. Kamal is strictly tallest (I follows) and Ojas is strictly shortest (II follows). Both follow.

- **5-Second Shortcut**: Combine inequalities into single chain: Kamal > Pawan > Navin > Ojas.
- **Trap**: Leaving Pawan unconnected to Navin.
- **Source**: Added practice

---
### COG-061: Blood Relations Deductive Tree

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Blood Relations

#### Statements
Statements:
1. A is the father of B.
2. B is the brother of C.
3. D is the mother of C.

#### Conclusions
Conclusions:
I. D is the wife of A.
II. A has at least two sons.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. B and C are siblings (since B is brother of C).
2. A is father of B, so A is also father of C.
3. D is mother of C. Therefore, A and D are the parents of B and C -> D is wife of A. Conclusion I follows.
4. B is male (brother), but C's gender is unspecified (could be sister). We cannot guarantee A has two sons. Only I follows.

- **5-Second Shortcut**: Parents of same children are spouses; sibling's gender is unknown unless explicitly stated.
- **Trap**: Assuming C is a brother because B is a brother.
- **Source**: Added practice

---
### COG-062: Direction Sense Deductive Route

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Direction Sense

#### Statements
Statements:
1. Rohan walks 10m North from point A to B.
2. He turns right and walks 10m to C.
3. He turns right and walks 10m to D.

#### Conclusions
Conclusions:
I. Point D is 10m East of Point A.
II. Rohan is currently facing South.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. A to B: 10m North (coordinates (0, 10)).
2. Turn right (East) 10m to C: coordinates (10, 10).
3. Turn right (South) 10m to D: coordinates (10, 0).
4. Point D is at (10, 0), exactly 10m East of start A (0, 0). (Conclusion I follows).
5. His last heading was South. (Conclusion II follows). Both follow.

- **5-Second Shortcut**: Plot on Cartesian grid: (0,0) -> (0,10) -> (10,10) -> (10,0).
- **Trap**: Adding distances rather than finding displacement.
- **Source**: Added practice

---
### COG-063: Syllogism: 'All' with 'No' Disjoint Chain

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Syllogisms

#### Statements
Statements:
1. All cars are vehicles.
2. No vehicle is an airplane.

#### Conclusions
I. No car is an airplane.
II. Some vehicles are cars.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Cars are completely contained inside Vehicles.
2. Vehicles and Airplanes are completely disjoint.
3. Therefore, Cars can have no overlap with Airplanes (Conclusion I follows).
4. Since All cars are vehicles, some vehicles must be cars (conversion of A-proposition). Both follow.

- **5-Second Shortcut**: Subset of a set disjoint from X is also disjoint from X.
- **Trap**: Missing the valid conversion of 'All A are B' to 'Some B are A'.
- **Source**: Added practice

---
### COG-064: Possibility vs Definite Conclusion

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Syllogisms

#### Statements
Statements:
1. Some lakes are rivers.
2. Some rivers are oceans.

#### Conclusions
I. Some lakes are oceans.
II. All lakes being oceans is a possibility.

- **A**: Only II follows
- **B**: Only I follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Between Lakes and Oceans there is no direct link; 'Some lakes are oceans' is not definitely true. Conclusion I fails.
2. In Venn diagrams, an arrangement where all lakes are oceans does not violate either premise (it is a valid possibility). Conclusion II follows.

- **5-Second Shortcut**: Two 'Some' statements yield no definite conclusion, but allow possibilities that do not contradict premises.
- **Trap**: Treating possibility as a definite conclusion.
- **Source**: Added practice

---
### COG-065: Four-Floor Building Arrangement

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Floor Puzzles

#### Statements
Statements:
Four people W, X, Y, Z live on floors 1, 2, 3, 4 (1 is ground, 4 is top).
1. Y lives on an odd-numbered floor.
2. Exactly two people live between Y and Z.
3. X lives on a floor immediately above W.

#### Conclusions
Conclusions:
I. Z lives on floor 4.
II. Y lives on floor 1.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Y lives on odd floor: either floor 1 or floor 3.
2. Exactly two floors between Y and Z:
   - If Y is on floor 3, two floors between would require Z on floor 0 or 6 (impossible).
   - Thus, Y must be on floor 1, and Z must be on floor 4 (floors 2 and 3 are between them).
3. X is immediately above W: X is on floor 3, W is on floor 2.
4. Therefore, Y is on floor 1 and Z is on floor 4. Both conclusions follow.

- **5-Second Shortcut**: Two people between on a 4-floor building MUST occupy floors 1 and 4.
- **Trap**: Trying Y on floor 3.
- **Source**: Added practice

---
### COG-066: Conditional Double Negation

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Conditional Deduction

#### Statements
Statements:
1. Unless the server is patched, the system is vulnerable.
2. The system is not vulnerable.

#### Conclusions
I. The server is patched.
II. The server is offline.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. 'Unless P, Q' translates logically to: 'If not P, then Q'.
2. Given premise: 'System is not vulnerable' ($\neg Q$).
3. By Modus Tollens: If $\neg P \implies Q$ and $\neg Q$, then $\neg(\neg P)$ which is $P$ (Server is patched).
4. Conclusion I follows directly. Conclusion II is unsupported.

- **5-Second Shortcut**: Unless P, Q = If not P then Q. Negating Q proves P.
- **Trap**: Assuming 'not vulnerable' implies offline.
- **Source**: Added practice

---
### COG-067: Syllogism: 'A few' and 'At least some'

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Syllogisms

#### Statements
Statements:
1. A few books are novels.
2. At least some novels are bestsellers.

#### Conclusions
I. Some books are novels.
II. Some novels are bestsellers.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. In formal logic, 'a few', 'at least some', 'many', and 'most' are all treated as the standard existential quantifier 'Some' (I-proposition).
2. Therefore Statement 1 asserts 'Some books are novels' (I follows).
3. Statement 2 asserts 'Some novels are bestsellers' (II follows). Both follow.

- **5-Second Shortcut**: 'A few' and 'at least some' are identical to 'Some'.
- **Trap**: Treating 'a few' as a percentage cutoff.
- **Source**: Added practice

---
### COG-068: Comparison of Weights Inequality Chain

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Ranking

#### Statements
Statements:
1. Bag P is heavier than Bag Q.
2. Bag R is lighter than Bag S.
3. Bag Q is heavier than Bag S.

#### Conclusions
Conclusions:
I. Bag P is the heaviest.
II. Bag R is the lightest.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. From 1: P > Q.
2. From 3: Q > S.
3. From 2: S > R.
4. Chain: P > Q > S > R.
5. P is strictly heaviest (I follows), R is strictly lightest (II follows). Both follow.

- **5-Second Shortcut**: Align inequality arrows: P > Q > S > R.
- **Trap**: Reversing lighter/heavier comparisons.
- **Source**: Added practice

---
### COG-069: Transitive Syllogism with Specific Instance

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Categorical Logic

#### Statements
Statements:
1. All prime numbers greater than 2 are odd.
2. 17 is a prime number greater than 2.

#### Conclusions
I. 17 is an odd number.
II. All odd numbers are prime.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Syllogistic instantiation: 17 satisfies the condition of premise 1, so 17 is odd. Conclusion I follows.
2. Conclusion II is an invalid converse ('All A are B' does not mean 'All B are A', e.g. 9 is odd but not prime). Only I follows.

- **5-Second Shortcut**: Universal affirmative applies to all members; converse is invalid.
- **Trap**: Accepting invalid converse statement.
- **Source**: Added practice

---
### COG-070: Exclusive OR vs Inclusive OR in Decisions

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Conditional Logic

#### Statements
Statements:
1. A team member must know either Python or Java to join the project (at least one).
2. Priya knows both Python and Java.

#### Conclusions
I. Priya is eligible to join the project.
II. Priya is disqualified because she knows both.

- **A**: Only I follows
- **B**: Only II follows
- **C**: Both follow
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. In logical specifications, 'either A or B' without the qualifier 'but not both' represents inclusive OR ($A \lor B$).
2. Priya knowing both satisfies $A \lor B$ as true.
3. Therefore Priya is eligible (Conclusion I follows) and not disqualified. Only I follows.

- **5-Second Shortcut**: Logical 'either/or' is inclusive by default unless 'not both' is explicitly stated.
- **Trap**: Interpreting standard either/or as mutually exclusive.
- **Source**: Added practice

---
### COG-071: Complex 3-Statement Syllogism with Disjoint Endpoints

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Syllogisms

#### Statements
Statements:
1. All apples are fruits.
2. No fruit is a vegetable.
3. All potatoes are vegetables.

#### Conclusions
Conclusions:
I. No apple is a vegetable.
II. No apple is a potato.

- **A**: Both I and II follow
- **B**: Only I follows
- **C**: Only II follows
- **D**: Neither follows

**Correct Answer**: **A**

#### Why
1. Apples are inside Fruits. Fruits and Vegetables are disjoint -> No apple is a vegetable (Conclusion I follows).
2. Potatoes are inside Vegetables. Since Apples and Vegetables are disjoint, Apples and Potatoes are completely disjoint -> No apple is a potato (Conclusion II follows).
3. Both conclusions follow.

- **5-Second Shortcut**: Fruits disjoint from Vegetables; Apples in Fruits and Potatoes in Vegetables means Apples and Potatoes are disjoint.
- **Trap**: Missing the double inclusion into disjoint sets.
- **Source**: Added practice

---


---

Previous: [02_digit_and_grid.md](02_digit_and_grid.md) | Next: [04_behavioral.md](04_behavioral.md)
