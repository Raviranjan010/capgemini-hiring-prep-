# Tree Traversal & Balancing Debugging

---

## Problem 1: Height-Balanced Binary Tree ($O(N)$ DFS)

**Tag**: [VIDEO]  
**Video Reference**: [KN Academy Height-Balanced Tree Debugging](http://www.youtube.com/watch?v=YEZy2e_PARE) (Verified)  
**Practice Link**: [LeetCode: balanced-binary-tree](https://leetcode.com/problems/balanced-binary-tree/)

### Problem Statement
Given a binary tree, determine if it is height-balanced. A binary tree is height-balanced if the depth of the two subtrees of every node never differs by more than 1.

### Sample Input & Output
- **Input**: `root = [3, 9, 20, null, null, 15, 7]`
- **Output**: `true`
- **Input**: `root = [1, 2, 2, 3, 3, null, null, 4, 4]`
- **Output**: `false`

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
class Solution {
    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }

    public int checkHeight(TreeNode node) {
        if (node == null) return 0;

        int left = checkHeight(node.left);
        int right = checkHeight(node.right);

        // BUG 1: Returning 0 masks unbalanced subtree failure (or using && requiring both to fail simultaneously)
        if (left == -1 && right == -1) return 0;

        // BUG 2: >= 1 flags valid trees with height difference of 1 as invalid
        if (Math.abs(left - right) >= 1) return -1;

        // BUG 3: Missing height calculation return statement (fails to compile or breaks ancestor checks)
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Sentinel Suppression & Faulty Conjunction (`left == -1 && right == -1 return 0;`)**:
   - *Why wrong*: Using `&&` requires both subtrees to fail simultaneously. If only the left subtree is unbalanced (`left == -1`) while the right is valid, execution continues. Furthermore, returning `0` treats an unbalanced tree as balanced with height 0, hiding previous errors.
   - *Fix*: If either branch returns `-1`, immediately propagate `-1` up the call stack (`if (left == -1 || right == -1) return -1;`).
2. **Checking `>= 1` instead of `> 1` (`Math.abs(left - right) >= 1`)**:
   - *Why wrong*: A height difference of exactly 1 is valid in a balanced binary tree. Only a difference strictly greater than 1 (`> 1`) violates balance.
3. **Missing Return Statement**:
   - *Why wrong*: After verifying the node is balanced, the function must return its true height: `1 + Math.max(left, right)`. Failing to return this causes compilation failure or broken height calculations for ancestor nodes.

### Fixed Production Code

#### Java
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }

    private int checkHeight(TreeNode node) {
        if (node == null) return 0;

        int left = checkHeight(node.left);
        if (left == -1) return -1; // FIX 1: Propagate -1 immediately

        int right = checkHeight(node.right);
        if (right == -1) return -1; // FIX 1: Propagate -1 immediately

        // FIX 2: Strictly greater than 1 violates balance
        if (Math.abs(left - right) > 1) return -1;

        // FIX 3: Return node height
        return Math.max(left, right) + 1;
    }
}
```

#### C++
```cpp
#include <algorithm>
#include <cmath>

class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return checkHeight(root) != -1;
    }

private:
    int checkHeight(TreeNode* node) {
        if (!node) return 0;

        int left = checkHeight(node->left);
        if (left == -1) return -1;

        int right = checkHeight(node->right);
        if (right == -1) return -1;

        if (std::abs(left - right) > 1) return -1;

        return std::max(left, right) + 1;
    }
};
```

### Dry Run (Small 4-Node Tree)
Tree Structure:
```text
       1
      / \
     2   3
    /
   4
