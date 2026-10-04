[Home](../README.md) > [04-code-debugging](README.md) > 01_trees.md

# 01. Tree Algorithms Debugging

This module covers common logical, structural, and traversal bugs planted in binary tree problems in technical assessments.

## Problem 1 (DBG-001): Height-Balanced Binary Tree ($O(N)$ DFS)
**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Binary Trees
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=YEZy2e_PARE)

### Problem Statement
A height-balanced binary tree is defined as a binary tree in which the left and right subtrees of every node differ in height by no more than 1. Implement a function to determine if a given binary tree is height-balanced in $O(N)$ time.

### Sample Input & Output
- **Input**: `root = [3, 9, 20, null, null, 15, 7]`
- **Output**: `true`
- **Input**: `root = [1, 2, 2, 3, 3, null, null, 4, 4]`
- **Output**: `false`

### Buggy Exam Code
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != 0;
    }
    
    private int checkHeight(TreeNode root) {
        if (root == null) return 0;
        
        int left = checkHeight(root.left);
        int right = checkHeight(root.right);
        
        if (Math.abs(left - right) >= 1) {
            return -1;
        }
        
        return Math.max(left, right);
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Wrong Balanced Condition (`>= 1` vs `> 1`)**: Height difference $\le 1$ is valid in balanced trees. Flagging difference $\ge 1$ incorrectly rejects balanced trees whose heights differ by 1.
2. **Missing Height Increment (`+ 1`)**: Returning `Math.max(left, right)` omits counting the current node, causing height to remain 0 across the entire tree.
3. **Missing Error Propagation**: When a subtree returns `-1` (unbalanced), the caller continues computing heights without immediate bubble-up.
4. **Invalid Return Check (`!= 0` vs `!= -1`)**: An empty tree has height 0 and is balanced; checking `!= 0` treats empty trees as unbalanced.

### Fixed Code
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return checkHeight(root) != -1;
    }
    
    private int checkHeight(TreeNode root) {
        if (root == null) return 0;
        
        int left = checkHeight(root.left);
        if (left == -1) return -1;
        
        int right = checkHeight(root.right);
        if (right == -1) return -1;
        
        if (Math.abs(left - right) > 1) return -1;
        
        return 1 + Math.max(left, right);
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return checkHeight(root) != -1;
    }
private:
    int checkHeight(TreeNode* root) {
        if (!root) return 0;
        int left = checkHeight(root->left);
        if (left == -1) return -1;
        int right = checkHeight(root->right);
        if (right == -1) return -1;
        if (std::abs(left - right) > 1) return -1;
        return 1 + std::max(left, right);
    }
};
```
</details>

### Dry-Run Table
| Node Visited | Left Height | Right Height | Abs Diff | Return Value |
| :--- | :---: | :---: | :---: | :---: |
| Leaf `9` | 0 | 0 | 0 | 1 |
| Leaf `15` | 0 | 0 | 0 | 1 |
| Leaf `7` | 0 | 0 | 0 | 1 |
| Subtree `20` | 1 | 1 | 0 | 2 |
| Root `3` | 1 | 2 | 1 | 3 (Balanced) |

### Spot-It-Fast Rule
If a tree function uses `-1` as an error indicator, verify `-1` is immediately propagated before computing `Math.max()`.

### Edge Cases
1. `root == null`: Empty tree is balanced (returns `true`).
2. Single-node tree `[1]`: Both subtrees are null (heights 0, difference 0 -> returns `true`).
3. Skewed line `[1, 2, null, 3]`: Left height 2, right height 0 (difference 2 > 1 -> returns `false`).

- **Complexity**: Time: $O(N)$, Space: $O(H)$ where $H$ is tree height.

---

## Problem 2 (DBG-002): Binary Tree Level Order Traversal (BFS)
**Tag**: [VIDEO] | **Difficulty**: Easy | **Topic**: Binary Trees
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=YEZy2e_PARE)

### Problem Statement
Given the `root` of a binary tree, return the level order traversal of its nodes' values (from left to right, level by level).

### Sample Input & Output
- **Input**: `root = [3, 9, 20, null, null, 15, 7]`
- **Output**: `[[3], [9, 20], [15, 7]]`

### Buggy Exam Code
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        
        while (!q.isEmpty()) {
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < q.size(); i++) {
                TreeNode curr = q.poll();
                level.add(curr.val);
                if (curr.left != null) q.add(curr.left);
                if (curr.right != null) q.add(curr.right);
            }
            res.add(level);
        }
        return res;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Unchecked Null Root Enqueue**: If `root == null`, `q.add(root)` inserts a null pointer. In the loop, `curr.val` throws a `NullPointerException`.
2. **Dynamic Queue Size in Loop Header**: In `for (int i = 0; i < q.size(); i++)`, `q.size()` changes whenever `q.poll()` or `q.add()` is called, causing premature inner loop termination and level fragmentation.

### Fixed Code
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        
        while (!q.isEmpty()) {
            int levelSize = q.size();
            List<Integer> level = new ArrayList<>(levelSize);
            for (int i = 0; i < levelSize; i++) {
                TreeNode curr = q.poll();
                level.add(curr.val);
                if (curr.left != null) q.add(curr.left);
                if (curr.right != null) q.add(curr.right);
            }
            res.add(level);
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
    std::vector<std::vector<int>> levelOrder(TreeNode* root) {
        std::vector<std::vector<int>> res;
        if (!root) return res;
        std::queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            int levelSize = q.size();
            std::vector<int> level;
            for (int i = 0; i < levelSize; i++) {
                TreeNode* curr = q.front();
                q.pop();
                level.push_back(curr->val);
                if (curr->left) q.push(curr->left);
                if (curr->right) q.push(curr->right);
            }
            res.push_back(level);
        }
        return res;
    }
};
```
</details>

