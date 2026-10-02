# Gamified Cognitive Challenges: Motion & Bubble Memory

**Tag**: [VIDEO]  
**Video Reference**: [Capgemini Cognitive Assessment Live Gameplay & Walkthrough](https://youtu.be/o5TbT3kzEnA)

---

## 1. Motion Challenge (Spatial Planning & Obstacle Clearing)

### Game Overview & Mechanics
- **Time Allotted**: 6 Minutes (Countdown timer).
- **Target Score**: Clear 8 to 10+ levels within the time limit.
- **Components**:
  - **Red Ball**: The object that must reach the goal.
  - **Black Hole**: The destination goal.
  - **Sliding Blocks**: Movable obstacles that slide along horizontal or vertical tracks.
  - **Static Barriers**: Immovable walls and boundaries.
- **Move Counter**: Sliding any movable block by 1 step or launching the ball counts as **1 Move**.
- **Ice-Sliding Mechanic**: When pushed, the red ball slides continuously until it collides with an obstacle, block, or boundary wall. It does not stop on empty squares.

### 3 Solved Levels from the Video Walkthrough

#### Level 1: Direct Ice Path (4 Moves)
- **Grid Layout**: Open corridor with three turning walls.
- **Step-by-Step Moves**:
  1. Slide ball **Left** into the far wall (Move 1).
  2. Slide ball **Up** into the ceiling boundary (Move 2).
  3. Slide ball **Right** into the alignment wall (Move 3).
  4. Slide ball **Down** directly into the black hole (Move 4).
- **Result**: Cleared in exactly 4 moves.

#### Level 2: Single Obstacle Clearance (7 Moves)
- **Grid Layout**: A long rectangular block obstructs the direct goal corridor.
- **Step-by-Step Moves**:
  1. Drag the obstructing slab down into the lower clearing pocket (Move 1).
  2. Slide ball **Right** into the newly opened boundary (Move 2).
  3. Slide ball **Up** past the cleared obstacle (Move 3).
  4. Slide ball **Left** to align with the goal lane (Move 4).
  5. Slide ball **Down** into the lower corner anchor (Move 5).
  6. Slide ball **Right** toward the goal entrance (Move 6).
  7. Slide ball **Up** into the black hole (Move 7).
- **Result**: Cleared in 7 moves with zero wasted ball maneuvers.

#### Level 3: Two-Block Corridor Opening (5 Moves)
- **Grid Layout**: Two intersecting blocks (yellow and blue) trap the ball in the starting chamber.
- **Step-by-Step Moves**:
  1. Slide yellow horizontal block **Right** to clear the vertical exit (Move 1).
  2. Slide blue vertical block **Down** into the empty buffer space (Move 2).
  3. Slide ball **Up** through the opened vertical corridor (Move 3).
  4. Slide ball **Right** into the top-right corner wall (Move 4).
  5. Slide ball **Down** into the black hole (Move 5).
#### Level 4: 2-Step Minimum Obstacle Clearance (Video Problem Walkthrough)
- **Obstacle Setup**: Vertical green block obstructing the bottom lane; horizontal blue block blocking the top corridor.
- **Step-by-Step Moves**:
  1. Slide the vertical green block **Down** into the lower empty space (1 move).
  2. Slide the horizontal blue block **Horizontally** into the vacated gap (1 move).
- **Result**: Clear line of sight for the ball to enter the goal.
- **Total Moves**: 2 steps.

### 3 Pro-Tricks for Motion Challenge
1. **Reverse Path Analysis**: Do not trace forward from the ball. Look at the black hole first: find which wall or block directly faces the hole entrance. Work backwards to determine which anchor allows the ball to reach that final position ("Which obstacle is directly blocking the target hole? Move that obstacle first").
2. **Never Move the Ball Prematurely**: Do not slide the ball while obstacles are still blocking the path. Move all sliding blocks into their final resting positions first, then execute the ball's route in uninterrupted slides.
3. **Use Static Walls as Anchors**: Since the ball slides until it hits an object, intentionally use immovable boundary walls to stop and redirect the ball at right angles.

### Motion Challenge Practice Question 1 (Q5: Multi-Block Sliding Obstacle Optimization)
**Problem**:  
A $4 \times 4$ grid contains a destination hole at $(3, 3)$. A ball starts at $(0, 0)$. Two obstacles are present:
- **Obstacle A** (size $1 \times 2$, horizontal) spans $(0, 1)$ and $(0, 2)$.
- **Obstacle B** (size $2 \times 1$, vertical) spans $(1, 0)$ and $(2, 0)$.

What is the minimum number of obstacle moves required to open at least one clear path for the ball?
- **A)** 1 Move
- **B)** 2 Moves
- **C)** 3 Moves
- **D)** 4 Moves

