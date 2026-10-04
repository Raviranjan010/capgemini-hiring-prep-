[Home](../README.md) > [09-mock-tests](README.md) > 04_coding_mock.md

# Full-Length Coding Mock Assessment

**Time Limit**: 65 minutes total (20 mins for Part 1 Debugging, 45 mins for Part 2 AI-Assisted)
**Structure**: 3 Debugging Problems + 2 AI-Assisted Problems

---

## Part 1: Code Debugging (20 Minutes, 3 Problems)

### Problem 1 (CODEMOCK-001): Invert Binary Tree
**Difficulty**: Easy | **Topic**: Binary Trees

#### Problem Statement
Given the `root` of a binary tree, invert the tree (mirror left and right subtrees recursively) and return its root.

#### Buggy Exam Code
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        // Bug: Missing base case null check!
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
        return root;
    }
}
```

---

### Problem 2 (CODEMOCK-002): Search a 2D Matrix II
**Difficulty**: Medium | **Topic**: 2D Matrix Search

#### Problem Statement
Write an efficient algorithm that searches for a `target` integer in an $M \times N$ integer matrix sorted in ascending order along rows and columns.

#### Buggy Exam Code
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int r = 0, c = matrix[0].length; // Bug 1: out-of-bounds start
        while (r < matrix.length || c >= 0) { // Bug 2: || instead of &&
            if (matrix[r][c] == target) return true;
            else if (matrix[r][c] < target) c--; // Bug 3: wrong direction
            else r++;
        }
        return false;
    }
}
```

---

### Problem 3 (CODEMOCK-003): Maximum Product Subarray
**Difficulty**: Medium | **Topic**: Dynamic Programming

#### Problem Statement
Given an integer array `nums`, find a subarray that has the largest product, and return the product.

#### Buggy Exam Code
```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxProd = nums[0], curProd = nums[0];
        for (int i = 1; i < nums.length; i++) {
            // Bug: Fails when multiplying two negative numbers; forgets to track minProd!
            curProd = Math.max(nums[i], curProd * nums[i]);
            maxProd = Math.max(maxProd, curProd);
        }
        return maxProd;
    }
}
```

---

## Part 2: AI-Assisted Coding (45 Minutes, 2 Problems)

### Problem 4 (CODEMOCK-004): Longest Substring Without Repeating Characters
**Difficulty**: Medium | **Topic**: Sliding Window

#### Problem Statement
Given a string `s`, find the length of the longest substring without repeating characters in $O(N)$ time.

#### Bot Trap
The AI bot's first prompt solution increments `left = left + 1` one step at a time inside a `while` loop rather than jumping `left = Math.max(left, lastSeen.get(c) + 1)`, consuming unnecessary operations and causing TLE on long repeated strings.

---

### Problem 5 (CODEMOCK-005): Number of Provinces (Connected Components)
**Difficulty**: Medium | **Topic**: Graph Traversal

#### Problem Statement
There are $n$ cities. You are given an $n \times n$ matrix `isConnected` where `isConnected[i][j] = 1` if city $i$ and city $j$ are directly connected. Return the total number of provinces (connected components).

#### Bot Trap
The bot attempts DFS but fails to mark `visited[u] = true` when visiting self-loop `isConnected[i][i] == 1`, or writes bidirectional recursion that hits stack overflow because of missing visited checks before recursion.

---

Previous: [03_mcq_mock_3_solutions.md](03_mcq_mock_3_solutions.md) | Next: [04_coding_mock_solutions.md](04_coding_mock_solutions.md)
