[Home](../README.md) > [08-recent-patterns](README.md) > 04_ai_assisted_coding.md

# 04. AI-Assisted Coding Reported Patterns

This document analyzes the 45-minute interactive AI-assisted coding round where the candidate must guide a token-budgeted AI assistant to write robust code while catching the bot's subtle bugs.

## Reported Pattern Archetypes

### 1. Bot Flaw: Grid BFS with Obstacle Elimination Quota
- **Reported In**: Gemini Chat
- **Teached By**: `AIC-030`
- **Pattern Details**: The AI assistant writes standard 2D BFS using `boolean visited[r][c]`. When a path consumes an obstacle budget $k$, a later path reaching the same cell with a larger remaining budget $k' > k$ is rejected as already visited, producing false negative reachability.
- **Correction**: 3D state tracking `visited[r][c][k]` or storing max remaining obstacles per cell `int maxK[r][c]`.

### 2. Bot Flaw: Binary Tree Boundary Traversal Tripartite Duplication
- **Reported In**: Gemini Chat
- **Teached By**: `AIC-031`
- **Pattern Details**: The bot attempts a single recursive traversal, accidentally duplicating leaf nodes or including the root node twice (in both left boundary and right boundary).
- **Correction**: Split into 3 independent methods: Left boundary (excluding leaves), all leaf nodes (via DFS), and right boundary bottom-up (excluding root and leaves).

### 3. Bot Flaw: Decode Ways Intermediate Zero Trap
- **Reported In**: Video Debriefs
- **Teached By**: `AIC-021`, `AIC-023`
- **Pattern Details**: The bot's DP transition treats every digit independently. A single `'0'` has no alphabetical mapping. Combinations like `'30'`, `'40'`, `'70'` cannot be decoded.
- **Correction**: Check `s.charAt(i-1) != '0'` for single-digit decode, and check value in `[10, 26]` for two-digit decode.

### 4. Bot Flaw: Negative Modulo Normalization in Prefix Sums
- **Reported In**: Gemini Chat
- **Teached By**: `AIC-009`
- **Pattern Details**: In Subarrays Divisible by K (`sum % k == 0`), negative prefix sums produce negative modulo remainders in C++/Java (e.g., `-2 % 5 = -2`). The bot uses negative remainders as array/map keys, crashing with index errors or failing to match equivalent positive remainders.
- **Correction**: Normalize remainder: `rem = (sum % k + k) % k`.

---

Previous: [03_debugging.md](03_debugging.md) | Next: [05_cognitive.md](05_cognitive.md)
