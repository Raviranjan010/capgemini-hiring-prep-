[Home](../README.md) > [05-ai-assisted-coding](README.md) > 07_binary_tree_boundary_traversal.md

# High-Yield Topic: Binary Tree Boundary Traversal

**Tag**: [AI-ASSIST-CODING] [TREE] [TRAVERSAL] [VIDEO]  
**Practice Link**: [LeetCode 545: Boundary of Binary Tree](https://leetcode.com/problems/boundary-of-binary-tree/) / [GFG: Tree Boundary Traversal](https://www.geeksforgeeks.org/problems/boundary-traversal-of-binary-tree/1)  
**Video Reference**: KN Academy Capgemini Technical Assessment 28 Sept Video

---

## Problem 1 (AIC-031): Binary Tree Boundary Traversal

Given the `root` of a binary tree, return the values of its boundary nodes in **anti-clockwise** direction starting from the root.

The boundary consists of three distinct parts in sequence:
1. **Left Boundary**: Nodes from the root down to the leftmost non-leaf node (top-to-bottom, excluding leaves).
2. **Leaf Nodes**: All leaf nodes of the tree traversed from left to right.
3. **Right Boundary**: Nodes from the rightmost non-leaf node up to the root (bottom-to-top, in reverse order, excluding leaves).

### Architectural Traversal Diagram

```text
                     BOUNDARY TRAVERSAL ORDER
                                (1) Root
                               /   \
          Left Boundary      (2)     (3)     Right Boundary
          (Top-to-Bottom)   /   \       \    (Bottom-to-Top)
                          (4)   (5)     (6)
                                 │       │
                                 └──┬────┘
                                    ▼
                         Leaves (Left-to-Right)
```

---

## 2. Step-by-Step Traversal Protocol

1. **Root Handling**: If the root is not a leaf node, add its value to the result list.
2. **Phase 1: Left Boundary (Top-to-Bottom)**:
   - Start from `root.left`.
   - While `curr != null`:
     - If `curr` is not a leaf, add `curr.val` to result.
     - Move to `curr.left` if it exists; otherwise move to `curr.right`.
3. **Phase 2: Leaf Nodes (Left-to-Right)**:
   - Perform a standard pre-order or in-order depth-first traversal starting from `root`.
   - If a node has `node.left == null && node.right == null`, add `node.val` to result.
4. **Phase 3: Right Boundary (Bottom-to-Top)**:
   - Start from `root.right`.
   - While `curr != null`:
     - If `curr` is not a leaf, collect `curr.val` into a temporary stack/list.
     - Move to `curr.right` if it exists; otherwise move to `curr.left`.
   - Pop/traverse the temporary list in **reverse order** and append to the result list.

---

## 3. Production Implementations

### Java Solution
```java
import java.util.*;

class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int v) { val = v; }
}

public class TreeBoundaryTraversal {

    private static boolean isLeaf(TreeNode node) {
        return node != null && node.left == null && node.right == null;
    }

    private static void addLeftBoundary(TreeNode root, List<Integer> res) {
        TreeNode curr = root.left;
        while (curr != null) {
            if (!isLeaf(curr)) {
                res.add(curr.val);
            }
            // Prefer left child; if absent, follow right child
            curr = (curr.left != null) ? curr.left : curr.right;
        }
    }

    private static void addLeaves(TreeNode root, List<Integer> res) {
        if (root == null) return;
        if (isLeaf(root)) {
            res.add(root.val);
            return;
        }
        if (root.left != null) addLeaves(root.left, res);
        if (root.right != null) addLeaves(root.right, res);
    }

    private static void addRightBoundary(TreeNode root, List<Integer> res) {
        TreeNode curr = root.right;
        List<Integer> temp = new ArrayList<>();
        while (curr != null) {
            if (!isLeaf(curr)) {
                temp.add(curr.val);
            }
            // Prefer right child; if absent, follow left child
            curr = (curr.right != null) ? curr.right : curr.left;
        }
        // Reverse order (bottom to top)
        for (int i = temp.size() - 1; i >= 0; i--) {
            res.add(temp.get(i));
        }
    }

    public static List<Integer> boundaryTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        // Step 1: Add root if it is not a leaf node
        if (!isLeaf(root)) {
            res.add(root.val);
        }

        // Step 2: Add left boundary
        addLeftBoundary(root, res);

        // Step 3: Add all leaves (left-to-right)
        addLeaves(root, res);

        // Step 4: Add right boundary (bottom-to-top)
        addRightBoundary(root, res);

        return res;
    }

    public static void main(String[] args) {
        // Construct sample tree:
        //        1
        //       / \
        //      2   3
        //     / \   \
        //    4   5   6
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);

        List<Integer> boundary = boundaryTraversal(root);
        System.out.println(boundary); // Output: [1, 2, 4, 5, 6, 3]
    }
}
```

### C++ Solution
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

bool isLeaf(TreeNode* node) {
    return node && !node->left && !node->right;
}

void addLeftBoundary(TreeNode* root, vector<int>& res) {
    TreeNode* curr = root->left;
    while (curr) {
        if (!isLeaf(curr)) res.push_back(curr->val);
        curr = curr->left ? curr->left : curr->right;
    }
}

void addLeaves(TreeNode* root, vector<int>& res) {
    if (!root) return;
    if (isLeaf(root)) {
        res.push_back(root->val);
        return;
    }
    if (root->left) addLeaves(root->left, res);
    if (root->right) addLeaves(root->right, res);
}

void addRightBoundary(TreeNode* root, vector<int>& res) {
    TreeNode* curr = root->right;
    vector<int> temp;
    while (curr) {
        if (!isLeaf(curr)) temp.push_back(curr->val);
        curr = curr->right ? curr->right : curr->left;
    }
    // Reverse order
    for (int i = (int)temp.size() - 1; i >= 0; i--) {
        res.push_back(temp[i]);
    }
}

vector<int> boundaryTraversal(TreeNode* root) {
    vector<int> res;
    if (!root) return res;

    if (!isLeaf(root)) res.push_back(root->val);

    addLeftBoundary(root, res);
    addLeaves(root, res);
    addRightBoundary(root, res);

    return res;
}
```

---

## 4. Complexity & Constraints

| Metric | Complexity | Explanation |
| :--- | :---: | :--- |
| **Time Complexity** | $O(N)$ | Each node is visited at most once across the three boundary/leaf phases. |
| **Space Complexity** | $O(H)$ | Recursion stack for leaf traversal takes $O(H)$ where $H$ is tree height; temporary list for right boundary holds $O(H)$ nodes. |

---

## 5. Common AI Assist Traps on Boundary Traversal

1. **Duplicate Leaves**: If you include leaf nodes while scanning the left or right boundary, they will be printed twice (once during the boundary pass and once during the leaf pass). Always check `!isLeaf(curr)`.
2. **Root Duplication**: If the root is a leaf (`root.left == null && root.right == null`), do NOT add it in both the root initialization and the leaf phase.
3. **Right Boundary Direction**: The right boundary must be printed from **bottom to top**. AI bots often output it from top to bottom; always verify that reverse iteration or stack popping is used.

---

Previous: [06_grid_bfs_shortest_path_obstacles.md](06_grid_bfs_shortest_path_obstacles.md) | Next: [../06-cognitive-and-behavioral/README.md](../06-cognitive-and-behavioral/README.md)
