[Home](../README.md) > [04-code-debugging](README.md) > 05_graphs_and_dp.md

# 05. Graph Algorithms & Dynamic Programming Debugging

This module covers edge registration, queue state updates, cycle detection backtracks, and DP directionality and offset indexing.

## The 4 Primary Failure Modes in Exam Debugging
1. **Unidirectional vs. Bidirectional Edges**: In undirected graphs, failing to push edges in both directions fragments connected components.
2. **Premature Loop Returns**: Placing `return result;` inside a `for` loop body terminates the algorithm prematurely.
3. **0-Indexed vs. 1-Indexed Allocation**: Allocating size $n$ instead of $n + 1$ or accessing out-of-bounds indices in DP tables.
4. **Forward vs. Backward Capacity Traversal**: Using forward loop instead of reverse traversal in space-optimized 0/1 Knapsack arrays.

---

## Problem 1 (DBG-014): Connected Components in Undirected Graph
**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Graph DFS
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=q5giVVUApwM)

### Problem Statement
Given $n$ vertices and an edge list of an undirected graph, return the number of connected components.

### Sample Input & Output
- **Input**: `n = 5`, `edges = [[0,1], [1,2], [3,4]]` -> `2`

### Buggy Exam Code
```cpp
int countComponents(int n, vector<vector<int>>& edges) {
    vector<vector<int>> adj(n);
    for (auto& edge : edges) {
        adj[edge[0]].push_back(edge[1]); // Bug 1: Only adds one-way edge!
    }
    vector<bool> visited(n, false);
    int count = 0;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            count++;
            dfs(i, adj, visited);
            return count; // Bug 2: Premature return inside loop!
        }
    }
    return count;
}
```

### Bugs Found & Why They Are Wrong
1. **Unidirectional Edge Push**: In an undirected graph, edges must be bidirectional (`adj[u].push_back(v)` and `adj[v].push_back(u)`). Storing only one direction breaks reachability.
2. **Premature Return Inside Loop**: Placing `return count;` inside the first `if (!visited[i])` block returns 1 immediately after traversing the first component, ignoring all other components.

### Fixed Code
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        boolean[] visited = new boolean[n];
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                count++;
                dfs(i, adj, visited);
            }
        }
        return count;
    }
    
    private void dfs(int u, List<List<Integer>> adj, boolean[] visited) {
        visited[u] = true;
        for (int v : adj.get(u)) {
            if (!visited[v]) dfs(v, adj, visited);
        }
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int countComponents(int n, std::vector<std::vector<int>>& edges) {
        std::vector<std::vector<int>> adj(n);
        for (const auto& edge : edges) {
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]);
        }
        std::vector<bool> visited(n, false);
        int count = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                count++;
                dfs(i, adj, visited);
            }
        }
        return count;
    }
