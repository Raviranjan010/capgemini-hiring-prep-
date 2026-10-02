# Tree Traversal & Balancing Debugging

---

## Problem 1: Height-Balanced Binary Tree ($O(N)$ DFS)

**Tag**: [CHAT]  
**Video Reference**: Height-Balanced Tree Debugging (unverified link, see [RESOURCES.md](../RESOURCES.md#video-references))  
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

        // BUG 1: Returning 0 masks unbalanced subtree failure
        if (left == -1 || right == -1) return 0;

        // BUG 2: >= 1 flags valid trees with height difference of 1 as invalid
        if (Math.abs(left - right) >= 1) return -1;

        // BUG 3: Missing height calculation return statement
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Returning `0` on subtree failure (`left == -1 || right == -1 return 0;`)**:
   - *Why wrong*: `-1` is the failure sentinel meaning a descendant subtree is unbalanced. Returning `0` masks the failure and falsely reports a valid height of 0 to parent nodes.
2. **Checking `>= 1` instead of `> 1` (`Math.abs(left - right) >= 1`)**:
   - *Why wrong*: A height difference of exactly 1 is valid in a balanced binary tree. Only a difference strictly greater than 1 (`> 1`) violates balance.
3. **Missing Return Statement**:
   - *Why wrong*: After verifying the node is balanced, the function must return its true height: `1 + Math.max(left, right)`. Failing to return this causes compilation failure.

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
Look for the return values in `checkHeight`: if `left == -1` returns `0`, or `Math.abs` checks `>= 1`, or `return Math.max(...) + 1` is missing, fix them immediately.

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
