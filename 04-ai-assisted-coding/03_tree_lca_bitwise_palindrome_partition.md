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

## Problem 1: Lowest Common Ancestor (LCA) in a Binary Tree (Actual Mock Question)

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

## Problem 2: Bitwise Equality Inversions (Actual Mock Question)

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

## Problem 3: Palindromic Partitioning Minimum Cuts (High Probability)

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