**Correct Answer**: **A (1 Move)**

**Derivation**:  
Moving horizontal obstacle A down or vertical obstacle B to the right takes only **1 sliding move** to open the path $(0, 0) \to (0, 1) \to (1, 1) \dots$ toward the destination $(3, 3)$.

---

### Motion Challenge Practice Question 2 (Q6: Path Optimization Trap)
**Tag**: [MOCK-EXAM]

**Problem**:  
In a Motion Challenge grid, a direct path to the goal requires navigating around an obstacle that takes 3 moves to slide completely out of the way. Alternatively, the ball can take a longer detour through an already open corridor requiring 5 ball movements.
- **Move Count Path A (Move Obstacle + Ball)**: 3 block slides + 3 ball steps = 6 total moves.
- **Move Count Path B (Ball Detour through open corridor)**: 0 block slides + 5 ball steps = 5 total moves.

Which strategy gives the optimal score?
- **A)** Path A, because block movements score higher than ball movements.
- **B)** Path B, because the game scores total moves regardless of whether a block or the ball is moved.
- **C)** Path A, because shorter physical paths are always prioritized.
- **D)** Both paths score identically.

**Correct Answer**: **B (Path B, because the game scores total moves regardless of whether a block or the ball is moved)**

**Explanation**:  
The game engine move counter increments by 1 for **any entity movement** (ball step or block translation). Total moves strictly dictate the score; Path B produces an optimal cost of 5 vs. Path A's cost of 6.

---

## 2. Bubble / Grid Memory Challenge (Visual Working Memory)

### Game Overview & Mechanics
- **Time Allotted**: 12 Minutes.
- **Format**: A cluster of circular bubbles appears on screen.
- **Sequence**: Several bubbles flash rapidly in a specific sequential order ($A \to B \to C \to \dots$).
- **Recall Phase**: Once flashing stops, the candidate must click the exact identical bubbles in the exact chronological order.
- **Progression**: Early levels start with 3 bubbles; higher levels expand to 6–7 bubbles with faster flash rates. A single incorrect click immediately fails the level.

### 3 Pro-Tricks for Bubble Memory
1. **Clock / Direction Mapping**: Convert spatial positions into verbal clock face hours or compass directions:
   - Example sequence: Top $\to$ Center $\to$ Bottom-Left $\to$ Right is mentally encoded as:  
     `"12 o'clock -> Center -> 7 o'clock -> 3 o'clock"`.
   - *Why it works*: Human verbal working memory holds sequential phonemes far more reliably than raw visual 2D coordinates.
2. **Finger / Cursor Tracing**: While bubbles flash, trace your finger or mouse pointer physically over the screen through each flashing bubble. Physical kinesthetic (motor) memory reinforces visual memory.
3. **Chunking Technique (Groups of 3)**: A 6-bubble sequence overwhelms short-term memory. Break it into two 3-item chunks:
   - Chunk 1: `"Top, Right, Center"`
   - Chunk 2: `"Bottom-Left, Bottom-Right, Top-Left"`
   - Recall Chunk 1 first, pause, then execute Chunk 2.

---

### Bubble Memory Practice Question (Q7: Cognitive Chunking Strategy)
**Tag**: [MOCK-EXAM]

**Problem**:  
During an 8-item sequential bubble memory prompt, what technique best protects against working memory decay under time pressure?
- **A)** Verbal repetition of coordinates as numerical pairs (e.g., "1-3, 2-1, 4-2").
- **B)** Tracing a continuous geometric path connecting the nodes visually.
- **C)** Splitting the sequence into two chunks of 4 items each, grouping by relative screen quadrant.
- **D)** Clicking as quickly as possible before the fade animation finishes.

**Correct Answer**: **C (Splitting the sequence into two chunks of 4 items each, grouping by relative screen quadrant)**

**Explanation**:  
Working memory capacity limits accurate recall to $4 \pm 1$ items (Cowan's working memory model). Spatial chunking (grouping items by screen region or quadrant into two 4-node batches) reduces cognitive load and prevents sequence decay.

