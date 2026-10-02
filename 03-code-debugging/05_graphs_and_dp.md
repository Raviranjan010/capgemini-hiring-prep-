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

