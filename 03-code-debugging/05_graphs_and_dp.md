# Debugging Graphs & Dynamic Programming

This guide covers high-frequency graph and dynamic programming bugs from the Capgemini Exceller 20-minute code debugging assessment.

---

## The 4 Primary Failure Modes in Exam Debugging

1. **Unidirectional vs. Bidirectional Edges**: In undirected graphs, inserting only `adj[u].push_back(v)` without `adj[v].push_back(u)` causes disconnected component counts to explode.
2. **Premature Loop Returns**: Accidental placement of `return result;` inside the outer `for` loop body terminates the algorithm on the first iteration.
3. **Array Bounds & Index Offsets**: Mismatch between 0-indexed arrays and 1-indexed DP tables (allocating size $n$ instead of $n + 1$ or accessing `dp[n]`).
4. **Forward vs. Backward Capacity Traversal in 1D DP**:
   - Forward loop (`w = wt[i] to W`): Converts 0/1 Knapsack into **Unbounded Knapsack** (items reused infinitely).
   - Backward loop (`w = W down to wt[i]`): Enforces **0/1 Knapsack** (each item used at most once).

---

## Problem 1: Connected Components in an Undirected Graph (Actual Mock Question)

### Problem Statement
Given $n$ vertices labeled $0$ to $n-1$ and an undirected edge list `edges` where `edges[i] = [u, v]`, return the total number of connected components in the graph.

### The Buggy Code (Provided in Exam Interface)
```cpp
// INCORRECT CODE (Provided in exam interface)
#include <vector>
using namespace std;

void dfs(int node, vector<vector<int>>& adj, vector<bool>& vis) {
    vis[node] = true;
    for (int neighbor : adj[node]) {
        if (!vis[neighbor]) {
            dfs(neighbor, adj, vis);
        }
    }
}

int countComponents(int n, vector<vector<int>>& edges) {
    vector<vector<int>> adj(n);
    for (auto edge : edges) {
        adj[edge[0]].push_back(edge[1]); // BUG 1: Only one-way edge pushed
    }
    vector<bool> vis(n, false);
    int components = 0;
    for (int i = 0; i < n; i++) {
        if (!vis[i]) {
            dfs(i, adj, vis);
            components++;
        }
        return components; // BUG 2: premature return inside loop
    }
    // BUG 3: Missing return outside loop if n == 0
}
```

### Bugs Identified
1. **Unidirectional Insertion**: The problem specifies an **undirected** graph. The buggy code only registers `adj[edge[0]].push_back(edge[1])`. Any search starting at `edge[1]` cannot traverse back to `edge[0]`, fragmenting connected subgraphs into false isolated components.
2. **Premature Loop Exit**: The `return components;` statement is placed inside the `for` loop body. Execution halts immediately after examining vertex `i = 0`, returning `1` regardless of how many other vertices exist.
3. **Missing Return Statement**: If $n = 0$, execution falls through without returning any integer, triggering undefined behavior or compiler errors (`control reaches end of non-void function`).

### Spotting & 10-Second Fix
```diff
     for (auto edge : edges) {
         adj[edge[0]].push_back(edge[1]);
+        adj[edge[1]].push_back(edge[0]); // FIX 1: Add reverse link for undirected graph
     }
     vector<bool> vis(n, false);
     int components = 0;
     for (int i = 0; i < n; i++) {
         if (!vis[i]) {
+            components++;
             dfs(i, adj, vis);
-            components++;
         }
-        return components; // BUG: Premature return inside loop
     }
+    return components; // FIX 2: Return outside loop after scanning all vertices
```

