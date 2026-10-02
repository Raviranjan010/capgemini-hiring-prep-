# Cognitive & Behavioral Assessment

This module covers the 4 gamified and psychometric assessment areas tested in Capgemini's cognitive evaluation stage.

## Updated Cognitive Assessment Architecture & Section Timers

The updated Capgemini Cognitive Assessment evaluates spatial planning, short-term visual working memory, formal deductive logic, and workplace behavioral alignment.

```text
Capgemini Cognitive Assessment Pipeline
├── 1. Motion Challenge (Interactive Spatial Puzzle) ──────► 6 Minutes
├── 2. Grid / Bubble Memory Challenge (Working Memory) ────► 12 Minutes
├── 3. Deductive Logical Reasoning (Critical Syllogisms) ──► 25 Minutes
└── 4. Behavioral Profile Assessment (Adaptive Forced-Choice) ► 25 Minutes
```

| Section | Duration | Format & Mechanics | Primary Competency Tested | Elimination Stage? |
| :--- | :---: | :--- | :--- | :---: |
| **Motion Challenge** | **6 Minutes** | Sliding block puzzle; clear obstacles to move a ball to an exit hole. | Spatial trajectory planning, shortest-path calculation under constraints. | **Yes** (Scores contribute to round cutoffs) |
| **Bubble / Grid Memory** | **12 Minutes** | Observe flashing sequential bubbles/grid coordinates and reproduce them in exact order. | Visual-spatial working memory capacity and sequential recall. | **Yes** |
| **Deductive Reasoning** | **25 Minutes** | Statement evaluation, categorical syllogisms, contrapositive logic. | Deductive logic, premise validity, conditional statement analysis. | **Yes** |
| **Behavioral Module** | **25 Minutes** | Forced-choice personality pairs (select which statement best describes you and rate agreement). | Cultural alignment, consistency, leadership traits, collaboration. | **No** (Profiling & consistency validation) |

---

## Cognitive Game Suite Overview
```text
Cognitive Assessment Suite (All Interactive Games & Modules)
├── 1. Deductive Logic / Grid Challenge (4x4 or 5x5 Mini-Sudoku | ~6 Mins)
├── 2. Switch Challenge / Permutations (Deductive Symbol Shifts)
├── 3. Visual Reasoning / Odd-One-Out (Rotational & Spatial Trajectories)
├── 4. Numerical Operations / Digit Equations (Mental Math & BODMAS)
└── 5. Motion Challenge (Sliding Obstacle Shortest Path Planning)
```

### Cognitive Round Strategy Matrix
```text
Cognitive Round Strategy Matrix
├── Motion Challenge:
│   ├── Look at the exit hole first: Find which block directly seals the target.
│   └── Count moves: Moving the ball through a detour is often cheaper than clearing two blocks.
├── Bubble Memory:
│   ├── Chunk patterns: Group nodes into triangles, lines, or quadrants (Cowan's 4±1 model).
│   └── Anchor start & end: Misclicking the first node invalidates the entire trial immediately.
├── Deductive Reasoning:
│   ├── Convert sentences to formulas: Translate "All X are Y" into X ⊆ Y.
│   ├── Use the contrapositive: (P → Q) ≡ (¬Q → ¬P).
│   └── Watch conversion traps: "Some A are B" does NOT guarantee that "Some A are not B".
└── Behavioral Module:
    ├── Maintain consistency: Repeated questions check for contradictory answers.
    └── Focus on core workplace traits: Prioritize collaboration, accountability, and delivery.
```

---

## Module Files
1. [01_motion_and_bubble.md](01_motion_and_bubble.md) - Motion Challenge (ball & obstacle ice-sliding, 2-step clearing, shortest path planning) and Bubble Memory Challenge (sequential recall).
2. [02_digit_and_grid.md](02_digit_and_grid.md) - Digit Challenge (speed arithmetic with BODMAS), Grid / Mini-Sudoku Challenge (Latin Square constraint satisfaction, 5x5 elimination), and Pattern Recognition (Match Challenge).
3. [03_deductive_reasoning.md](03_deductive_reasoning.md) - Formal syllogisms, conversion logic, and contrapositive conditional reasoning.
4. [04_behavioral.md](04_behavioral.md) - Adaptive behavioral profiling, consistency engine rules, enterprise priority matrix, and dilemma resolution.
5. [05_switch_challenge.md](05_switch_challenge.md) - Switch Challenge (symbol operator deduction, pull-based permutation mechanics, anchor element strategy, video walkthroughs, and 12 fully-worked practice puzzles).
6. [06_visual_reasoning_and_odd_one_out.md](06_visual_reasoning_and_odd_one_out.md) - Visual Reasoning / Odd-One-Out (perimeter loops, clock hand rotations, dihedral reflection vs point symmetry, and 10 exam-level practice questions).