private:
    void dfs(int u, const std::vector<std::vector<int>>& adj, std::vector<bool>& visited) {
        visited[u] = true;
        for (int v : adj[u]) {
            if (!visited[v]) dfs(v, adj, visited);
        }
    }
};
```
</details>

### Dry-Run Table
| Vertex `i` | Visited Before Check? | Action | Vertices Marked in DFS | Component Count |
| :---: | :---: | :--- | :--- | :---: |
| 0 | False | New Component DFS | `0, 1, 2` | 1 |
| 1 | True | Skipped | None | 1 |
| 2 | True | Skipped | None | 1 |
| 3 | False | New Component DFS | `3, 4` | 2 |
| 4 | True | Skipped | None | 2 |

### Spot-It-Fast Rule
Undirected graphs MUST add edges both ways: `adj[u].add(v)` AND `adj[v].add(u)`. Return count MUST be outside the outer loop.

### Edge Cases
1. Graph with no edges ($E = 0$): Returns $n$ isolated components.
2. Completely connected graph: Returns `1`.

- **Complexity**: Time: $O(V + E)$, Space: $O(V + E)$.

---

## Problem 2 (DBG-015): 0/1 Knapsack Space-Optimized DP (Direction Bug)
**Tag**: [VIDEO] | **Difficulty**: Hard | **Topic**: Dynamic Programming
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=q5giVVUApwM)

### Problem Statement
Given weights and values of $n$ items, find the maximum value that can be put in a knapsack of capacity $W$. Each item can be picked at most once (0/1 Knapsack).

### Sample Input & Output
- **Input**: `values = [60, 100, 120]`, `weights = [10, 20, 30]`, `W = 50` -> `220`

### Buggy Exam Code
```java
class Knapsack {
    public static int knapSack(int W, int wt[], int val[], int n) {
        int[] dp = new int[W + 1];
        for (int i = 0; i < n; i++) {
            // Bug: Forward loop enables multiple inclusion (Unbounded Knapsack)!
            for (int w = wt[i]; w <= W; w++) {
                dp[w] = Math.max(dp[w], val[i] + dp[w - wt[i]]);
            }
        }
        return dp[W];
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Forward Capacity Traversal**: Looping forward `w = wt[i]; w <= W` uses the updated value `dp[w - wt[i]]` computed in the *current* item iteration, allowing an item to be selected multiple times. In 0/1 Knapsack, the capacity loop MUST run in reverse (`w = W down to wt[i]`).

### Fixed Code
```java
class Knapsack {
    public static int knapSack(int W, int wt[], int val[], int n) {
        int[] dp = new int[W + 1];
        for (int i = 0; i < n; i++) {
            for (int w = W; w >= wt[i]; w--) {
                dp[w] = Math.max(dp[w], val[i] + dp[w - wt[i]]);
            }
        }
        return dp[W];
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Knapsack {
public:
    static int knapSack(int W, const std::vector<int>& wt, const std::vector<int>& val, int n) {
        std::vector<int> dp(W + 1, 0);
        for (int i = 0; i < n; i++) {
            for (int w = W; w >= wt[i]; w--) {
                dp[w] = std::max(dp[w], val[i] + dp[w - wt[i]]);
            }
        }
        return dp[W];
    }
};
```
</details>

### Dry-Run Table
| Item `(wt, val)` | Capacity `w` Scanned | Computation (`Math.max(dp[w], val + dp[w-wt])`) | Resulting `dp` Array State |
| :---: | :---: | :--- | :--- |
| `(10, 60)` | $50 \dots 10$ (reverse) | `dp[10..50] = 60` | `[0, ..., 60, ..., 60]` |
| `(20, 100)` | $50 \dots 20$ (reverse) | `dp[30..50] = max(60, 100 + 60) = 160` | `dp[30]=160` |
| `(30, 120)` | $50 \dots 30$ (reverse) | `dp[50] = max(160, 120 + 100) = 220` | `dp[50]=220` |

### Spot-It-Fast Rule
1D 0/1 Knapsack: Outer loop over items, inner loop backwards `for (int w = W; w >= wt[i]; w--)`. Forward loop is unbounded knapsack.

### Edge Cases
1. $W = 0$: Returns `0`.
2. Item weight exceeds capacity $W$: Inner loop does not execute for that item.

- **Complexity**: Time: $O(N \times W)$, Space: $O(W)$.

---

## Problem 3 (DBG-016): Cycle in Directed Graph (Missing Backtrack Reset)
**Tag**: [VIDEO] | **Difficulty**: Hard | **Topic**: Graph DFS
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=q5giVVUApwM)

### Problem Statement
Given a directed graph with $V$ vertices and $E$ edges, determine if it contains a cycle.

### Sample Input & Output
- **Input**: `V = 4`, `adj = [[1], [2], [3], []]` -> `false` (linear DAG)
- **Input**: `V = 3`, `adj = [[1], [2], [0]]` -> `true` (cycle `0 -> 1 -> 2 -> 0`)

### Buggy Exam Code
```java
class Solution {
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] vis = new boolean[V];
        boolean[] inStack = new boolean[V];
        
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                if (dfs(i, adj, vis, inStack)) return true;
            }
        }
        return false;
    }
    
    private boolean dfs(int u, ArrayList<ArrayList<Integer>> adj, boolean[] vis, boolean[] inStack) {
        vis[u] = true;
        inStack[u] = true;
        
        for (int v : adj.get(u)) {
            if (!vis[v]) {
                if (dfs(v, adj, vis, inStack)) return true;
            } else if (inStack[v]) {
                return true;
            }
        }
        // Bug: Missing inStack[u] = false!
        return false;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Recursion Stack Backtrack Reset**: `inStack[u]` records whether vertex $u$ is in the current DFS path. Failing to set `inStack[u] = false` when exiting $u$ causes nodes from completely different, acyclic branches to falsely flag cycles.

### Fixed Code
```java
class Solution {
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] vis = new boolean[V];
        boolean[] inStack = new boolean[V];
        
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                if (dfs(i, adj, vis, inStack)) return true;
            }
        }
        return false;
    }
    
    private boolean dfs(int u, ArrayList<ArrayList<Integer>> adj, boolean[] vis, boolean[] inStack) {
        vis[u] = true;
        inStack[u] = true;
        
        for (int v : adj.get(u)) {
            if (!vis[v]) {
                if (dfs(v, adj, vis, inStack)) return true;
            } else if (inStack[v]) {
                return true;
            }
        }
        inStack[u] = false; // Essential backtrack reset
        return false;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    bool isCyclic(int V, const std::vector<std::vector<int>>& adj) {
        std::vector<bool> vis(V, false);
        std::vector<bool> inStack(V, false);
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                if (dfs(i, adj, vis, inStack)) return true;
            }
        }
        return false;
    }
