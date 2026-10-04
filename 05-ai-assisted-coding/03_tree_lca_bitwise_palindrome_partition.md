[Home](../README.md) > [05-ai-assisted-coding](README.md) > 03_tree_lca_bitwise_palindrome_partition.md

# AI-Assisted Coding: LCA, Bitwise Equality & Palindrome Partitioning

This guide covers the three actual and high-probability algorithmic problems tested in the Capgemini Exceller AI-Assisted (Vibe) Coding round.

---

## Round Strategy: Working with the Exam AI Assistant

- **Token Discipline**: Avoid conversational small talk ("Hello", "Can you help me?"). Evaluators track token usage and turn counts.
- **Structured Specification**: Supply the exact algorithm, base cases, data structures, and asymptotic bounds in your very first prompt.
- **Zero-Trust Review**: Never copy-paste blindly. Check:
  1. Did the bot generate 64-bit integer accumulators (`long long`) where values can overflow?
  2. Did it check empty/single-element base cases?
  3. Does it pass hidden large-$N$ constraints without $O(N^2)$ bottlenecks?

---

## Problem 1 (AIC-013): Lowest Common Ancestor (LCA) in a Binary Tree (Actual Mock Question)

### Problem Statement
Given a binary tree and two distinct nodes $p$ and $q$, find the Lowest Common Ancestor (LCA) of the two nodes. According to the definition of LCA on Wikipedia: *"The lowest common ancestor is defined between two nodes $p$ and $q$ as the lowest node in $T$ that has both $p$ and $q$ as descendants (where we allow a node to be a descendant of itself)."*

### Prompting Strategy (AI Dialog Format)
```text
Role: Senior C++ Algorithm Engineer.
Context: Tree traversal in an algorithmic assessment.
Task: Implement lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) in C++ using post-order DFS.
Logic:
1. Base cases: If root is NULL, return NULL. If root == p or root == q, return root.
2. Search subtrees: left = lowestCommonAncestor(root->left, p, q), right = lowestCommonAncestor(root->right, p, q).
3. Decision: If both left and right are non-null, root is LCA (return root). If only one is non-null, return that non-null child.
Constraints: Time O(N), Space O(H) recursion stack depth. Output clean C++ without boilerplate text.
```

### Production-Grade C++ Solution
```cpp
#include <iostream>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Base case: if reached leaf or found one of the targets
        if (!root || root == p || root == q) {
            return root;
        }

        // Post-order traversal: recurse left and right
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        // If p and q are found in different subtrees, root is their LCA
        if (left && right) {
            return root;
        }

        // Otherwise, return whichever subtree found a target node
        return left ? left : right;
    }
};
```

### Complexity
- **Time Complexity**: $O(N)$ where $N$ is the number of nodes in the binary tree (visits each node at most once).
- **Space Complexity**: $O(H)$ auxiliary recursion stack space, where $H$ is the height of the tree ($O(\log N)$ average for balanced tree, $O(N)$ worst-case for degenerate tree).

---

## Problem 2 (AIC-014): Bitwise Equality Inversions (Actual Mock Question)

### Problem Statement
Given an integer array $A$ of size $N$, compute the count of pairs $(i, j)$ such that $0 \le i < j < N$ and:
$$A[i] \ \& \ A[j] == A[i] \oplus A[j]$$

### Mathematical Deduction
We know the identity relating bitwise XOR, OR, and AND for any non-negative integers $x$ and $y$:
$$x \oplus y = (x \mid y) - (x \ \& \ y)$$

Substitute this into the problem equality:
$$x \ \& \ y = (x \mid y) - (x \ \& \ y) \implies 2 \cdot (x \ \& \ y) = x \mid y$$

Now inspect the truth table for each individual bit position $k$:

