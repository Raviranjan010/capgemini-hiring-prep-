[Home](../README.md) > [06-cognitive-and-behavioral](README.md) > 06_visual_reasoning_and_odd_one_out.md

# Cognitive Assessment: Visual Reasoning & Odd-One-Out

**Tag**: [COGNITIVE-GAME]  
**Video Reference**: [Capgemini Game-Based Aptitude Walkthrough](https://www.youtube.com/watch?v=wRwQYaxPEjE)

---

## 1. Overview & Mechanics

The **Visual Reasoning / Odd-One-Out** challenge tests perceptual speed, rotational invariance, spatial trajectories, and geometric symmetry under strict per-question countdowns (~15–20 seconds per question).

Candidates are presented with sequential frames ($A, B, C, \dots$) or a set of candidate figures and must identify:
1. **The Odd-One-Out**: The single frame that violates an angular progression, spatial coordinate path, reflection symmetry, or element count invariant.
2. **Next in Sequence**: The exact orientation, coordinate position, and state (shading/color) of the succeeding frame.

```text
Visual Reasoning Invariance Pipeline
├── 1. Spatial Trajectory (Perimeter loops, diamond paths, vertex shifts)
├── 2. Angular Velocity (Constant degrees: +45°, +90°, +180° or alternating steps)
├── 3. Internal Element Invariance (Line count == polygon sides, inner vs outer relative offset)
└── 4. Symmetry Classification (Bilateral reflection D1 vs Point rotational C2)
```

---

## 2. Core Video Problem Walkthroughs

### Video Problem 1 (COG-039): Odd-One-Out (Spatial Trajectory Violation)
**Tag**: [VIDEO]

**Setup**:  
A square element rotates along a designated cyclical path across successive frames: $A, B, C, D, E, F, G$. You must identify the frame that violates the progression.

**Pattern Tracing**:
- **Frame $A$**: Square is at the top edge.
- **Frame $B$**: Shifts clockwise to the top-right / right side.
- **Frame $C$**: Rotates further clockwise down the right edge.
- **Frame $D$**: Moves continuously along the clockwise perimeter toward the bottom.
- **Frame $E$**: The square moves counter-clockwise down to the bottom rather than returning up toward the top, breaking the clockwise perimeter flow.
- **Frames $F$ & $G$**: Resume the standard clockwise perimeter pattern.

**Answer**: **Frame E**

---

### Video Problem 2 (COG-040): Rotational Symmetry Violation (Clock Faces)
**Tag**: [VIDEO]

**Setup**:  
Five clock faces show minute and hour hand positions:
- **A**: Hour at 12, Minute at 3 ($90^\circ$ clockwise angle)
- **B**: Hour at 3, Minute at 6 ($90^\circ$ clockwise angle)
- **C**: Hour at 6, Minute at 9 ($90^\circ$ clockwise angle)
- **D**: Hour at 9, Minute at 11 ($60^\circ$ clockwise angle)
- **E**: Hour at 9, Minute at 12 ($90^\circ$ clockwise angle)

Which frame is the odd one out?
- **A)** Frame A
- **B)** Frame B
- **C)** Frame C
- **D)** Frame D

**Correct Answer**: **Option D (Frame D)**

**Explanation**:  
Frames A, B, C, and E maintain a constant orthogonal $90^\circ$ angle between hands, while Frame D forms a $60^\circ$ angle (2 clock hours $\times 30^\circ/\text{hour} = 60^\circ$), violating angular invariance.

---

## 3. High-Yield Practice Question Bank (10 Exam Puzzles)

### Question 1 (COG-041): Perimeter Track Progression (Odd-One-Out)
A reference square grid contains a black dot traveling along the outer border cells across five sequential stages ($A, B, C, D, E$):
- **A**: Dot is at Top-Left $(0,0)$.
- **B**: Dot is at Top-Right $(0,2)$.
- **C**: Dot is at Bottom-Right $(2,2)$.
- **D**: Dot is at Bottom-Center $(2,1)$.
- **E**: Dot is at Bottom-Left $(2,0)$.

Which stage violates the spatial sequence?
- **A)** Stage B
- **B)** Stage C
- **C)** Stage D
- **D)** Stage E

**Correct Answer**: **C (Stage D)**

**Explanation**:  
Tracing the step distance along the perimeter:
- $A \to B$: Moves 2 steps clockwise (Top-Left $\to$ Top-Right).
- $B \to C$: Moves 2 steps clockwise (Top-Right $\to$ Bottom-Right).
- Continuing the $+2$ step clockwise rule from Bottom-Right $(2,2)$ should place the dot at Bottom-Left $(2,0)$ directly.  
Stage D only advances 1 step to Bottom-Center $(2,1)$, breaking the uniform $+2$ step invariant.