private:
    bool dfs(int u, const std::vector<std::vector<int>>& adj, std::vector<bool>& vis, std::vector<bool>& inStack) {
        vis[u] = true;
        inStack[u] = true;
        for (int v : adj[u]) {
            if (!vis[v]) {
                if (dfs(v, adj, vis, inStack)) return true;
            } else if (inStack[v]) {
                return true;
            }
        }
        inStack[u] = false;
        return false;
    }
};
```
</details>

### Dry-Run Table
| Vertex Explored | `inStack` Active Path | Backtrack Action | `inStack` After Return |
| :---: | :---: | :---: | :---: |
| 0 | `{0}` | Calls 1 | `{0, 1}` |
| 1 | `{0, 1}` | No outgoing edges | Reset `inStack[1] = false` |
| 0 | `{0}` | Calls 2 | `{0, 2}` (1 is not inStack!) |

### Spot-It-Fast Rule
Cycle in directed graph: `inStack[u] = true` before loop; MUST have `inStack[u] = false` before `return false`.

### Edge Cases
1. Self-loop (`u -> u`): Visited check matches `inStack[u] == true` immediately; returns `true`.
2. Disconnected acyclic components: Backtrack prevents cross-component false positives.

- **Complexity**: Time: $O(V + E)$, Space: $O(V)$.

---

## Problem 4 (DBG-017): BFS Traversal of Undirected Graph (4 Exam Bugs)
**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Graph BFS
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=q5giVVUApwM)

### Problem Statement
Given an undirected connected graph, return a list containing the BFS traversal of the graph starting from vertex 0.

### Sample Input & Output
- **Input**: `V = 5`, `adj = [[1,2,3], [], [4], [], []]` -> `[0, 1, 2, 3, 4]`

### Buggy Exam Code
```java
class Solution {
    public ArrayList<Integer> bfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        ArrayList<Integer> bfs = new ArrayList<>();
        boolean[] vis = new boolean[V];
        Queue<Integer> q = new LinkedList<>();
        
        q.add(0); // Bug 1: Forgot to mark vis[0] = true!
        