| Bit of $x$ | Bit of $y$ | $x \ \& \ y$ | $x \oplus y$ | $x \ \& \ y == x \oplus y$? |
| :---: | :---: | :---: | :---: | :---: |
| `1` | `1` | `1` | `0` | **False** ($1 \ne 0$) |
| `1` | `0` | `0` | `1` | **False** ($0 \ne 1$) |
| `0` | `1` | `0` | `1` | **False** ($0 \ne 1$) |
| `0` | `0` | `0` | `0` | **True** ($0 == 0$) |

### Mathematical Conclusion
The condition $x \ \& \ y == x \oplus y$ holds **if and only if no bit is set to 1 in either operand**.

Therefore:
$$A[i] = 0 \quad \text{and} \quad A[j] = 0$$

A pair qualifies if and only if **both numbers are zero**. The total number of valid pairs is simply the number of combinations of choosing 2 zeroes out of all zero elements ($Z$):

$$\text{Valid Pairs} = \binom{Z}{2} = \frac{Z \cdot (Z - 1)}{2}$$

### Optimal $O(N)$ C++ Solution
```cpp
#include <iostream>
#include <vector>
using namespace std;

long long countValidBitwisePairs(const vector<int>& arr) {
    long long zero_count = 0;
    
    for (int val : arr) {
        if (val == 0) {
            zero_count++;
        }
    }
    
    // Choose 2 zeros from zero_count: Z * (Z - 1) / 2
    return (zero_count * (zero_count - 1)) / 2;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    if (cin >> n) {
        vector<int> arr(n);
        for (int i = 0; i < n; i++) {
            cin >> arr[i];
        }
        cout << countValidBitwisePairs(arr) << "\n";
    }
    return 0;
}
```

### Complexity
- **Time Complexity**: $O(N)$ single pass over array.
- **Space Complexity**: $O(1)$ auxiliary storage.
- **Data Type Safety**: Uses `long long` for `zero_count` to prevent 32-bit integer overflow when $Z \approx 10^5 \implies \binom{Z}{2} \approx 5 \times 10^9$.

---

## Problem 3 (AIC-015): Palindromic Partitioning Minimum Cuts (High Probability)

### Problem Statement
Given a string $s$, find the minimum number of cuts needed to partition $s$ such that every substring in the partition is a palindrome.

### Strategy & Formulation
A naive recursion checks $O(2^N)$ partitions. We optimize to $O(N^2)$ using two Dynamic Programming steps:

1. **Palindrome Table Precomputation ($O(N^2)$)**:
   Define a 2D boolean table `isPal[i][j]` representing whether substring $s[i \dots j]$ is a palindrome:
   $$\text{isPal}[i][j] = (s[i] == s[j]) \ \&\& \ (j - i \le 2 \ \vert{}\vert{} \ \text{isPal}[i+1][j-1])$$

2. **1D DP for Minimum Cuts ($O(N^2)$)**:
   Let `dp[i]` be the minimum cuts required for the prefix $s[0 \dots i]$:
   - If `isPal[0][i] == true`, the entire prefix is a palindrome $\implies \text{dp}[i] = 0$.
   - Otherwise, iterate over all possible cut positions $j$ ($1 \le j \le i$):
     $$\text{dp}[i] = \min_{1 \le j \le i} (\text{dp}[j - 1] + 1) \quad \text{for all } j \text{ where } \text{isPal}[j][i] == \text{true}$$