---

### Question 2 (COG-042): Dual Element Opposite Rotations (Next in Sequence)
A circular dial contains two indicator hands (an Arrow and a Dot):
- **Frame 1**: Arrow points at 12 o'clock; Dot is at 6 o'clock.
- **Frame 2**: Arrow points at 2 o'clock; Dot is at 5 o'clock.
- **Frame 3**: Arrow points at 4 o'clock; Dot is at 4 o'clock (overlapping).
- **Frame 4**: Arrow points at 6 o'clock; Dot is at 3 o'clock.

Where will the Arrow and Dot point in Frame 5?
- **A)** Arrow at 8 o'clock; Dot at 2 o'clock
- **B)** Arrow at 7 o'clock; Dot at 2 o'clock
- **C)** Arrow at 8 o'clock; Dot at 1 o'clock
- **D)** Arrow at 9 o'clock; Dot at 2 o'clock

**Correct Answer**: **A (Arrow at 8 o'clock; Dot at 2 o'clock)**

**Explanation**:  
Analyze each element's angular velocity independently:
- **Arrow**: Advances $+2$ clock hours clockwise per frame ($12 \to 2 \to 4 \to 6 \to \mathbf{8}$).
- **Dot**: Retracts $-1$ clock hour counter-clockwise per frame ($6 \to 5 \to 4 \to 3 \to \mathbf{2}$).  
Frame 5 strictly yields Arrow at 8 o'clock and Dot at 2 o'clock.

---

### Question 3 (COG-043): Alternating Angle Rotation with Shading (Odd-One-Out)
An asymmetrical arrow rotates about its center across five consecutive frames:
- **Frame 1**: Points North ($0^\circ$), head is shaded black.
- **Frame 2**: Points North-East ($45^\circ$), head is white.
- **Frame 3**: Points South-East ($135^\circ$), head is shaded black.
- **Frame 4**: Points South ($180^\circ$), head is white.
- **Frame 5**: Points North-West ($315^\circ$), head is shaded black.

Which frame contains an orientation or state error?
- **A)** Frame 2
- **B)** Frame 3
- **C)** Frame 4
- **D)** Frame 5

**Correct Answer**: **D (Frame 5)**

**Explanation**:  
Two alternating patterns govern the sequence:
1. **Angular rotation step**: Alternates between $+45^\circ$ and $+90^\circ$ clockwise:
   - $0^\circ + 45^\circ = 45^\circ$ (NE)
   - $45^\circ + 90^\circ = 135^\circ$ (SE)
   - $135^\circ + 45^\circ = 180^\circ$ (S)
   - Next rotation must be $+90^\circ \implies 180^\circ + 90^\circ = \mathbf{270^\circ}$ (West, not $315^\circ$ NW).
2. **Shading**: Strictly alternates Black $\to$ White $\to$ Black $\to$ White $\to$ Black.  
Frame 5 has the wrong angular orientation.

---

### Question 4 (COG-044): Polygon Vertex Migration (Next in Sequence)
A regular hexagon has vertices labeled 1 to 6 clockwise. An inner symbol shifts vertices according to an arithmetic sequence:
- **Step 1**: Vertex 1
- **Step 2**: Vertex 2 ($+1$ step clockwise)
- **Step 3**: Vertex 4 ($+2$ steps clockwise)
- **Step 4**: Vertex 1 ($+3$ steps clockwise: $4 \to 5 \to 6 \to 1$)

Which vertex will the symbol occupy in Step 5?
- **A)** Vertex 3
- **B)** Vertex 4
- **C)** Vertex 5
- **D)** Vertex 6

**Correct Answer**: **C (Vertex 5)**

**Explanation**:  
The step increment increases by $+1$ at each transition:
- Transition 1: $+1$ step
- Transition 2: $+2$ steps
- Transition 3: $+3$ steps
- Transition 4: $+4$ steps clockwise from Vertex 1 $\implies (1 + 4) \pmod 6 = 5$.  
The symbol lands on **Vertex 5**.

---

### Question 5 (COG-045): Inner vs. Outer Compound Rotation (Odd-One-Out)
A compound figure consists of an outer equilateral triangle and an inner inscribed arrow:
- **Figure A**: Triangle apex points Up; Arrow points Right ($90^\circ$ relative angle).
- **Figure B**: Triangle apex points Right; Arrow points Down ($90^\circ$ relative angle).
- **Figure C**: Triangle apex points Down; Arrow points Left ($90^\circ$ relative angle).
- **Figure D**: Triangle apex points Left; Arrow points Up ($90^\circ$ relative angle).
- **Figure E**: Triangle apex points Up; Arrow points Left ($270^\circ$ relative angle).