### Dry-Run Table
| Iteration | Initial Queue State | `levelSize` Snapshot | Processed Elements | Result List |
| :--- | :--- | :---: | :--- | :--- |
| Level 0 | `[3]` | 1 | `3` | `[[3]]` |
| Level 1 | `[9, 20]` | 2 | `9, 20` | `[[3], [9, 20]]` |
| Level 2 | `[15, 7]` | 2 | `15, 7` | `[[3], [9, 20], [15, 7]]` |

### Spot-It-Fast Rule
Always cache `int sz = q.size();` before the inner level loop. Never evaluate `q.size()` in the `for` loop header.

### Edge Cases
1. `root == null`: Must return empty list `[]` without throwing NPE.
2. Single-node tree: Returns `[[val]]`.
3. Left-skewed tree: Every level has exactly 1 element.

- **Complexity**: Time: $O(N)$, Space: $O(N)$.

---

## Problem 3 (DBG-003): Zigzag Level Order Traversal
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Trees
**Source**: Added practice

### Problem Statement
Given the `root` of a binary tree, return the zigzag level order traversal of its nodes' values (alternating left-to-right then right-to-left).

### Sample Input & Output
- **Input**: `root = [3, 9, 20, null, null, 15, 7]`
- **Output**: `[[3], [20, 9], [15, 7]]`

### Buggy Exam Code
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        boolean leftToRight = true;
        
        while (!q.isEmpty()) {
            int sz = q.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < sz; i++) {
                TreeNode curr = q.poll();
                if (leftToRight) {
                    level.add(curr.val);
                } else {
                    level.add(0, curr.val);
                }
                if (curr.left != null) q.add(curr.left);
                if (curr.right != null) q.add(curr.right);
            }
            res.add(level);
        }
        return res;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Flag Never Toggled**: `leftToRight = true` is never toggled (`leftToRight = !leftToRight;`), resulting in standard level order rather than alternating zigzag.

