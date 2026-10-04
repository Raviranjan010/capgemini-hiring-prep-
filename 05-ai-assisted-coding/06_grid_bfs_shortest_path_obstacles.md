[Home](../README.md) > [05-ai-assisted-coding](README.md) > 06_grid_bfs_shortest_path_obstacles.md

# AI Assist Coding: Shortest Path in 2D Grid with Obstacles Elimination

**Tag**: [AI-ASSIST-CODING] [BFS] [VIDEO]  
**Timestamp**: `[00:18:52]` - `[00:20:31]`  
**LeetCode Reference**: [LeetCode 1293: Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)  
**Video Reference**: KN Academy Capgemini Technical Assessment 28 Sept Video

---

## Problem 1 (AIC-030): Shortest Path in 2D Grid with Obstacles Elimination

You are given an $N \times M$ 2D integer grid containing integers `0` and `1`:
- `0` represents a passable empty cell.
- `1` represents an obstacle.

You begin at the top-left cell $(0, 0)$ and must reach the bottom-right cell $(N-1, M-1)$ moving in 4 cardinal directions (Up, Down, Left, Right). Each move costs 1 step. You are allowed to eliminate at most $K$ obstacles along your path.

Return the **minimum steps** required to walk from $(0, 0)$ to $(N-1, M-1)$, or return `-1` if the destination is unreachable.

### Visual Representation
```text
(0,0) Start [0] ──> [0] ──> [1] (Obstacle: Destroy using 1 quota)
                             │
                            [0] ──> [0] ──> (N-1, M-1) Destination
```

---

## 2. Why Candidates Lose Tokens with the AI Bot

In Capgemini's AI Assist interface, candidates have a limited token budget (~2,000 tokens). A widespread failure pattern is submitting vague prompts:

> ❌ **Vague Candidate Prompt**: *"Write code to reach the end of a grid with obstacles in Java."*

### Why the AI Assistant Fails
1. The AI bot defaults to standard 2D Dynamic Programming:
   $$\text{dp}[i][j] = \min(\text{dp}[i-1][j], \text{dp}[i][j-1]) + 1$$
2. **The Flaw**: Standard 2D DP only handles **Right and Down** (Directed Acyclic Graph) transitions. It cannot model 4-directional movements (cycles) and cannot maintain dynamic obstacle elimination quotas ($K$).
3. **Token Drain**: Candidates notice failing test cases and spend 1,000+ tokens going back and forth trying to patch the DP table, eventually exhausting their token budget before reaching a working solution.

---

## 3. High-Scoring One-Shot Prompting Template

To steer the bot to the optimal solution in a single prompt and preserve your token pool:

```text
"I need an optimal Java solution for: Shortest Path in a Grid with Obstacles Elimination.
- Grid: N x M with values 0 (empty) and 1 (obstacle).
- Movements: 4-directional (Up, Down, Left, Right). Each move costs 1 step.
- Constraint: Can eliminate at most K obstacles.
- Approach: Breadth-First Search (BFS) for unweighted shortest path.
- State: Queue contains (row, col, steps, remaining_k).
- Visited Tracking: Maintain a 2D array visited[row][col] storing the maximum remaining_k 
  seen at that cell to prune suboptimal paths.
- Base Cases & Optimizations:
  * If n == 1 && m == 1, return 0.
  * If k >= (n + m - 2), return (n + m - 2) (Manhattan distance optimization).
  * If start cell cannot be traversed, handle accordingly.
Please provide the complete, clean BFS implementation with optimal time and space complexity."
```

---

## 4. Optimal Algorithmic Architecture

### State-Space BFS with Pruning
- An unweighted shortest path on a 2D grid requires **Breadth-First Search (BFS)** because the first time we pop the destination $(N-1, M-1)$ from the queue, it is guaranteed to have the minimum steps.
- **State Representation**: $(r, c, \text{steps}, \text{remaining\_k})$.
- **Pruning Matrix (`visited[r][c]`)**: Instead of storing a 3D boolean array `visited[N][M][K+1]`, store a 2D integer array `visited[r][c]` holding the **maximum remaining obstacle quota** with which $(r, c)$ was previously reached.
  - If we reach $(r, c)$ with a smaller or equal remaining $K$, discard the state (pruned).
  - If we reach $(r, c)$ with a strictly higher remaining $K$, update `visited[r][c] = nextK` and push to queue.
- **Manhattan Distance Shortcut**: The shortest possible path without any obstacles from $(0, 0)$ to $(N-1, M-1)$ has length $(N - 1) + (M - 1) = N + M - 2$. If $K \ge N + M - 2$, we can simply destroy every obstacle along the direct Manhattan path.

---

## 5. Production Implementations