Which figure does not belong to the set?
- **A)** Figure B
- **B)** Figure C
- **C)** Figure D
- **D)** Figure E

**Correct Answer**: **D (Figure E)**

**Explanation**:  
In Figures A through D, the inner arrow is oriented at an exact $+90^\circ$ clockwise offset relative to the triangle's apex orientation. In Figure E, the arrow points Left instead of Right (a $270^\circ$ offset / $-90^\circ$), violating internal geometric invariance.

---

### Question 6 (COG-046): 3x3 Grid Cross Pattern Trajectory (Next in Sequence)
In a $3 \times 3$ matrix, a shaded cross symbol moves according to a fixed coordinate vector:
- **Frame 1**: Located at $(0, 1)$ (Top-Center).
- **Frame 2**: Located at $(1, 2)$ (Middle-Right).
- **Frame 3**: Located at $(2, 1)$ (Bottom-Center).
- **Frame 4**: Located at $(1, 0)$ (Middle-Left).

Where does the cross appear in Frame 5?
- **A)** Center $(1, 1)$
- **B)** Top-Center $(0, 1)$
- **C)** Top-Right $(0, 2)$
- **D)** Bottom-Left $(2, 0)$

**Correct Answer**: **B (Top-Center $(0, 1)$)**

**Explanation**:  
The cross moves through the 4 non-corner edge cells in a continuous clockwise diamond cycle:
$$\text{Top-Center } (0,1) \to \text{Right-Center } (1,2) \to \text{Bottom-Center } (2,1) \to \text{Left-Center } (1,0) \to \mathbf{\text{Top-Center } (0,1)}$$

---

### Question 7 (COG-047): Geometric Morphing with Rotation (Odd-One-Out)
Each item displays a geometric polygon with internal radial segment lines:
- **A**: Equilateral triangle with 3 internal segments rotated by $0^\circ$.
- **B**: Square with 4 internal segments rotated by $45^\circ$.
- **C**: Regular pentagon with 5 internal segments rotated by $90^\circ$.
- **D**: Regular hexagon with 5 internal segments rotated by $135^\circ$.
- **E**: Regular heptagon with 7 internal segments rotated by $180^\circ$.

Which option is the odd one out?
- **A)** Option B
- **B)** Option C
- **C)** Option D
- **D)** Option E

**Correct Answer**: **C (Option D)**

**Explanation**:  
Two rules couple the elements:
1. Overall figure rotation advances by $+45^\circ$ per step ($0^\circ \to 45^\circ \to 90^\circ \to 135^\circ \to 180^\circ$).
2. The number of internal segments must equal the polygon's total number of sides ($n$-gon has $n$ lines).
   - Triangle: 3 sides, 3 segments.
   - Square: 4 sides, 4 segments.
   - Pentagon: 5 sides, 5 segments.
   - Hexagon (Option D): 6 sides, but contains only 5 segments, violating the side-to-line equality rule.

---

### Question 8 (COG-048): Multi-Layer Concentric Ring Spin (Next in Sequence)
A visual puzzle has three concentric rings (Outer, Middle, Inner), each marked with a single notch:
- **Initial State (Frame 1)**: All three notches align at 12 o'clock ($0^\circ$).
- **Frame 2**: Outer notch at $90^\circ$; Middle notch at $180^\circ$; Inner notch at $270^\circ$.
- **Frame 3**: Outer notch at $180^\circ$; Middle notch at $0^\circ$ ($360^\circ$); Inner notch at $180^\circ$.
- **Frame 4**: Outer notch at $270^\circ$; Middle notch at $180^\circ$; Inner notch at $90^\circ$.

What are the positions of the three notches in Frame 5?
- **A)** Outer: $0^\circ$, Middle: $0^\circ$, Inner: $0^\circ$
- **B)** Outer: $0^\circ$, Middle: $90^\circ$, Inner: $180^\circ$
- **C)** Outer: $90^\circ$, Middle: $0^\circ$, Inner: $270^\circ$
- **D)** Outer: $0^\circ$, Middle: $180^\circ$, Inner: $0^\circ$

**Correct Answer**: **A (Outer: $0^\circ$, Middle: $0^\circ$, Inner: $0^\circ$)**