```

| Node | Left Height | Right Height | $| \text{Left} - \text{Right} |$ | Balanced? | Return Value |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **4** | `0` (null) | `0` (null) | $|0 - 0| = 0$ | Yes ($\le 1$) | $\max(0,0) + 1 = \mathbf{1}$ |
| **2** | `1` (Node 4) | `0` (null) | $|1 - 0| = 1$ | Yes ($\le 1$) | $\max(1,0) + 1 = \mathbf{2}$ |
| **3** | `0` (null) | `0` (null) | $|0 - 0| = 0$ | Yes ($\le 1$) | $\max(0,0) + 1 = \mathbf{1}$ |
| **1** | `2` (Node 2) | `1` (Node 3) | $|2 - 1| = 1$ | Yes ($\le 1$) | $\max(2,1) + 1 = \mathbf{3}$ |

*Result*: Root returns `3 != -1` $\implies \mathbf{true}$.

### Spot-It-Fast Rule
Look for the return values in `checkHeight`: if `left == -1` returns `0`, or `Math.abs` checks `>= 1`, or `return Math.max(left, right) + 1` is missing, fix them immediately.

### Edge Cases
- Empty tree (`root == null`): Returns `true` (height `0 != -1`).
- Skewed tree ($1 \to 2 \to 3$): Detects imbalance at root (left = 2, right = 0, $|2-0| = 2 > 1$) and returns `false`.

### Time & Space Complexity
- **Time**: $O(N)$ — Each node visited once with bottom-up early exit.
- **Space**: $O(H)$ — Recursion call stack bounded by tree height $H$ ($O(\log N)$ balanced, $O(N)$ skewed).

---

## Problem 2: Binary Tree Level Order Traversal (BFS)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: binary-tree-level-order-traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Problem Statement
Given the `root` of a binary tree, return the level order traversal of its nodes' values (i.e., from left to right, level by level).

### Sample Input & Output
- **Input**: `root = [3, 9, 20, null, null, 15, 7]`
- **Output**: `[[3], [9, 20], [15, 7]]`

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root); // BUG 1: Pushes null into queue if root is null, leading to NullPointerException

    while (!q.isEmpty()) {
        List<Integer> level = new ArrayList<>();

        // BUG 2: Dynamic q.size() in loop header changes as children are enqueued
        for (int i = 0; i < q.size(); i++) {
            TreeNode node = q.poll();
            level.add(node.val);

            // BUG 3: Enqueues null children unconditionally
            q.add(node.left);
            q.add(node.right);
        }
        res.add(level);
    }
    return res;
}
```

### Bugs Found & Why They Are Wrong
1. **Unchecked `q.add(root)` on null root**:
   - *Why wrong*: If `root == null`, the queue contains `[null]`. Calling `q.poll().val` throws `NullPointerException`.
2. **Dynamic `q.size()` in loop condition**:
   - *Why wrong*: As children are added via `q.add()`, `q.size()` grows dynamically during the for-loop, mixing levels together.
3. **Adding null children (`q.add(node.left)`)**:
   - *Why wrong*: Pushing `null` without checking `!= null` fills the queue with null references, causing crashes on the next level.

### Fixed Production Code

#### Java
```java
import java.util.*;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res; // FIX 1: Guard clause

        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);

        while (!q.isEmpty()) {
            int levelSize = q.size(); // FIX 2: Freeze level size
            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode node = q.poll();
                level.add(node.val);

                // FIX 3: Enqueue only non-null children
                if (node.left != null) q.add(node.left);
                if (node.right != null) q.add(node.right);
            }
            res.add(level);
        }
        return res;
    }
}
```

#### C++
```cpp
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            vector<int> level;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* node = q.front();
                q.pop();
                level.push_back(node->val);

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            res.push_back(level);
        }
        return res;
    }
};
```

### Dry Run
Tree: `root = [3, 9, 20, null, null, 15, 7]`

| While Iteration | `q.size()` at Start | Nodes Polled | Level Values | Nodes Added to Queue | Queue State at Level End |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **Level 0** | 1 | `[3]` | `[3]` | `9, 20` | `[9, 20]` |
| **Level 1** | 2 | `[9, 20]` | `[9, 20]` | `15, 7` | `[15, 7]` |
| **Level 2** | 2 | `[15, 7]` | `[15, 7]` | None | `[]` (Empty $\to$ Exit) |

*Output*: `[[3], [9, 20], [15, 7]]`.

### Spot-It-Fast Rule
Look at the BFS loop header: if you see `for (int i = 0; i < q.size(); i++)` without `int size = q.size();` preceding it, that is Bug #1. Check for `if (node.left != null)` before enqueuing.

### Edge Cases
- `root == null`: Guard returns `[]` immediately without throwing NPE.
- Single-node tree: Outputs `[[val]]`.

### Time & Space Complexity
- **Time**: $O(N)$ — Every node is enqueued and dequeued exactly once.
- **Space**: $O(N)$ — In the worst case (complete binary tree), the queue holds up to $N/2$ leaf nodes.

---

## Problem 3: Zigzag Level Order Traversal (BFS with Alternating Reversal)