### Fixed Code
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        boolean leftToRight = true;
        
        while (!q.isEmpty()) {
            int sz = q.size();
            LinkedList<Integer> level = new LinkedList<>();
            for (int i = 0; i < sz; i++) {
                TreeNode curr = q.poll();
                if (leftToRight) {
                    level.addLast(curr.val);
                } else {
                    level.addFirst(curr.val);
                }
                if (curr.left != null) q.add(curr.left);
                if (curr.right != null) q.add(curr.right);
            }
            res.add(level);
            leftToRight = !leftToRight;
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
    std::vector<std::vector<int>> zigzagLevelOrder(TreeNode* root) {
        std::vector<std::vector<int>> res;
        if (!root) return res;
        std::queue<TreeNode*> q;
        q.push(root);
        bool leftToRight = true;
        while (!q.empty()) {
            int sz = q.size();
            std::vector<int> level(sz);
            for (int i = 0; i < sz; i++) {
                TreeNode* curr = q.front();
                q.pop();
                int idx = leftToRight ? i : (sz - 1 - i);
                level[idx] = curr->val;
                if (curr->left) q.push(curr->left);
                if (curr->right) q.push(curr->right);
            }
            res.push_back(level);
            leftToRight = !leftToRight;
        }
        return res;
    }
};
```
</details>

### Dry-Run Table
| Level | `leftToRight` Flag | Traversed Nodes | Level Array Output |
| :---: | :---: | :--- | :--- |
| 0 | `true` | 3 | `[3]` |
| 1 | `false` | 9, 20 | `[20, 9]` |
| 2 | `true` | 15, 7 | `[15, 7]` |

### Spot-It-Fast Rule
Whenever an alternating boolean flag is declared before a `while` loop, verify it is negated (`flag = !flag`) at the end of each iteration.

### Edge Cases
1. Empty tree: Returns `[]`.
2. Single-node tree: Returns `[[val]]`.
3. Skewed binary tree: Alternates direction every step.

- **Complexity**: Time: $O(N)$, Space: $O(N)$.

---

## Problem 4 (DBG-004): Validate Binary Search Tree
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Trees
**Source**: Added practice

### Problem Statement
Given the `root` of a binary tree, determine if it is a valid binary search tree (BST). A valid BST satisfies: left subtree values are strictly less than node value, and right subtree values are strictly greater.

### Sample Input & Output
- **Input**: `root = [2, 1, 3]` -> `true`
- **Input**: `root = [5, 1, 4, null, null, 3, 6]` -> `false`

### Buggy Exam Code
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        if (root.left != null && root.left.val >= root.val) return false;
        if (root.right != null && root.right.val <= root.val) return false;
        return isValidBST(root.left) && isValidBST(root.right);
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Local Only Check**: Only checks direct parent-child relationships. Subtree nodes can violate ancestor bounds (e.g., node 3 in the right subtree of 5 with parent 4).
2. **Integer Overflow on Limits**: Using `Integer.MIN_VALUE` or `Integer.MAX_VALUE` as int bounds fails when node values equal `Integer.MIN_VALUE`.

### Fixed Code
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return validate(root, null, null);
    }
    
    private boolean validate(TreeNode node, Integer low, Integer high) {
        if (node == null) return true;
        if (low != null && node.val <= low) return false;
        if (high != null && node.val >= high) return false;
        return validate(node.left, low, node.val) && validate(node.right, node.val, high);
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return validate(root, nullptr, nullptr);
    }
private:
    bool validate(TreeNode* node, long long* low, long long* high) {
        if (!node) return true;
        if (low && node->val <= *low) return false;
        if (high && node->val >= *high) return false;
        long long val = node->val;
        return validate(node->left, low, &val) && validate(node->right, &val, high);
    }
};
```
</details>

### Dry-Run Table
| Node | Valid Range `(low, high)` | Condition Check | Result |
| :---: | :---: | :---: | :---: |
| 5 (root) | $(-\infty, +\infty)$ | Valid | Proceed |
| 1 (left) | $(-\infty, 5)$ | Valid | Proceed |
| 4 (right) | $(5, +\infty)$ | `4 <= 5` FAILS | Return `false` |

### Spot-It-Fast Rule
Never validate a BST by only checking `node.left.val < node.val`. A valid check must pass ancestor `(low, high)` bounds down the recursion.

### Edge Cases
1. `root.val == Integer.MAX_VALUE` or `Integer.MIN_VALUE`: Handled cleanly with `null` boundary wrappers.
2. Duplicate values in tree: `node.val <= low` or `>= high` correctly catches duplicates (BST strictly requires `<` and `>`).

- **Complexity**: Time: $O(N)$, Space: $O(H)$.

---

## Problem 5 (DBG-023): Diameter of Binary Tree
**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Binary Trees
**Source**: Added practice

### Problem Statement
The diameter of a binary tree is the length of the longest path between any two nodes. This path may or may not pass through the root. Length is measured in number of edges.