**Explanation**:  
Calculate independent rotational velocities:
- **Outer Ring**: Rotates $+90^\circ$ clockwise each frame: $270^\circ + 90^\circ = 360^\circ \equiv \mathbf{0^\circ}$.
- **Middle Ring**: Toggles by $+180^\circ$ each frame: $180^\circ + 180^\circ = 360^\circ \equiv \mathbf{0^\circ}$.
- **Inner Ring**: Rotates $-90^\circ$ counter-clockwise each frame: $90^\circ - 90^\circ = \mathbf{0^\circ}$.  
In Frame 5, all three notches realign at the top ($0^\circ / 12 \text{ o'clock}$).

---

### Question 9 (COG-049): Reflectional vs. Rotational Invariance (Odd-One-Out)
Examine the following capital letter glyphs under planar 2D transformations:
- **A**: Letter **H**
- **B**: Letter **I**
- **C**: Letter **N**
- **D**: Letter **X**
- **E**: Letter **O**

Which letter is the odd one out regarding bilateral reflection symmetry?
- **A)** Letter H
- **B)** Letter I
- **C)** Letter N
- **D)** Letter X

**Correct Answer**: **C (Letter N)**

**Explanation**:  
- **H, I, X, and O** possess both horizontal and vertical lines of reflective symmetry ($D_2$ dihedral symmetry).
- **N** possesses $180^\circ$ point-rotational symmetry ($C_2$), but has **neither horizontal nor vertical line reflection symmetry**.

---

### Question 10 (COG-050): Matrix Corner Traversal with Color Flip (Next in Sequence)
A square block travels across the four corners of a $4 \times 4$ board:
- **Step 1**: Top-Left corner $(0,0)$, colored White.
- **Step 2**: Top-Right corner $(0,3)$, colored Black.
- **Step 3**: Bottom-Right corner $(3,3)$, colored White.
- **Step 4**: Bottom-Left corner $(3,0)$, colored Black.

What are the position and fill color of the block in Step 5?
- **A)** Bottom-Left $(3,0)$, White
- **B)** Top-Left $(0,0)$, White
- **C)** Top-Left $(0,0)$, Black
- **D)** Center $(1,1)$, White

**Correct Answer**: **B (Top-Left $(0,0)$, White)**

**Explanation**:  
1. **Corner Path**: The block cycles clockwise through the four corners:
   $$\text{Top-Left } \to \text{Top-Right } \to \text{Bottom-Right } \to \text{Bottom-Left } \to \mathbf{\text{Top-Left }(0,0)}$$
2. **Color Inversion**: Color strictly alternates: $\text{White} \to \text{Black} \to \text{White} \to \text{Black} \to \mathbf{\text{White}}$.

---

## 4. Assessment Strategy & Speed Optimization

| Challenge Type | Key Pitfall | Recommended Speed Technique | Target Time |
| :--- | :--- | :--- | :---: |
| **Switch Challenge** | Mapping all 4 elements simultaneously | **Anchor Tracking**: Track only the symbol moving into Position 1 or 4 to eliminate 2 of 3 choices immediately. | **10–15 sec** |
| **Grid Challenge (Sudoku)** | Guessing symbols before checking columns | **Maximum Density First**: Start with rows or columns containing $N-1$ filled cells. | **20–30 sec** |
| **Visual Reasoning** | Overlooking movement direction reversals | **Delta Calculation**: Trace step size between consecutive frames ($+45^\circ$, $+90^\circ$, or $+1$ slot) to catch directional flips. | **15–20 sec** |
| **Digit Equation Balancing** | Violating operator precedence | **Bound the Multiplier**: In $A \times B \pm C = T$, approximate $A \times B$ close to $T$ first, then resolve $C$. | **15 sec** |

### Key Visual Reasoning Heuristics Table

| Test Pattern | Verification Technique | Fast Identification Rule |
| :--- | :--- | :--- |
| **Track Movements** | Perimeter step counting | Convert coordinates to 1D index loop: $0 \to 1 \to 2 \dots \to N-1 \to 0$. |
| **Rotational Hands** | Angular delta calculation | Track degree changes per jump ($+45^\circ, +90^\circ, +135^\circ$). |
| **Odd-One-Out** | Count invariance | Count internal lines, dots, or vertex intersections before checking rotation. |
| **Symmetry Checks** | Fold line test | Distinguish between a $180^\circ$ spin ($C_2$) and a mirrored reflection ($D_1$). |

---

Previous: [05_switch_challenge.md](05_switch_challenge.md) | Next: [07-communication/README.md](../07-communication/README.md)