**Tag**: [ADDED]  
**Practice Link**: [LeetCode: binary-tree-zigzag-level-order-traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

### Problem Statement
Given the `root` of a binary tree, return the zigzag level order traversal of its nodes' values (i.e., from left to right, then right to left for the next level and alternate between).

### Sample Input & Output
- **Input**: `root = [3, 9, 20, null, null, 15, 7]`
- **Output**: `[[3], [20, 9], [15, 7]]`

### Common Exam Bugs
1. **Forgetting to flip boolean flag (`leftToRight = !leftToRight;`)**:
   - Traversal stays strictly left-to-right on all levels.
2. **Reversing the wrong list or double reversing**:
   - Reversing `level` unconditionally or re-reversing in the result builder.
3. **Null children pushed to deque/queue**:
   - Dereferencing null node pointers during alternating collection.

### Fixed Production Code

#### Java
```java
import java.util.*;

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        boolean leftToRight = true;

        while (!q.isEmpty()) {
            int levelSize = q.size();
            List<Integer> level = new ArrayList<>(levelSize);

            for (int i = 0; i < levelSize; i++) {
                TreeNode node = q.poll();

                if (leftToRight) {
                    level.add(node.val);
                } else {
                    level.add(0, node.val); // Add to head for right-to-left
                }

                if (node.left != null) q.add(node.left);
                if (node.right != null) q.add(node.right);
            }

            res.add(level);
            leftToRight = !leftToRight; // FIX: Flip direction for next level
        }
        return res;
    }
}
```

#### C++
```cpp
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;

        queue<TreeNode*> q;
        q.push(root);
        bool leftToRight = true;

        while (!q.empty()) {
            int levelSize = q.size();
            vector<int> level(levelSize);

            for (int i = 0; i < levelSize; i++) {
                TreeNode* node = q.front();
                q.pop();

                // Compute destination index based on direction
                int index = leftToRight ? i : (levelSize - 1 - i);
                level[index] = node->val;

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }

            res.push_back(level);
            leftToRight = !leftToRight; // Flip flag
        }
        return res;
    }
};
```

### Dry Run
Tree: `root = [3, 9, 20, null, null, 15, 7]`

| Level | `leftToRight` | Level Size | Insertion Order | Level Output | Next `leftToRight` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | `true` | 1 | Append 3 $\to$ `[3]` | `[3]` | `false` |
| 1 | `false` | 2 | Add 9 to head, add 20 to head $\to$ `[20, 9]` | `[20, 9]` | `true` |
| 2 | `true` | 2 | Append 15, append 7 $\to$ `[15, 7]` | `[15, 7]` | `false` |

*Output*: `[[3], [20, 9], [15, 7]]`.

### Spot-It-Fast Rule
Look for `leftToRight = !leftToRight;` at the end of the while loop. If missing, all levels traverse in the same direction.

### Edge Cases
- Skewed trees: Continues alternating depth levels correctly.
- Single node: Outputs `[[val]]`.

### Time & Space Complexity
- **Time**: $O(N)$ — Every node is visited once.
- **Space**: $O(N)$ — Queue holds at most $N/2$ nodes.

---

## Problem 4: Validate Binary Search Tree (Boundary Pointer Bug)

**Tag**: [MOCK-EXAM]  
**Practice Link**: [LeetCode: validate-binary-search-tree](https://leetcode.com/problems/validate-binary-search-tree/)

### Problem Statement
Given the root of a binary tree, determine if it is a valid binary search tree (BST).  
A valid BST requires that:
1. The left subtree of a node contains only nodes with keys strictly less than the node's key.
2. The right subtree of a node contains only nodes with keys strictly greater than the node's key.
3. Both the left and right subtrees must also be binary search trees.

### Buggy Exam Code
```cpp
// BUGGY: Local child check fails to enforce global ancestor bounds
bool isValidBST(TreeNode* root) {
    if (!root) return true;
    if (root->left && root->left->val >= root->val) return false;
    if (root->right && root->right->val <= root->val) return false;
    return isValidBST(root->left) && isValidBST(root->right);
}
```

### Bugs Found & Why They Are Wrong
1. **Immediate Child Check Only (Missing Global Range Propagation)**:
   - *Why wrong*: The buggy code only checks whether a node's direct children satisfy the BST condition. It fails when a deeply nested descendant violates an ancestor's bound.
   - *Counter-Example*: Tree `root = [5, 4, 6, null, null, 3, 7]`. Here, node `6` has left child `3` and right child `7`. Locally at node `6`, $3 < 6$ and $7 > 6$ holds. However, node `3` is in the *right subtree* of `5`, which violates the global requirement that all nodes in the right subtree of `5` must be $> 5$. The buggy code incorrectly returns `true` instead of `false`.

### Fixed Production Code

#### C++
```cpp
class Solution {
public:
    bool validate(TreeNode* node, long long minVal, long long maxVal) {
        if (!node) return true;
        // Strictly between minVal and maxVal
        if (node->val <= minVal || node->val >= maxVal) return false;
        return validate(node->left, minVal, node->val) && 
               validate(node->right, node->val, maxVal);
    }

    bool isValidBST(TreeNode* root) {
        return validate(root, LLONG_MIN, LLONG_MAX);
    }
};
```

#### Java
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean validate(TreeNode node, long minVal, long maxVal) {
        if (node == null) return true;
        if (node.val <= minVal || node.val >= maxVal) return false;
        return validate(node.left, minVal, node.val) && 
               validate(node.right, node.val, maxVal);
    }
}
```

### Time & Space Complexity
- **Time**: $O(N)$ — Every node is visited once during the range validation.
- **Space**: $O(H)$ — Recursion stack bounded by tree height $H$ ($O(\log N)$ balanced, $O(N)$ skewed).