### Sample Input & Output
- **Input**: `root = [1, 2, 3, 4, 5]`
- **Output**: `3` (path `[4, 2, 1, 3]` or `[5, 2, 1, 3]`)

### Buggy Exam Code
```java
class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
        return leftH + rightH;
    }
    
    private int getHeight(TreeNode node) {
        if (node == null) return 0;
        return 1 + Math.max(getHeight(node.left), getHeight(node.right));
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Assuming Path Passes Through Root**: Longest path could lie entirely within the left or right subtree.
2. **Quadratic Complexity ($O(N^2)$)**: Calling `getHeight` repeatedly causes redundant subtree traversals.

### Fixed Code
```java
class Solution {
    private int maxDiameter = 0;
    
    public int diameterOfBinaryTree(TreeNode root) {
        maxDiameter = 0;
        maxDepth(root);
        return maxDiameter;
    }
    
    private int maxDepth(TreeNode node) {
        if (node == null) return 0;
        int left = maxDepth(node.left);
        int right = maxDepth(node.right);
        maxDiameter = Math.max(maxDiameter, left + right);
        return 1 + Math.max(left, right);
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
    int maxDiameter = 0;
public:
    int diameterOfBinaryTree(TreeNode* root) {
        maxDiameter = 0;
        maxDepth(root);
        return maxDiameter;
    }
private:
    int maxDepth(TreeNode* node) {
        if (!node) return 0;
        int left = maxDepth(node->left);
        int right = maxDepth(node->right);
        maxDiameter = std::max(maxDiameter, left + right);
        return 1 + std::max(left, right);
    }
};
```
</details>

### Dry-Run Table
| Node | Left Depth | Right Depth | Local Diameter (`L + R`) | Global Max Diameter |
| :---: | :---: | :---: | :---: | :---: |
| 4 | 0 | 0 | 0 | 0 |
| 5 | 0 | 0 | 0 | 0 |
| 2 | 1 | 1 | 2 | 2 |
| 3 | 0 | 0 | 0 | 2 |
| 1 | 2 | 1 | 3 | 3 |

### Spot-It-Fast Rule
Diameter is tracked globally inside a single bottom-up depth traversal in $O(N)$ time.

### Edge Cases
1. Single node: Both children null, depths 0 -> returns `0`.
2. Fully skewed tree: Returns $N - 1$.

- **Complexity**: Time: $O(N)$, Space: $O(H)$.

---

## Problem 6 (DBG-024): Lowest Common Ancestor (LCA) in Binary Tree
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Trees
**Source**: Added practice

### Problem Statement
Given a binary tree and two nodes `p` and `q`, find their Lowest Common Ancestor (LCA). The LCA is the lowest node that has both `p` and `q` as descendants.

### Sample Input & Output
- **Input**: `root = [3, 5, 1, 6, 2, 0, 8]`, `p = 5`, `q = 1`
- **Output**: `3`

### Buggy Exam Code
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (root == p && root == q) return root;
        
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        if (left != null) return left;
        return right;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Invalid Target Match (`&&` instead of `||`)**: `root == p && root == q` is impossible since $p \neq q$. Base check must be `root == p || root == q`.
2. **Missing Convergence Point Check**: When both `left != null` and `right != null`, the current node is the LCA, but buggy code immediately returns `left`.

### Fixed Code
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        if (left != null && right != null) return root;
        return left != null ? left : right;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left && right) return root;
        return left ? left : right;
    }
};
```
</details>

### Dry-Run Table
| Node Visited | Left Return | Right Return | Evaluation |
| :---: | :---: | :---: | :---: |
| 5 (`root == p`) | - | - | Returns node 5 |
| 1 (`root == q`) | - | - | Returns node 1 |
| 3 (Root) | Node 5 | Node 1 | Both non-null -> Returns Root 3 |

### Spot-It-Fast Rule
LCA condition is: `if (left != null && right != null) return root;`. If only one is non-null, bubble that non-null node up.

### Edge Cases
1. One target is ancestor of the other (e.g., `p = 5`, `q = 4` where 4 is inside 5's subtree): Returns `p` immediately.
2. Targets at different depths in separate subtrees: Returns their common ancestor.

- **Complexity**: Time: $O(N)$, Space: $O(H)$.

---

Previous: [README.md](README.md) | Next: [02_matrix.md](02_matrix.md)