        while (!q.isEmpty()) {
            Integer node = q.peek(); // Bug 2: peek() without poll() causes infinite loop!
            bfs.add(node);
            
            for (Integer it : adj.get(node)) {
                if (vis[it] == false) {
                    q.add(it);
                    // Bug 3: Delaying visited marking causes duplicate queue inserts!
                }
            }
            vis[node] = true;
        }
        return bfs;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Initial Visited Marking**: `vis[0]` is not set to `true` when adding 0 to the queue. When neighbors explore edges back to 0, 0 is re-enqueued.
2. **Missing `q.poll()`**: `q.peek()` reads the front node without removing it, creating an infinite loop.
3. **Late Visited Marking**: Setting `vis[it] = true` after popping rather than upon enqueue allows multiple neighbors to enqueue the same node simultaneously.

### Fixed Code
```java
class Solution {
    public ArrayList<Integer> bfsOfGraph(int V, ArrayList<ArrayList<Integer>> adj) {
        ArrayList<Integer> bfs = new ArrayList<>();
        boolean[] vis = new boolean[V];
        Queue<Integer> q = new LinkedList<>();
        
        vis[0] = true;
        q.add(0);
        
        while (!q.isEmpty()) {
            int node = q.poll();
            bfs.add(node);
            
            for (int neighbor : adj.get(node)) {
                if (!vis[neighbor]) {
                    vis[neighbor] = true;
                    q.add(neighbor);
                }
            }
        }
        return bfs;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<int> bfsOfGraph(int V, const std::vector<std::vector<int>>& adj) {
        std::vector<int> bfs;
        std::vector<bool> vis(V, false);
        std::queue<int> q;
        vis[0] = true;
        q.push(0);
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            bfs.push_back(node);
            for (int neighbor : adj[node]) {
                if (!vis[neighbor]) {
                    vis[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
        return bfs;
    }
};
```
</details>

### Dry-Run Table
| Dequeued Node | Neighbors Checked | Newly Marked & Enqueued | Queue State | Traversal Order |
| :---: | :---: | :---: | :---: | :--- |
| 0 | `1, 2, 3` | `1, 2, 3` | `[1, 2, 3]` | `[0]` |
| 1 | `[]` | None | `[2, 3]` | `[0, 1]` |
| 2 | `[4]` | `4` | `[3, 4]` | `[0, 1, 2]` |
| 3 | `[]` | None | `[4]` | `[0, 1, 2, 3]` |
| 4 | `[]` | None | `[]` | `[0, 1, 2, 3, 4]` |

### Spot-It-Fast Rule
Mark visited immediately upon `q.add(neighbor)`, never when popping from queue.

### Edge Cases
1. Single node ($V = 1$): Enqueues 0, pops 0, returns `[0]`.
2. Star graph (0 connected to all others): Level 1 enqueues all nodes once.

- **Complexity**: Time: $O(V + E)$, Space: $O(V)$.

---

## Problem 5 (DBG-034): Topological Sort (Kahn's Algorithm) In-Degree Bug
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Graph Topological Sort
**Source**: Added practice

### Problem Statement
Given a Directed Acyclic Graph (DAG) with $V$ vertices and $E$ edges, return a topological sorting of the vertices.

### Sample Input & Output
- **Input**: `V = 4`, `edges = [[1, 0], [2, 0], [3, 1], [3, 2]]` -> `[3, 1, 2, 0]` or `[3, 2, 1, 0]`

### Buggy Exam Code
```java
class Solution {
    public int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {
        int[] inDegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int v : adj.get(i)) {
                inDegree[i]++; // Bug 1: Increments source i instead of destination v!
            }
        }
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (inDegree[i] == 0) q.add(i);
        }
        int[] res = new int[V];
        int idx = 0;
        while (!q.isEmpty()) {
            int u = q.poll();
            res[idx++] = u;
            for (int v : adj.get(u)) {
                if (inDegree[v] == 0) { // Bug 2: Checks 0 before decrementing!
                    q.add(v);
                }
            }
        }
        return res;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Wrong In-Degree Target**: `inDegree[i]++` counts out-degree of vertex $i$. In-degree belongs to destination $v$ (`inDegree[v]++`).
2. **Missing In-Degree Decrement**: Edges from $u$ to $v$ are removed by decrementing `inDegree[v]--`. Checking `inDegree[v] == 0` without decrementing leaves in-degrees positive.

### Fixed Code
```java
class Solution {
    public int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {
        int[] inDegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int v : adj.get(i)) {
                inDegree[v]++;
            }
        }
        
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (inDegree[i] == 0) q.add(i);
        }
        
        int[] res = new int[V];
        int idx = 0;
        while (!q.isEmpty()) {
            int u = q.poll();
            res[idx++] = u;
            for (int v : adj.get(u)) {
                inDegree[v]--;
                if (inDegree[v] == 0) {
                    q.add(v);
                }
            }
        }
        return res;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<int> topoSort(int V, const std::vector<std::vector<int>>& adj) {
        std::vector<int> inDegree(V, 0);
        for (int i = 0; i < V; i++) {
            for (int v : adj[i]) inDegree[v]++;
        }
        std::queue<int> q;
        for (int i = 0; i < V; i++) {
            if (inDegree[i] == 0) q.push(i);
        }
        std::vector<int> res;
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            res.push_back(u);
            for (int v : adj[u]) {
                if (--inDegree[v] == 0) q.push(v);
            }
        }
        return res;
    }
};
```
</details>

### Dry-Run Table
| Step | Dequeued Vertex | Neighbors Decremented | In-Degree Reached 0 | Queue State |
| :---: | :---: | :--- | :--- | :--- |
| Init | - | - | 3 (in-degree 0) | `[3]` |
| 1 | 3 | `1` (0), `2` (0) | 1, 2 | `[1, 2]` |
| 2 | 1 | `0` (1 -> 0 remains from 2) | None | `[2]` |
| 3 | 2 | `0` (0) | 0 | `[0]` |
| 4 | 0 | None | None | `[]` |

### Spot-It-Fast Rule
Kahn's algorithm: Increment `inDegree[v]++`, then in BFS loop: `if (--inDegree[v] == 0) q.add(v);`.

### Edge Cases
1. Graph contains cycle: Queue empties before visiting all $V$ nodes (`idx < V`).
2. Single-node graph: Directly enqueues and returns `[0]`.

- **Complexity**: Time: $O(V + E)$, Space: $O(V)$.

---

## Problem 6 (DBG-035): Dijkstra's Shortest Path Priority Queue Visited Bug
**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Shortest Path
**Source**: Added practice

### Problem Statement
Given a weighted, undirected graph and a source vertex `S`, find the shortest distance from `S` to all other vertices.

### Sample Input & Output
- **Input**: `V = 3`, `edges = [[0, 1, 1], [1, 2, 2], [0, 2, 4]]`, `S = 0` -> `[0, 1, 3]`

### Buggy Exam Code
```java
class Solution {
    public int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[S] = 0;
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]); // Sorts by distance
        pq.add(new int[]{S, 0});
        
        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int u = curr[0], d = curr[1];
            
            // Bug: Missing stale entry check (if (d > dist[u]) continue;) causes TLE!
            for (ArrayList<Integer> edge : adj.get(u)) {
                int v = edge.get(0), weight = edge.get(1);
                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.add(new int[]{v, dist[v]});
                }
            }
        }
        return dist;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Stale Pair Eviction**: In Dijkstra without decrease-key, vertices are pushed to the priority queue multiple times. Without checking `if (d > dist[u]) continue;`, outdated paths with suboptimal distances are fully expanded, degrading performance from $O(E \log V)$ to exponential on dense graphs (TLE).