### Fully Verified C++ Solution
```cpp
#include <vector>
using namespace std;

class Solution {
private:
    void dfs(int node, const vector<vector<int>>& adj, vector<bool>& vis) {
        vis[node] = true;
        for (int neighbor : adj[node]) {
            if (!vis[neighbor]) {
                dfs(neighbor, adj, vis);
            }
        }
    }

public:
    int countComponents(int n, const vector<vector<int>>& edges) {
        if (n <= 0) return 0;
        
        vector<vector<int>> adj(n);
        for (const auto& edge : edges) {
            adj[edge[0]].push_back(edge[1]);
            adj[edge[1]].push_back(edge[0]); // Bidirectional insertion
        }
        
        vector<bool> vis(n, false);
        int components = 0;
        
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                components++;
                dfs(i, adj, vis);
            }
        }
        
        return components; // Return after full DFS traversal
    }
};
```

### Complexity
- **Time Complexity**: $O(V + E)$ where $V = n$ and $E = \text{edges.size()}$. Every vertex and edge is visited at most twice.
- **Space Complexity**: $O(V + E)$ for adjacency list storage and $O(V)$ for the boolean visited array and recursion stack.

---

## Problem 2: 0/1 Knapsack Space-Optimized DP (High-Frequency Debugging)

### Problem Statement
Given a maximum capacity $W$ and arrays of item weights `wt` and values `val` of size $n$, find the maximum value subset of items that fits within capacity $W$, where each item can be chosen **at most once**.

### The Buggy Code (Provided in Exam Interface)
```cpp
// INCORRECT CODE
#include <vector>
#include <algorithm>
using namespace std;

int knapsack(int W, vector<int>& wt, vector<int>& val, int n) {
    vector<int> dp(W + 1, 0);
    
    for (int i = 0; i < n; i++) {
        for (int w = wt[i]; w <= W; w++) { // BUG: Forward loop allows multiple item usage
            dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }
    
    return dp[W];
}
```

### Bug Analysis
- In space-optimized 1D Dynamic Programming:
  - If the inner capacity loop runs **forward** (`w = wt[i]; w <= W; w++`), the updated value `dp[w - wt[i]]` computed in the **current item's iteration** is immediately used to calculate `dp[w]`.
  - This allows the same item $i$ to be selected multiple times, transforming the problem from a **0/1 Knapsack** into an **Unbounded (Complete) Knapsack**.
- **The Fix**: The inner loop must traverse **backwards** from $W$ down to $wt[i]$ (`w = W; w >= wt[i]; w--`). This guarantees that `dp[w - wt[i]]` always references the state from the **previous item's iteration** ($i - 1$).

### Spotting & 10-Second Fix
```diff
     for (int i = 0; i < n; i++) {
-        for (int w = wt[i]; w <= W; w++) { // Forward traversal = Unbounded Knapsack
+        for (int w = W; w >= wt[i]; w--) { // Reverse traversal = 0/1 Knapsack
             dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
         }
     }
```

### Fully Verified C++ Solution
```cpp
#include <vector>
#include <algorithm>
using namespace std;

int knapsack(int W, const vector<int>& wt, const vector<int>& val, int n) {
    if (W <= 0 || n <= 0) return 0;
    
    vector<int> dp(W + 1, 0);
    
    for (int i = 0; i < n; i++) {
        // Reverse traversal ensures each item is used at most once
        for (int w = W; w >= wt[i]; w--) {
            dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }
    
    return dp[W];
}
```

### Complexity
- **Time Complexity**: $O(n \times W)$ pseudopolynomial time.
- **Space Complexity**: $O(W)$ using a single 1D rolling array.

---

## Problem 3: Cycle in a Directed Graph (Missing Backtrack Reset)