### Java Solution
```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Queue;

public class ShortestPathGridObstacles {

    public static int shortestPath(int[][] grid, int k) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return -1;

        int n = grid.length;
        int m = grid[0].length;

        // Base case: already at destination
        if (n == 1 && m == 1) return 0;

        // Manhattan Distance Optimization:
        // If allowed obstacle removals >= shortest path length, take the direct path
        if (k >= n + m - 2) {
            return n + m - 2;
        }

        // visited[r][c] stores the highest remaining 'k' with which cell (r, c) was visited
        int[][] visited = new int[n][m];
        for (int[] row : visited) {
            Arrays.fill(row, -1);
        }

        // Queue stores: {row, col, steps, remaining_k}
        Queue<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[]{0, 0, 0, k});
        visited[0][0] = k;

        int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int r = curr[0];
            int c = curr[1];
            int steps = curr[2];
            int remK = curr[3];

            // Target reached
            if (r == n - 1 && c == m - 1) {
                return steps;
            }

            for (int[] dir : directions) {
                int nr = r + dir[0];
                int nc = c + dir[1];

                // Boundary check
                if (nr >= 0 && nr < n && nc >= 0 && nc < m) {
                    int nextK = remK - grid[nr][nc];

                    // Valid move if quota is non-negative and offers strictly better remaining k
                    if (nextK >= 0 && nextK > visited[nr][nc]) {
                        visited[nr][nc] = nextK;
                        queue.offer(new int[]{nr, nc, steps + 1, nextK});
                    }
                }
            }
        }

        return -1; // Destination unreachable
    }

    public static void main(String[] args) {
        int[][] grid1 = {
            {0, 0, 0},
            {1, 1, 0},
            {0, 0, 0},
            {0, 1, 1},
            {0, 0, 0}
        };
        System.out.println(shortestPath(grid1, 1)); // Expected: 6

        int[][] grid2 = {
            {0, 1, 1},
            {1, 1, 1},
            {1, 0, 0}
        };
        System.out.println(shortestPath(grid2, 1)); // Expected: -1
    }
}
```

### C++ Solution
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct State {
    int r, c, steps, remK;
};

int shortestPath(vector<vector<int>>& grid, int k) {
    int n = grid.size();
    int m = grid[0].size();

    if (n == 1 && m == 1) return 0;

    // Manhattan distance optimization
    if (k >= n + m - 2) return n + m - 2;

    // visited[r][c] stores the highest remaining k seen so far
    vector<vector<int>> visited(n, vector<int>(m, -1));
    queue<State> q;

    q.push({0, 0, 0, k});
    visited[0][0] = k;

    int dr[4] = {0, 1, 0, -1};
    int dc[4] = {1, 0, -1, 0};

    while (!q.empty()) {
        auto [r, c, steps, remK] = q.front();
        q.pop();

        if (r == n - 1 && c == m - 1) return steps;

        for (int i = 0; i < 4; i++) {
            int nr = r + dr[i];
            int nc = c + dc[i];

            if (nr >= 0 && nr < n && nc >= 0 && nc < m) {
                int nextK = remK - grid[nr][nc];
                if (nextK >= 0 && nextK > visited[nr][nc]) {
                    visited[nr][nc] = nextK;
                    q.push({nr, nc, steps + 1, nextK});
                }
            }
        }
    }

    return -1;
}
```

---

## 6. Complexity Analysis

| Metric | Complexity | Explanation |
| :--- | :---: | :--- |
| **Time Complexity** | $O(N \cdot M \cdot K)$ | In the worst case, each cell $(r, c)$ can be visited at most $K$ times (once for each strictly increasing remaining quota). |
| **Space Complexity** | $O(N \cdot M \cdot K)$ | The BFS queue holds at most $O(N \cdot M \cdot K)$ states; auxiliary space for the `visited` matrix is $O(N \cdot M)$. |

---

## 7. AI Assist Defense Checklist

- [ ] **Reject 2D DP**: If the bot suggests `dp[i][j] = dp[i-1][j] + dp[i][j-1]`, reject it immediately. Point out that 4-directional moves and obstacle quotas require BFS.
- [ ] **State Representation**: Verify the queue contains `steps` and `remaining_k`.
- [ ] **Visited Optimization**: Use `visited[r][c] = remaining_k` instead of a 3D boolean table to minimize memory overhead.
- [ ] **Manhattan Shortcut**: Include `if (k >= n + m - 2) return n + m - 2;` to bypass BFS on dense obstacle maps.

---

Previous: [05_dp_decode_ways_and_frequent_patterns.md](05_dp_decode_ways_and_frequent_patterns.md) | Next: [07_binary_tree_boundary_traversal.md](07_binary_tree_boundary_traversal.md)