### Fixed Code
```java
class Solution {
    public int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[S] = 0;
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(a[1], b[1]));
        pq.add(new int[]{S, 0});
        
        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int u = curr[0], d = curr[1];
            
            if (d > dist[u]) continue; // Discard outdated path
            
            for (ArrayList<Integer> edge : adj.get(u)) {
                int v = edge.get(0), weight = edge.get(1);
                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.add(new int[]{v, dist[v]});
                }
            }
        }
        return dist;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<int> dijkstra(int V, const std::vector<std::vector<std::pair<int, int>>>& adj, int S) {
        std::vector<int> dist(V, 1e9);
        dist[S] = 0;
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<>> pq;
        pq.push({0, S});
        while (!pq.empty()) {
            auto [d, u] = pq.top();
            pq.pop();
            if (d > dist[u]) continue;
            for (auto& [v, w] : adj[u]) {
                if (dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                    pq.push({dist[v], v});
                }
            }
        }
        return dist;
    }
};
```
</details>

### Dry-Run Table
| Popped `(u, d)` | Current `dist[u]` | Stale (`d > dist[u]`)? | Neighbors Relaxed |
| :---: | :---: | :---: | :--- |
| `(0, 0)` | 0 | No | `dist[1] = 1, dist[2] = 4` |
| `(1, 1)` | 1 | No | `dist[2] = min(4, 1 + 2) = 3` |
| `(2, 3)` | 3 | No | None |
| `(2, 4)` | 3 | **Yes ($4 > 3$)** | **Discarded immediately!** |

### Spot-It-Fast Rule
Always guard Dijkstra priority queue pops with: `if (d > dist[u]) continue;`.

### Edge Cases
1. Unreachable vertices: Retain `Integer.MAX_VALUE`.
2. Graph with multiple paths to same node: Stale check prevents duplicate processing.

