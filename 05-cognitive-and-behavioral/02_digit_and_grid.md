# Cognitive Games: Digit Challenge & Grid Inductive Reasoning

---

## 1. Digit Challenge (Speed Arithmetic Puzzle)

> **Note**: Practice example (not from video)

### Game UI & Rules
- **Interface**: An incomplete mathematical equation with blank number slots and operator boxes, equating to a specified **Target Value**.
- **Rule**: Each given digit can be used at most once.
- **BODMAS Precedence**: The game engine strictly follows standard algebraic order of operations:
  $$\text{Multiplication } (*) \text{ and Division } (/) \implies \text{Addition } (+) \text{ and Subtraction } (-)$$
  Calculations are **not** evaluated left-to-right blindly.

### Pro-Tricks for Fast Solving
1. **Anchor the Multiplier First**: If the target value is large (e.g., 42), identify two usable digits whose product lands closest to the target:
   - $7 \times 5 = 35$ or $8 \times 5 = 40$.
2. **Remainder Matching**: Calculate the remaining difference:
   - Target $42 - 40 = 2$.
   - Check if $2$ is in the available digit pool. If yes, add it directly: $(8 \times 5) + 2 = 42$.
3. **Division as a Fractional Reducer**: Unless the target includes decimals, only place division operators between pairs of numbers that divide cleanly with zero remainder (e.g., $8 / 2 = 4$ or $6 / 3 = 2$).

### Worked Example from Chat
- **Equation**: `[ ? ] [ op ] [ ? ] [ op ] [ ? ] = 42`
- **Available Digits**: `[3, 5, 8, 2, 7]`
- **Step-by-Step Solution**:
  1. Target is 42. Using multiplication, $8 \times 5 = 40$.
  2. Remainder needed is $42 - 40 = 2$.
  3. Form equation: `8 * 5 + 2`.
  4. Hand check: $8 \times 5 = 40$, then $40 + 2 = 42$. (BODMAS valid).
- **Final Answer**: `8 * 5 + 2 = 42`

---

## 2. Grid / Missing Symbol Challenge (Mini-Sudoku Constraint Satisfaction)

### Core Rules
- **Grid Dimensions**: Typically played on a $4 \times 4$ or $5 \times 5$ grid using a fixed alphabet of symbols (e.g., $\{\boldsymbol{+}, \bigcirc, \blacktriangle, \blacksquare\}$).
- **Exact Constraint (Latin Square Property)**: Every row and every column must contain each symbol **exactly once**. No symbol may repeat within any row or column.
- **Target Cell**: One or more cells are masked with a question mark `?`.

### Solving Strategy (Elimination Order)
1. **Find Maximum Density**: Identify the row or column containing $N - 1$ out of $N$ symbols first (e.g., 3 out of 4 symbols filled); the remaining cell is immediately deterministic.
2. **Intersection Cross-Check**: For the target cell marked `?`, eliminate all symbols present across its entire row **AND** its entire column.
3. **Hypothesis / Backtracking**: If two candidates remain, pick one candidate tentatively and verify whether it forces an immediate row/column clash in an adjacent intersected cell.

### 3 Core Deductive Rules
1. **Row & Column Exclusivity (Shapes)**: Each distinct primary shape appears exactly once in each row and column (Sudoku Latin-square principle).
2. **Horizontal Consistency (Inner Markers)**: Sub-features (dots, crosses, stars) often remain uniform across an entire row while shapes permute.
3. **Rotational Progression**: Lines, arrows, or shaded sectors rotate by fixed angular increments ($+45^\circ$, $+90^\circ$, or $-90^\circ$) across each step.

### Worked Example: Latin Square Elimination
```text
            Column 1       Column 2       Column 3
Row 1:    [Circle, •]    [Square, •]    [Triangle, •]   <-- Inner marker '•' uniform across row
Row 2:    [Triangle, +]  [Circle, +]    [Square, +]     <-- Inner marker '+' uniform across row
Row 3:    [Square, *]    [Triangle, *]  [    ?    ]     <-- Target Cell
```

**Step-by-Step Deduction**:
1. **Determine Shape**:
   - Row 3 already contains **Square** and **Triangle**.
   - By Row Exclusivity, the missing shape must strictly be a **Circle**.
2. **Determine Inner Marker**:
   - In Row 3, Column 1 has inner marker `*` and Column 2 has inner marker `*`.
   - By Horizontal Consistency, the inner marker for Column 3 must strictly be `*`.
3. **Final Verified Symbol**: **Circle containing an asterisk (*)**.

---

## 3. Pattern Recognition / Match Challenge (Spatial & Structural Invariance)

### Core Concept
In the Pattern Recognition (Match Challenge), candidates are shown reference patterns or small grid tiles and must locate identical, transformed, or invariant sub-patterns within a larger selection matrix.

### Invariance Checks & Elimination Filters
1. **Row / Column Symmetry**:
   - Check if symmetric pairs exist (e.g., Row 1 and Row 4 are identical; Column 1 is a mirror of Column 4).
2. **Fixed Symbol / Shading Counts**:
   - Count the total number of filled vs. unfilled cells (e.g., exactly 6 filled cells and 3 unfilled cells). Eliminate options with mismatched densities.
3. **Rotational Symmetry**:
   - Verify whether the target pattern is simply the reference rotated by $90^\circ, 180^\circ,$ or $270^\circ$ clockwise.
4. **Structural Alignment**:
   - Inspect boundary edges: do corner cells have diagonal connections, L-shapes, or isolated single dots?

---

## 4. Extra Practice Puzzles

### Practice Puzzle 1: Digit Challenge
**Tag**: [ADDED]  
- **Target**: `53`
- **Available Digits**: `[4, 6, 9, 5, 1]`
- **Solution**: Anchor multiplication near 53: $9 \times 6 = 54$. Remainder adjustment: $54 - 1 = 53$.
- **Equation**: `(9 * 6) - 1 = 53` (Verified: $54 - 1 = 53$).

### Practice Puzzle 2: Digit Challenge with Division
**Tag**: [ADDED]  
- **Target**: `24`
- **Available Digits**: `[2, 3, 4, 8, 5]`
- **Solution**: Direct multiplication $8 \times 3 = 24$, or using parentheses with remainder $(5 - 2) \times 8 = 3 \times 8 = 24$.
- **Equation**: `8 * 3 = 24` or `(5 - 2) * 8 = 24`.

### Practice Puzzle 3: Grid Rotational Step
**Tag**: [ADDED]  
- **Pattern**: A single clock hand pointer rotates within a square:
  - Row 1: Pointing North ($0^\circ$) $\to$ Pointing North-East ($45^\circ$) $\to$ Pointing East ($90^\circ$).
  - Row 2: Pointing East ($90^\circ$) $\to$ Pointing South-East ($135^\circ$) $\to$ Pointing South ($180^\circ$).
  - Row 3: Pointing South ($180^\circ$) $\to$ Pointing South-West ($225^\circ$) $\to$ `[ ? ]`.
- **Deduction**: The pointer advances $+45^\circ$ clockwise at each column. $225^\circ + 45^\circ = 270^\circ$ (Pointing West).
- **Target Symbol**: Pointer pointing directly **West** ($270^\circ$).
