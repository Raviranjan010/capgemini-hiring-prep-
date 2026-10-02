# Capgemini AI-Assisted (Vibe) Coding — Master Study Notes

> **Source Analysis**: Based on the updated Capgemini hiring pattern and technical walkthrough (*KN ACADEMY: Capgemini AI Assist Coding Solution | Actual Capgemini Preparation | Capgemini Coding | Updated Pattern* — Video Reference: [KN Academy AI-Assisted Coding Tutorial](http://www.youtube.com/watch?v=tqXkeM3D8D4)).

---

## 1. Exam Mechanics & Evaluation Architecture

In the updated Capgemini hiring pattern, traditional standalone code editors have been replaced in this round with an **Interactive AI Co-Pilot / Chatbot Interface**. Candidates collaborate with an embedded AI agent to verify problem invariants, design algorithms, justify complexities, and audit starter code.

### Interactive AI Coding Pipeline

```text
Interactive AI Coding Pipeline
├── 1. Problem Specification & Test Cases Displayed
├── 2. Click "Start AI Discussion"
├── 3. Structured Multi-Turn Dialogue:
│   ├── Turn 1: Problem Definition & Invariant Verification
│   ├── Turn 2: Algorithmic Selection & Disconnected Component Handling
│   ├── Turn 3: Conflict Detection & State Resolution
│   ├── Turn 4: Asymptotic Complexity Justification
│   └── Turn 5: Edge Case Taxonomy
├── 4. Starter Code Generation by AI
└── 5. Candidate Code Review, Verification & Manual Editing
```

### Critical Rules & Constraints

1. **Bounded Token / Turn Budget**:
   - You cannot submit casual chit-chat, conversational filler, or unstructured reasoning.
   - Every dialogue turn evaluates technical terminology, concise formulations, and algorithmic clarity.
2. **No Direct Solution Spoon-Feeding**:
   - The AI agent will not write the full code upfront when simply asked "give me the code".
   - The bot asks targeted questions to verify whether you understand *why* the data structure or algorithm works.
3. **Starter Code Audit**:
   - The AI-generated code frequently contains **deliberate bugs**, missing boundary loops, or suboptimal data structures.
   - Candidates must audit and refine the generated implementation before compiling and running tests.

---

## 2. In-Depth Walkthrough: Is Graph Bipartite? (BFS 2-Coloring)

### Problem Statement
Given an undirected graph represented as an adjacency list, determine whether the graph is **Bipartite**.

A graph is **bipartite** if its vertices can be partitioned into two independent sets $U$ and $V$ such that every edge $(u, v)$ connects a vertex from $U$ to a vertex from $V$ (no two adjacent vertices belong to the same group):

$$\forall (u, v) \in E, \quad u \in U \iff v \in V$$

---

### Core Theoretical Concepts & Invariants

#### 1. 2-Colorability Equivalence
Assign each vertex one of two colors ($0$ or $1$). If two adjacent vertices ever require the same color, the graph is not bipartite:
$$\text{color}[v] = \text{color}[u] \oplus 1$$

#### 2. The Odd-Length Cycle Theorem
$$\text{A graph is bipartite} \iff \text{It contains NO odd-length cycles}$$

* **Cycle of Length 6 (Even)**: Vertices $1 \to 2 \to 3 \to 4 \to 5 \to 6 \to 1$.
  * Coloring alternation: $0 \to 1 \to 0 \to 1 \to 0 \to 1 \to 0$. Adjacent colors never clash $\implies$ **Bipartite (`true`)**.
* **Cycle of Length 5 (Odd)**: Vertices $1 \to 2 \to 3 \to 4 \to 5 \to 1$.
  * Coloring alternation: $0 \to 1 \to 0 \to 1 \to 0$. Edge $(5, 1)$ connects two vertices of color $0$ $\implies$ **Not Bipartite (`false`)**.

#### 3. Disconnected Component Coverage
Because the graph may contain multiple isolated subgraphs or disconnected components, an outer loop must iterate over every vertex $0 \le i < V$. If $\text{color}[i] == -1$, initiate a new BFS component traversal.

---

### Complete AI Dialog Script (Exact Video Q&A)

Here are the questions asked by the AI agent and the optimal technical responses:

#### Turn 1: Theoretical Foundation
> **AI Question:**  
> *"Can you explain what makes an undirected graph bipartite, and how coloring the vertices with two different colors helps determine this property?"*

**Candidate Response:**
> "A graph is bipartite when its vertices can be partitioned into two disjoint sets such that every edge connects vertices across opposing sets. We model these sets using colors 0 and 1. When a vertex is assigned color C, all its uncolored neighbors must receive $1 \oplus C$ (opposite color). If any edge connects two vertices that already share the identical color, the bipartite property is violated and we immediately terminate."

---

#### Turn 2: Algorithmic Selection & Disconnected Graphs
> **AI Question:**  
> *"Why is BFS suitable for this problem, and how will you handle graphs containing multiple disconnected components?"*

**Candidate Response:**
> "BFS explores vertices level-by-level (breadth-first), allowing us to assign colors in alternating concentric layers from any source. To support disconnected components, we maintain an outer loop from $0$ to $V - 1$. Whenever an uncolored node ($\text{color} == -1$) is found, we initiate an independent BFS traversal for that component."

---

#### Turn 3: Conflict Detection & State Resolution
> **AI Question:**  
> *"During BFS, when you encounter a neighbor that is already colored, what check must be performed and what conclusion do you draw?"*

**Candidate Response:**
> "We inspect whether $\text{color}[\text{neighbor}] == \text{color}[\text{current\_node}]$. If equal, an odd cycle is detected because two adjacent nodes occupy the same set, so we return false immediately. If the neighbor has the opposite color, the edge is valid and we proceed without re-enqueuing."

---

#### Turn 4: Asymptotic Complexity Justification
> **AI Question:**  
> *"What time and space complexity does your BFS-based solution achieve, and how do V and E impact this?"*

**Candidate Response:**
> "Time Complexity is $O(V + E)$ because every vertex is enqueued at most once and every undirected edge is examined at most twice. Space Complexity is $O(V)$ for the color array and queue, while the adjacency list occupies $O(V + E)$ auxiliary space."

---

#### Turn 5: Edge Case Taxonomy
> **AI Question:**  
> *"What critical edge cases must be handled during testing?"*

**Candidate Response:**
> "1. Graph with 0 edges (isolated vertices): Bipartite (`true`).  
> 2. Single vertex: Bipartite (`true`).  
> 3. Disconnected graph with individually bipartite components: Bipartite (`true`).  
> 4. Graph containing self-loops: NOT bipartite (`false`), since a node cannot differ in color from itself.  
> 5. Odd-length cycle vs. Even-length cycle: `false` vs. `true`."

---

### Production-Grade Verified Solution

#### C++ Implementation
```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

class Solution {
public:
    bool isBipartite(const vector<vector<int>>& graph) {
        int n = graph.size();
        // Color array: -1 represents unvisited/uncolored
        vector<int> color(n, -1);

        // Outer loop handles disconnected components
        for (int i = 0; i < n; ++i) {
            if (color[i] != -1) continue;

            // Initialize BFS queue for new component
            queue<int> q;
            q.push(i);
            color[i] = 0; // Seed color

            while (!q.empty()) {
                int u = q.front();
                q.pop();

                for (int v : graph[u]) {
                    // Case 1: Neighbor is not yet colored
                    if (color[v] == -1) {
                        // Assign opposite color using bitwise XOR: 0 ^ 1 = 1, 1 ^ 1 = 0
                        color[v] = color[u] ^ 1;
                        q.push(v);
                    }
                    // Case 2: Neighbor already has the same color -> Odd cycle detected
                    else if (color[v] == color[u]) {
                        return false;
                    }
                }
            }
        }
        return true; // All components successfully 2-colored
    }
};

int main() {
    // Example: Disconnected Bipartite Graph
    vector<vector<int>> graph = {
        {1, 3}, // Node 0
        {0, 2}, // Node 1
        {1, 3}, // Node 2
        {0, 2}  // Node 3
    };
    Solution solver;
    cout << (solver.isBipartite(graph) ? "Bipartite" : "Not Bipartite") << "\n";
    return 0;
}
```

#### Java Implementation
```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Queue;

public class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        Arrays.fill(color, -1); // -1 = unvisited

        for (int i = 0; i < n; i++) {
            if (color[i] != -1) continue;

            Queue<Integer> queue = new ArrayDeque<>();
            queue.offer(i);
            color[i] = 0;

            while (!queue.isEmpty()) {
                int u = queue.poll();

                for (int v : graph[u]) {
                    if (color[v] == -1) {
                        color[v] = color[u] ^ 1; // Opposite color
                        queue.offer(v);
                    } else if (color[v] == color[u]) {
                        return false; // Adjacent clash
                    }
                }
            }
        }
        return true;
    }
}
```

**Complexity**:
- **Time Complexity**: $O(V + E)$ where $V$ is vertices and $E$ is edges.
- **Space Complexity**: $O(V)$ for queue and color array.

---

## 3. High-Probability AI-Assisted Coding Question Bank

### Problem 1: Course Schedule / Cycle Detection in Directed Graph (Kahn's Algorithm)

#### Problem Statement
Given $N$ courses ($0$ to $N - 1$) and a prerequisite pair list `prerequisites[i] = [a, b]` meaning course $b$ must be completed before course $a$, determine if all courses can be finished.

#### Formulated AI Dialogue Script
> **AI:** *"What algorithm should we use, and what constitutes an impossible schedule?"*

**Candidate Response:**
> "We model this as a Directed Graph where an edge $b \to a$ represents a prerequisite dependency. Completing all courses is impossible if and only if the graph contains a directed cycle. We use Kahn's Algorithm (BFS topological sort) by tracking the in-degree of all vertices. Vertices with in-degree $0$ are added to a queue. As each course is dequeued, we decrement its neighbors' in-degrees. If the total number of processed courses equals $N$, the schedule is valid ($O(V + E)$ time, $O(V + E)$ space)."

#### Production C++ Code
```cpp
#include <vector>
#include <queue>

using namespace std;

bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> adj(numCourses);
    vector<int> inDegree(numCourses, 0);

    for (const auto& edge : prerequisites) {
        adj[edge[1]].push_back(edge[0]);
        inDegree[edge[0]]++;
    }

    queue<int> q;
    for (int i = 0; i < numCourses; ++i) {
        if (inDegree[i] == 0) q.push(i);
    }

    int processed = 0;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        processed++;

        for (int v : adj[u]) {
            if (--inDegree[v] == 0) {
                q.push(v);
            }
        }
    }
    return processed == numCourses;
}
```

#### Production Java Code
```java
import java.util.*;

public class CourseSchedule {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());
        int[] inDegree = new int[numCourses];

        for (int[] p : prerequisites) {
            adj.get(p[1]).add(p[0]);
            inDegree[p[0]]++;
        }

        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) queue.offer(i);
        }

        int count = 0;
        while (!queue.isEmpty()) {
            int u = queue.poll();
            count++;
            for (int v : adj.get(u)) {
                if (--inDegree[v] == 0) {
                    queue.offer(v);
                }
            }
        }
        return count == numCourses;
    }
}
```

---

### Problem 2: Number of Connected Islands (Multi-Source BFS / Flood Fill)

#### Problem Statement
Given an $m \times n$ 2D binary grid representing a map of `'1'`s (land) and `'0'`s (water), return the total number of connected islands.

#### Formulated AI Dialogue Script
> **AI:** *"How should we avoid mutating input memory or prevent infinite recursion across cycles?"*

**Candidate Response:**
> "We iterate through every cell $(r, c)$. Upon finding a '1', we increment our island count and launch a BFS traversal. We sink the visited land cells in-place by setting `grid[r][c] = '0'` upon queue insertion, preventing duplicate visits without needing extra $O(M \times N)$ visited matrix space. The queue checks all 4 cardinal directions within grid boundaries. Complexity: $O(M \times N)$ time, $O(\min(M, N))$ auxiliary queue space."

#### Production C++ Code
```cpp
#include <vector>
#include <queue>

using namespace std;

int numIslands(vector<vector<char>>& grid) {
    if (grid.empty() || grid[0].empty()) return 0;
    int m = grid.size(), n = grid[0].size();
    int islands = 0;
    int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (grid[i][j] == '1') {
                islands++;
                grid[i][j] = '0'; // Sink cell immediately upon discovery
                queue<pair<int, int>> q;
                q.push({i, j});

                while (!q.empty()) {
                    auto [r, c] = q.front();
                    q.pop();

                    for (auto& d : dirs) {
                        int nr = r + d[0], nc = c + d[1];
                        if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == '1') {
                            grid[nr][nc] = '0'; // Sink immediately on push
                            q.push({nr, nc});
                        }
                    }
                }
            }
        }
    }
    return islands;
}
```

#### Production Java Code
```java
import java.util.ArrayDeque;
import java.util.Queue;

public class NumIslands {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        int m = grid.length, n = grid[0].length;
        int islands = 0;
        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    islands++;
                    grid[i][j] = '0'; // Sink cell
                    Queue<int[]> queue = new ArrayDeque<>();
                    queue.offer(new int[]{i, j});

                    while (!queue.isEmpty()) {
                        int[] curr = queue.poll();
                        int r = curr[0], c = curr[1];
                        for (int[] d : dirs) {
                            int nr = r + d[0], nc = c + d[1];
                            if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == '1') {
                                grid[nr][nc] = '0'; // Sink on push
                                queue.offer(new int[]{nr, nc});
                            }
                        }
                    }
                }
            }
        }
        return islands;
    }
}
```

---

### Problem 3: 0/1 Matrix Shortest Distance (Multi-Source BFS)

#### Problem Statement
Given an $m \times n$ binary matrix `mat`, return the distance of the nearest `0` for each cell. The distance between two adjacent cells is $1$.

#### Formulated AI Dialogue Script
> **AI:** *"Why is running single-source BFS from each '1' inefficient, and what is the optimal alternative?"*

**Candidate Response:**
> "Running a BFS from every '1' cell individually takes $O((M \times N)^2)$ time, which will time out. Instead, we use Multi-Source BFS: initialize all 0 cells with distance 0 and push them all into our BFS queue simultaneously, while setting all 1 cells to -1 (unvisited). As we expand outward level-by-level, the first time we visit an unvisited cell gives its shortest path from any zero. This reduces total time complexity to $O(M \times N)$."

#### Production C++ Code
```cpp
#include <vector>
#include <queue>

using namespace std;

vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int m = mat.size(), n = mat[0].size();
    vector<vector<int>> dist(m, vector<int>(n, -1));
    queue<pair<int, int>> q;

    // Multi-source initialization: enqueue all 0 cells
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (mat[i][j] == 0) {
                dist[i][j] = 0;
                q.push({i, j});
            }
        }
    }

    int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    while (!q.empty()) {
        auto [r, c] = q.front();
        q.pop();

        for (auto& d : dirs) {
            int nr = r + d[0], nc = c + d[1];
            if (nr >= 0 && nr < m && nc >= 0 && nc < n && dist[nr][nc] == -1) {
                dist[nr][nc] = dist[r][c] + 1;
                q.push({nr, nc});
            }
        }
    }
    return dist;
}
```

#### Production Java Code
```java
import java.util.ArrayDeque;
import java.util.Queue;

public class ZeroOneMatrix {
    public int[][] updateMatrix(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[][] dist = new int[m][n];
        Queue<int[]> queue = new ArrayDeque<>();

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0;
                    queue.offer(new int[]{i, j});
                } else {
                    dist[i][j] = -1; // Unvisited sentinel
                }
            }
        }

        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int r = curr[0], c = curr[1];

            for (int[] d : dirs) {
                int nr = r + d[0], nc = c + d[1];
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && dist[nr][nc] == -1) {
                    dist[nr][nc] = dist[r][c] + 1;
                    queue.offer(new int[]{nr, nc});
                }
            }
        }
        return dist;
    }
}
```

**Complexity**:
- **Time Complexity**: $O(M \times N)$ — each matrix cell is pushed into the queue at most once.
- **Space Complexity**: $O(M \times N)$ for distance output and BFS queue.

---

## 4. Assessment Strategy: Chatbot Dialogue Management

### Dialogue Response Blueprint

```text
Dialogue Response Blueprint
├── 1. Answer Promptly with Technical Precision:
│   └── Mention exact terms: "In-degree", "Bipartite", "Topological Order", "Odd-length cycle".
├── 2. Specify Alternating Flips using Bitwise Logic:
│   └── State: "color[v] = color[u] ^ 1" rather than verbose if-else conditions.
├── 3. Highlight Graph Disconnections Proactively:
│   └── State: "An outer loop iterates from 0 to V - 1 to handle disconnected subgraphs."
└── 4. Audit the Generated Starter Code:
    └── Check for three common generated bugs:
        ├── Missing outer loop for disconnected components
        ├── Strict equality error (color[neighbor] == color[curr] instead of assignment)
        └── Forgetting to check self-loops (adj[i] containing i)
```

### The 4 Golden Rules for Dialogue Interaction
1. **Never use colloquial or conversational phrases**: Avoid *"I think we should probably..."* Instead write: *"We implement Kahn's Algorithm by tracking vertex in-degrees."*
2. **Always state both Time and Space bounds simultaneously**: Include auxiliary space, queue size, and call stack overhead in Turn 4.
3. **Preemptively identify edge cases**: Mention empty graphs, self-loops, disconnected forests, and single-node instances before the AI prompts you.
4. **Inspect starter code before compiling**: AI-generated starter code intentionally omits the outer disconnected-components loop or reverses neighbor coloring conditions. Fix these immediately.