- **Complexity**: Time: $O(E \log V)$, Space: $O(V + E)$.

---

## Problem 7 (DBG-036): Coin Change Minimum Coins Unbounded DP Loop Bug
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Dynamic Programming
**Source**: Added practice

### Problem Statement
Given an integer array `coins` representing coins of different denominations and an integer `amount`, return the fewest number of coins that you need to make up that amount. If impossible, return `-1`.

### Sample Input & Output
- **Input**: `coins = [1, 2, 5]`, `amount = 11` -> `3` ($5 + 5 + 1$)

### Buggy Exam Code
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        // Bug 1: Default array is 0, so Math.min always picks 0!
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }
        return dp[amount];
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Uninitialized DP Table**: In Java, integer arrays default to `0`. `Math.min(0, 1 + dp[i - coin])` will always return `0`, leaving `dp[amount] = 0` for all inputs.
2. **Missing Base Case**: `dp[0]` must equal `0`, and all other cells `1` to `amount` must be initialized to a large value (like `amount + 1`).

### Fixed Code
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int coinChange(const std::vector<int>& coins, int amount) {
        std::vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] = std::min(dp[i], 1 + dp[i - coin]);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```
</details>

### Dry-Run Table
| Capacity `i` | Coin `1` | Coin `2` | Coin `5` |
| :---: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 |
| 2 | 2 | 1 | 1 |
| 5 | 5 | 3 | 1 |
| 11 | 11 | 6 | **3** |

### Spot-It-Fast Rule
Minimization DP table must be filled with `amount + 1` (or infinity) before computing `Math.min()`. Return `-1` if `dp[amount] > amount`.

### Edge Cases
1. `amount == 0`: Returns `0`.
2. Amount cannot be formed (e.g. `coins = [2]`, `amount = 3`): Returns `-1`.

- **Complexity**: Time: $O(N \times \text{amount})$, Space: $O(\text{amount})$.

---

## Problem 8 (DBG-037): Longest Common Subsequence Table Offset Bug
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Dynamic Programming
**Source**: Added practice

### Problem Statement
Given two strings `text1` and `text2`, return the length of their longest common subsequence (LCS).

### Sample Input & Output
- **Input**: `text1 = "abcde"`, `text2 = "ace"` -> `3`

### Buggy Exam Code
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m][n]; // Bug 1: Size m x n instead of (m+1) x (n+1) causes out-of-bounds on i-1
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i) == text2.charAt(j)) { // Bug 2: 1-indexed i reads out of bounds in 0-indexed string!
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```

### Bugs Found & Why They Are Wrong
1. **DP Allocation Size**: Initializing `dp` as `[m][n]` crashes when accessing `dp[m][n]` or when checking base index `i - 1 = -1`. The table must be `[m + 1][n + 1]`.
2. **String Character Index Offset**: Loop indices run `1` to `m`. Accessing `text1.charAt(i)` causes `StringIndexOutOfBoundsException` when `i = m`. Must read `text1.charAt(i - 1)`.

### Fixed Code
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int longestCommonSubsequence(const std::string& text1, const std::string& text2) {
        int m = text1.length(), n = text2.length();
        std::vector<std::vector<int>> dp(m + 1, std::vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = std::max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
};
```
</details>

### Dry-Run Table
| `text1[i-1]` | `text2[j-1]` | Match? | Transition | Value `dp[i][j]` |
| :---: | :---: | :---: | :---: | :---: |
| 'a' | 'a' | Yes | `1 + dp[0][0]` | 1 |
| 'c' | 'c' | Yes | `1 + dp[2][1]` | 2 |
| 'e' | 'e' | Yes | `1 + dp[4][2]` | 3 |

### Spot-It-Fast Rule
LCS 2D DP table: Allocate `(m + 1) x (n + 1)`. When loop is 1-indexed, character check is `charAt(i - 1)`.

### Edge Cases
1. No common characters: Returns `0`.
2. Identical strings: Returns `length`.

- **Complexity**: Time: $O(M \times N)$, Space: $O(M \times N)$.

---

Previous: [04_linkedlist_stack_backtracking.md](04_linkedlist_stack_backtracking.md) | Next: [06_kadane_and_binary_search.md](06_kadane_and_binary_search.md)