**Tag**: [MOCK-EXAM]  
**Practice Link**: [LeetCode: course-schedule](https://leetcode.com/problems/course-schedule/)

### Problem Statement
Given a directed graph with $V$ vertices labeled $0$ to $V-1$ and an adjacency list `adj`, determine whether the graph contains a directed cycle.

### Buggy Exam Code
```cpp
// BUGGY: Node remains permanently marked in call stack
bool dfs(int u, vector<vector<int>>& adj, vector<bool>& vis, vector<bool>& inStack) {
    vis[u] = true;
    inStack[u] = true;

    for (int v : adj[u]) {
        if (!vis[v] && dfs(v, adj, vis, inStack)) return true;
        else if (inStack[v]) return true;
    }

    // BUG: Missing inStack[u] = false on backtrack!
    return false;
}

bool hasCycle(int V, vector<vector<int>>& adj) {
    vector<bool> vis(V, false);
    vector<bool> inStack(V, false);
    for (int i = 0; i < V; ++i) {
        if (!vis[i] && dfs(i, adj, vis, inStack)) return true;
    }
    return false;
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Recursion Backtracking Reset (`inStack[u] = false;`)**:
   - *Why wrong*: `inStack[u]` denotes whether vertex $u$ is in the **current recursion call stack path**. If `inStack[u]` is never reset to `false` when backtracking, vertex $u$ remains marked as active.
   - *Failure Scenario*: In a diamond-shaped DAG: $0 \to 1 \to 3$ and $0 \to 2 \to 3$ (no cycle). When the DFS finishes exploring path $0 \to 1 \to 3$ and returns, it later explores $0 \to 2 \to 3$. When it reaches $3$ from $2$, it finds `inStack[3] == true` and falsely flags a non-existent cycle!
   - *Fix*: Unmark the current node upon exiting the function: `inStack[u] = false;` right before `return false;`.

### Fixed Production Code

#### C++
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    bool dfs(int u, const vector<vector<int>>& adj, vector<bool>& vis, vector<bool>& inStack) {
        vis[u] = true;
        inStack[u] = true;

        for (int v : adj[u]) {
            if (!vis[v]) {
                if (dfs(v, adj, vis, inStack)) return true;
            } else if (inStack[v]) {
                // Back-edge detected -> cycle exists
                return true;
            }
        }

        // BACKTRACK: Remove node from current recursion path
        inStack[u] = false;
        return false;
    }

    bool hasCycle(int V, const vector<vector<int>>& adj) {
        vector<bool> vis(V, false);
        vector<bool> inStack(V, false);

        for (int i = 0; i < V; ++i) {
            if (!vis[i]) {
                if (dfs(i, adj, vis, inStack)) return true;
            }
        }
        return false;
    }
};
```

### Time & Space Complexity
- **Time Complexity**: $O(V + E)$ — Every vertex and edge is visited at most once.
- **Space Complexity**: $O(V)$ — Visited and inStack arrays plus recursion stack depth.

---

## Problem 4: BFS Traversal of an Undirected Graph (Exam-Level Debugging)

> **Source Analysis**: Walkthrough from the Capgemini assessment (*KN ACADEMY: Complete Capgemini One Shot Preparation | Capgemini Technical, Cognitive, English Test | Solution* — Video Reference: [Complete Capgemini Master Preparation Video](http://www.youtube.com/watch?v=q5giVVUApwM)).

### Problem Statement
Given an unweighted connected undirected graph with $V$ vertices labeled $0$ to $V-1$ represented as an adjacency list `adj`, return a vector containing the standard BFS traversal sequence starting from vertex $0$.

```text
Graph Structure (5 Vertices):
      0 ──── 1
     / \
    2   3
     \
      4

Adjacency List:
0: [1, 2, 3]
1: [0]
2: [0, 4]
3: [0]
4: [2]
```

### The Buggy Code (Provided in Exam Interface)
```cpp
// BUGGY SOURCE CODE (Contains multiple logical and compiler flaws)
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    // BUG 1: Passed by value (creates expensive copy and drops pointer modifications)
    vector<int> bfsOfGraph(int V, vector<vector<int>> adj) {
        vector<int> bfs;
        vector<bool> vis(V, false);
        queue<int> q;

        q.push(0);
        // BUG 2: Vertex 0 was pushed into queue, but NEVER marked as visited!

        while (!q.empty()) {
            int node = q.front();
            // BUG 3: q.front() read, but node is NEVER popped! Causes infinite loop!

            bfs.push_back(node);

            for (auto neighbor : adj[node]) {
                if (!vis[neighbor]) {
                    q.push(neighbor);
                    // BUG 4: Pushed neighbor to queue without setting vis[neighbor] = true!
                    // Causes duplicate entries when multiple nodes share neighbors!
                }
            }
        }
        return bfs;
    }
};
```

### Bugs Breakdown & Step-by-Step Fixes

1. **Bug 1: Adjacency List Passed by Value (`vector<vector<int>> adj`)**:
   - *Why it fails*: Passing large nested containers by value creates an expensive deep copy of the entire graph on every call, leading to memory bloat and performance degradation.
   - *Fix*: Pass by constant reference: `const vector<vector<int>>& adj`.

2. **Bug 2: Missing Root Visited Mark (`vis[0] = true;`)**:
   - *Why it fails*: Node $0$ is enqueued with `q.push(0);` but `vis[0]` remains `false`. When node $1$ (a neighbor of $0$) is later expanded, it sees $0$ marked unvisited and pushes $0$ back into the queue, producing an infinite cycle.
   - *Fix*: Add `vis[0] = true;` immediately after `q.push(0);`.

3. **Bug 3: Missing Queue Pop (`q.pop();`)**:
   - *Why it fails*: The code reads `int node = q.front();` but never removes the element. Because `q.empty()` remains permanently `false` and `q.front()` always returns `0`, the loop spins infinitely until the platform times out (TLE).
   - *Fix*: Add `q.pop();` right after extracting `q.front()`.

4. **Bug 4: Delayed Visited Marking (Duplicate Enqueue Bug)**:
   - *Why it fails*: Pushing a neighbor into the queue without marking `vis[neighbor] = true` allows another vertex that shares the same neighbor to push it a second time before the first instance is popped.
   - *Example*: Node $0$ is connected to $2$ and $3$, and both $2$ and $3$ connect to $4$. Without immediate marking on push, node $4$ gets pushed twice into the queue!
   - *Fix*: Set `vis[neighbor] = true;` **at the moment of enqueueing**, not at the moment of popping.

---

### Corrected, Production-Ready Solutions

#### C++ Implementation
```cpp
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    vector<int> bfsOfGraph(int V, const vector<vector<int>>& adj) {
        vector<int> bfs;
        vector<bool> vis(V, false);
        queue<int> q;

        // Initialize BFS from node 0
        q.push(0);
        vis[0] = true; // FIX 2: Root marked visited immediately

        while (!q.empty()) {
            int node = q.front();
            q.pop(); // FIX 3: Remove processed node to prevent infinite loop

            bfs.push_back(node);

            for (int neighbor : adj[node]) {
                if (!vis[neighbor]) {
                    vis[neighbor] = true; // FIX 4: Mark visited on push to prevent duplicates
                    q.push(neighbor);
                }
            }
        }
        return bfs; // Returns [0, 1, 2, 3, 4]
    }
};
```

#### Java Implementation
```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<Integer> bfsOfGraph(int V, List<List<Integer>> adj) {
        List<Integer> bfs = new ArrayList<>();
        boolean[] vis = new boolean[V];
        Queue<Integer> q = new ArrayDeque<>();

        // Enqueue root and mark visited
        q.offer(0);
        vis[0] = true;

        while (!q.isEmpty()) {
            int node = q.poll(); // Reads and removes front element
            bfs.add(node);

            for (int neighbor : adj.get(node)) {
                if (!vis[neighbor]) {
                    vis[neighbor] = true; // Mark visited on push
                    q.offer(neighbor);
                }
            }
        }
        return bfs;
    }
}
```

### Complexity
- **Time Complexity**: $O(V + E)$ — Each vertex is pushed and popped exactly once, and each undirected edge is scanned twice.
- **Space Complexity**: $O(V)$ — For the visited boolean array and the BFS queue.