### Complete C++ Solution
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        if (n <= 1) return 0;

        // Step 1: Precompute isPal[i][j]
        vector<vector<bool>> isPal(n, vector<bool>(n, false));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[i] == s[j] && (j - i <= 2 || isPal[i + 1][j - 1])) {
                    isPal[i][j] = true;
                }
            }
        }

        // Step 2: 1D DP for minimum cuts of prefix s[0..i]
        vector<int> dp(n, 0);
        for (int i = 0; i < n; i++) {
            if (isPal[0][i]) {
                dp[i] = 0; // No cuts needed if prefix is a palindrome
            } else {
                int min_cuts = i; // Worst-case: cut every character
                for (int j = 1; j <= i; j++) {
                    if (isPal[j][i]) {
                        min_cuts = min(min_cuts, dp[j - 1] + 1);
                    }
                }
                dp[i] = min_cuts;
            }
        }

        return dp[n - 1];
    }
};
```

### Complexity
- **Time Complexity**: $O(N^2)$ to build the 2D palindrome table and $O(N^2)$ to fill the 1D DP array. Total time: $O(N^2)$.
- **Space Complexity**: $O(N^2)$ for the 2D boolean palindrome table and $O(N)$ for the 1D DP array.

---

## Problem 4 (AIC-016): Palindrome Partitioning I — Return All Valid Partitions (Backtracking + 2D DP)

> **Source Analysis**: Walkthrough from the Capgemini assessment (*KN ACADEMY: Complete Capgemini One Shot Preparation | Capgemini Technical, Cognitive, English Test | Solution* — Video Reference: [Complete Capgemini Master Preparation Video](https://www.youtube.com/watch?v=q5giVVUApwM)).

### Problem Statement
Given a string $S$, partition it such that every substring in the partition is a palindrome. Return all possible valid palindrome partitions.

- **Example Input**: `s = "aab"`
- **Expected Output**: `[["a", "a", "b"], ["aa", "b"]]`

---

### Complete Multi-Turn AI Dialog Transcript (Exact Exam Q&A)

#### Turn 1: Problem Definition & Partition Constraints
> **AI Question:**  
> *"In your own words, what condition must every single substring in a valid partition satisfy, and how does this differ from simply finding a matching substring?"*

**Candidate Response:**
> "1. **Partitioning Rule**: Slicing the entire string from index 0 to $N$ with no dropped characters into contiguous segments.  
> 2. **Palindrome Requirement**: Every sliced segment must read identically forwards and backwards.  
> 3. **Difference from Search**: A search finds one isolated symmetric substring, whereas partitioning requires that all cut pieces are simultaneously valid palindromes."

---

#### Turn 2: Edge Cases & Combinatorial Bounds
> **AI Question:**  
> *"What are the guaranteed outputs for a single-character string versus a string where all characters are identical?"*

**Candidate Response:**
> "1. **Single-character string** (e.g., `'a'`): Exactly 1 partition `[['a']]`.  
> 2. **All identical characters** (e.g., `'aaaa'`): Slicing at any cut point yields valid palindromes. For a string of length $N$, there are $N - 1$ potential cut points, producing $2^{N - 1}$ possible partitions."

---

#### Turn 3: Algorithmic Strategy & Paradigm Choice
> **AI Question:**  
> *"Which algorithmic paradigm—Backtracking/DFS, Greedy, or BFS—is appropriate here and why?"*

**Candidate Response:**
> "We use recursive Backtracking (DFS). Greedy fails because picking the largest local palindrome early can leave an invalid suffix with no palindromic breakdown. Backtracking allows us to systematically explore every valid palindromic prefix, recurse on the remaining suffix, and pop elements on backtrack to try all valid cut combinations."

---

#### Turn 4: State Variables & Base Termination
> **AI Question:**  
> *"What exact state variables do you pass through each function call, and what is your base condition for saving a complete answer?"*

**Candidate Response:**
> "State Variables: `start` index, original string `s`, `currentPath` (list of valid substrings), and `result` (master list of completed partitions).  
> Base Condition: If `start == s.length()`, the entire string has been successfully sliced. We append a copy of `currentPath` to `result` and return."

---

#### Turn 5: Time Complexity & DP Optimization
> **AI Question:**  
> *"Checking palindromes on the fly takes $O(N)$ per substring, yielding an $O(N \cdot 2^N)$ complexity. How can we optimize this using DP?"*

**Candidate Response:**
> "We precompute an $N \times N$ boolean table `isPal[i][j]` using the transition:  
> `isPal[i][j] = (s[i] == s[j]) && (j - i <= 2 || isPal[i + 1][j - 1])`.  
> This reduces the palindrome check during backtracking from $O(N)$ to an $O(1)$ table lookup."

---

#### Turn 6: Code Audit & The Deliberate Outer Loop Bug
> **The AI loads generated starter code containing this intentional bug:**  
> ```cpp
> for (int i = 0; i < n; i++) {
>     for (int j = i; j < n; j++) {
>         if (s[i] == s[j] && (j - i <= 2 || isPal[i + 1][j - 1])) ...
> ```

**Candidate Audit & Fix:**
> *"The forward loop `for (int i = 0; i < n; i++)` is invalid because `isPal[i][j]` relies on `isPal[i + 1][j - 1]`, which has not been computed yet for row $i + 1$. Uncomputed entries default to `false`, causing valid palindromes to be incorrectly skipped.  
> We must iterate row `i` in reverse: `for (int i = n - 1; i >= 0; i--)` to ensure subproblems are resolved before they are read."*

---

### Production-Ready Verified Solutions

#### C++ Implementation
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

class Solution {
private:
    vector<vector<string>> result;
    vector<string> currentPath;
    vector<vector<bool>> isPal;

    void backtrack(const string& s, int start, int n) {
        // Base case: Entire string processed successfully
        if (start == n) {
            result.push_back(currentPath);
            return;
        }

        // Try every possible cut boundary
        for (int end = start; end < n; ++end) {
            // O(1) lookup using the precomputed DP table
            if (isPal[start][end]) {
                // Include current palindrome prefix
                currentPath.push_back(s.substr(start, end - start + 1));
                // Recurse on remaining suffix
                backtrack(s, end + 1, n);
                // Backtrack: Remove choice to explore other cut points
                currentPath.pop_back();
            }
        }
    }

public:
    vector<vector<string>> partition(string s) {
        int n = s.length();
        result.clear();
        currentPath.clear();
        isPal.assign(n, vector<bool>(n, false));

        // Precompute palindrome states
        // Crucial fix: Iterate row i backwards to resolve subproblems before reading
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                if (s[i] == s[j] && (j - i <= 2 || isPal[i + 1][j - 1])) {
                    isPal[i][j] = true;
                }
            }
        }

        backtrack(s, 0, n);
        return result;
    }
};

int main() {
    Solution solver;
    auto res = solver.partition("aab");
    for (const auto& path : res) {
        cout << "[";
        for (int i = 0; i < path.size(); ++i) {
            cout << "\"" << path[i] << "\"" << (i + 1 < path.size() ? ", " : "");
        }
        cout << "]\n";
    }
    return 0;
}
```

#### Java Implementation
```java
import java.util.ArrayList;
import java.util.List;

public class PalindromePartitioning {
    private List<List<String>> result;
    private List<String> currentPath;
    private boolean[][] isPal;

    public List<List<String>> partition(String s) {
        int n = s.length();
        result = new ArrayList<>();
        currentPath = new ArrayList<>();
        isPal = new boolean[n][n];

        // Fill DP table from bottom to top
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s.charAt(i) == s.charAt(j) && (j - i <= 2 || isPal[i + 1][j - 1])) {
                    isPal[i][j] = true;
                }
            }
        }

        backtrack(s, 0, n);
        return result;
    }

    private void backtrack(String s, int start, int n) {
        if (start == n) {
            result.add(new ArrayList<>(currentPath));
            return;
        }

        for (int end = start; end < n; end++) {
            if (isPal[start][end]) {
                currentPath.add(s.substring(start, end + 1));
                backtrack(s, end + 1, n);
                currentPath.remove(currentPath.size() - 1);
            }
        }
    }
}
```

### Complexity
- **Time Complexity**: $O(N^2 + N \cdot 2^{N - 1})$ — $O(N^2)$ to precompute the palindrome DP table, and $O(N \cdot 2^{N - 1})$ worst-case to generate and copy all palindromic partitions.
- **Space Complexity**: $O(N^2)$ for the 2D boolean palindrome table and $O(N)$ for recursion call stack depth.

---

Previous: [02_prefix_sum_hashmap.md](02_prefix_sum_hashmap.md) | Next: [04_graph_bipartite_course_schedule_islands.md](04_graph_bipartite_course_schedule_islands.md)
