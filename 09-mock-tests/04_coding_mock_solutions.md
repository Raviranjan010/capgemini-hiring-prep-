[Home](../README.md) > [09-mock-tests](README.md) > 04_coding_mock_solutions.md

# Coding Mock Assessment: Solutions & Explanations

---

## Problem 1 (CODEMOCK-001): Invert Binary Tree Solution

### Bugs Identified
1. **Missing Null Base Case**: Attempting `root.left` when `root == null` throws `NullPointerException` at leaf children.

### Fixed Code
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
        return root;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return nullptr;
        TreeNode* temp = root->left;
        root->left = invertTree(root->right);
        root->right = invertTree(temp);
        return root;
    }
};
```
</details>

- **Complexity**: Time: $O(N)$, Space: $O(H)$.

---

## Problem 2 (CODEMOCK-002): Search a 2D Matrix II Solution

### Bugs Identified
1. **Starting Col Out of Bounds**: Must be `matrix[0].length - 1`.
2. **Loop Condition**: Must use `r < matrix.length && c >= 0`.
3. **Inverted Navigation**: If `matrix[r][c] < target`, move down (`r++`), not `c--`.

### Fixed Code
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) return false;
        int r = 0, c = matrix[0].length - 1;
        while (r < matrix.length && c >= 0) {
            if (matrix[r][c] == target) return true;
            else if (matrix[r][c] < target) r++;
            else c--;
        }
        return false;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    bool searchMatrix(const std::vector<std::vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int r = 0, c = matrix[0].size() - 1;
        while (r < matrix.size() && c >= 0) {
            if (matrix[r][c] == target) return true;
            else if (matrix[r][c] < target) r++;
            else c--;
        }
        return false;
    }
};
```
</details>

- **Complexity**: Time: $O(M + N)$, Space: $O(1)$.

---

## Problem 3 (CODEMOCK-003): Maximum Product Subarray Solution

### Bugs Identified
1. **Missing Minimum Product Tracker**: Multiplying two negative numbers produces a large positive number. We must simultaneously track `curMax` and `curMin`.

### Fixed Code
```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxSoFar = nums[0];
        int curMax = nums[0], curMin = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int x = nums[i];
            if (x < 0) {
                int temp = curMax;
                curMax = curMin;
                curMin = temp;
            }
            curMax = Math.max(x, curMax * x);
            curMin = Math.min(x, curMin * x);
            maxSoFar = Math.max(maxSoFar, curMax);
        }
        return maxSoFar;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int maxProduct(const std::vector<int>& nums) {
        int maxSoFar = nums[0];
        int curMax = nums[0], curMin = nums[0];
        for (size_t i = 1; i < nums.size(); i++) {
            int x = nums[i];
            if (x < 0) std::swap(curMax, curMin);
            curMax = std::max(x, curMax * x);
            curMin = std::min(x, curMin * x);
            maxSoFar = std::max(maxSoFar, curMax);
        }
        return maxSoFar;
    }
};
```
</details>

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 4 (CODEMOCK-004): Longest Substring Without Repeating Characters Solution

### 3-Prompt Token Strategy
1. **Prompt 1**: "Implement longest substring without repeating characters using sliding window and an array of last seen indices for ASCII characters."
2. **Prompt 2 (Fix)**: "Ensure the left pointer jumps directly: `left = Math.max(left, lastSeen[c] + 1)` rather than using an inner while loop."
3. **Prompt 3**: "Add edge cases: empty string, string of all identical characters."

### Fixed Code
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] lastSeen = new int[128];
        Arrays.fill(lastSeen, -1);
        int maxLen = 0, left = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (lastSeen[c] >= left) {
                left = lastSeen[c] + 1;
            }
            lastSeen[c] = right;
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(const std::string& s) {
        std::vector<int> lastSeen(128, -1);
        int maxLen = 0, left = 0;
        for (int right = 0; right < (int)s.length(); right++) {
            char c = s[right];
            if (lastSeen[c] >= left) {
                left = lastSeen[c] + 1;
            }
            lastSeen[c] = right;
            maxLen = std::max(maxLen, right - left + 1);
        }
        return maxLen;
    }
};
```
</details>

- **Complexity**: Time: $O(N)$, Space: $O(1)$ (128 array).

---

## Problem 5 (CODEMOCK-005): Number of Provinces Solution

### 3-Prompt Token Strategy
1. **Prompt 1**: "Implement connected components on an $N \times N$ adjacency matrix `isConnected` using iterative BFS."
2. **Prompt 2 (Fix)**: "Ensure `visited[i] = true` is set immediately before adding to queue to avoid duplicate insertions."
3. **Prompt 3**: "Optimize to $O(N^2)$ time and $O(N)$ space."

### Fixed Code
```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int provinces = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                provinces++;
                bfs(i, isConnected, visited);
            }
        }
        return provinces;
    }
    
    private void bfs(int start, int[][] isConnected, boolean[] visited) {
        Queue<Integer> q = new LinkedList<>();
        visited[start] = true;
        q.add(start);
        
        while (!q.isEmpty()) {
            int u = q.poll();
            for (int v = 0; v < isConnected.length; v++) {
                if (isConnected[u][v] == 1 && !visited[v]) {
                    visited[v] = true;
                    q.add(v);
                }
            }
        }
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int findCircleNum(const std::vector<std::vector<int>>& isConnected) {
        int n = isConnected.size();
        std::vector<bool> visited(n, false);
        int provinces = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                provinces++;
                std::queue<int> q;
                visited[i] = true;
                q.push(i);
                while (!q.empty()) {
                    int u = q.front();
                    q.pop();
                    for (int v = 0; v < n; v++) {
                        if (isConnected[u][v] == 1 && !visited[v]) {
                            visited[v] = true;
                            q.push(v);
                        }
                    }
                }
            }
        }
        return provinces;
    }
};
```
</details>

- **Complexity**: Time: $O(N^2)$, Space: $O(N)$.

---

Previous: [04_coding_mock.md](04_coding_mock.md) | Next: [../10-quick-revision/README.md](../10-quick-revision/README.md)
