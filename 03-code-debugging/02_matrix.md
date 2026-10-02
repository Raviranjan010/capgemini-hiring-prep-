# 2D Matrix Debugging Problems

---

## Problem 1: 2D Matrix Maximum Row Sum (All 7 Bugs Fixed)

**Tag**: [CHAT]  
**Video Reference**: 2D Matrix Debugging (7 Bugs) (unverified link, see [RESOURCES.md](../RESOURCES.md#video-references))

### Problem Statement
Given an $n \times m$ matrix of integers, calculate the sum of elements for each row and return the maximum row sum found. The matrix can contain negative integers.

### Sample Input & Output
- **Input**:
  ```text
  mat = [
    [-5,  2, -1],
    [-1, -2, -3]
  ]
  n = 2, m = 3
  ```
- **Output**: `-4` (Row 0 sum = $-5 + 2 - 1 = -4$; Row 1 sum = $-1 - 2 - 3 = -6$; $\max(-4, -6) = -4$)

### Buggy Exam Code (Demonstrating All 7 Bugs)
```cpp
// BUGGY CODE PROVIDED IN EXAM PORTAL
#include <vector>
using namespace std;

int findMaxRowSum(vector<vector<int>>& mat, int n, int m) {
    // BUG 1: Initializing maxSum to 0 fails when all rows have negative sums
    int maxSum = 0;

    // BUG 2: Loop condition <= n causes out-of-bounds row index access
    for (int i = 0; i <= n; i++) {
        // BUG 3: Declaring rowSum outside or forgetting to reset per row
        int rowSum; 

        // BUG 4: Using row bound 'n' instead of column bound 'm' for inner loop
        for (int j = 0; j < n; j++) {
            // BUG 5: Overwriting value (=) instead of accumulating (+=)
            rowSum = mat[i][j]; 
        }

        // BUG 6: Inverted comparison condition (finds minimum instead of maximum)
        if (rowSum < maxSum) {
            maxSum = rowSum;
        }
    }

    // BUG 7: Returning last row's rowSum instead of global maxSum
    return rowSum;
}
```

### The 7 Bugs Identified & Why They Are Wrong
1. **`int maxSum = 0;`**: Fails on matrices with all-negative elements (e.g., maximum sum $-4$ will be falsely reported as $0$). Must be initialized to `INT_MIN` or `Integer.MIN_VALUE`.
2. **`i <= n` loop bound**: Vector indices are $0$-indexed up to $n-1$. Checking $i \le n$ causes a segmentation fault on `mat[n]`.
3. **Uninitialized / un-reset `rowSum`**: Must be explicitly reinitialized to `0` at the start of each row.
4. **`j < n` inner loop**: Rows have $m$ columns. Iterating up to $n$ causes array out-of-bounds or skips columns when $n \ne m$.
5. **`rowSum = mat[i][j]`**: Overwrites `rowSum` on each iteration, leaving only the last column's value. Must use `rowSum += mat[i][j]`.
6. **`if (rowSum < maxSum)`**: Inverted condition tracks the minimum row sum instead of the maximum.
7. **`return rowSum;`**: Returns the sum of the final row rather than the accumulated global maximum `maxSum`.

### Fixed Production Code

#### C++
```cpp
#include <vector>
#include <climits>
#include <algorithm>
using namespace std;

int findMaxRowSum(vector<vector<int>>& mat, int n, int m) {
    if (n == 0 || m == 0) return 0;

    // FIX 1: Support all-negative values with INT_MIN
    int maxSum = INT_MIN;

    // FIX 2: Loop strictly up to n
    for (int i = 0; i < n; i++) {
        // FIX 3: Reset row accumulator for each row
        int rowSum = 0;

        // FIX 4: Loop strictly up to m columns
        for (int j = 0; j < m; j++) {
            // FIX 5: Accumulate with +=
            rowSum += mat[i][j];
        }

        // FIX 6: Update maximum when rowSum is greater
        if (rowSum > maxSum) {
            maxSum = rowSum;
        }
    }

    // FIX 7: Return global maximum
    return maxSum;
}
```

#### Java
```java
public class MatrixDebugger {
    public static int findMaxRowSum(int[][] mat, int n, int m) {
        if (n == 0 || m == 0) return 0;

        int maxSum = Integer.MIN_VALUE; // FIX 1

        for (int i = 0; i < n; i++) { // FIX 2
            int rowSum = 0; // FIX 3

            for (int j = 0; j < m; j++) { // FIX 4
                rowSum += mat[i][j]; // FIX 5
            }

            if (rowSum > maxSum) { // FIX 6
                maxSum = rowSum;
            }
        }

        return maxSum; // FIX 7
    }
}
```

### Dry Run ($2 \times 3$ Matrix with Negatives)
Matrix: `mat = [[-5, 2, -1], [-1, -2, -3]]`, $n = 2, m = 3$.  
Initial State: `maxSum = INT_MIN (-2147483648)`

| Row $i$ | Column $j$ | Element `mat[i][j]` | `rowSum` Accumulation | Row Complete? | Comparison (`rowSum > maxSum`?) | New `maxSum` |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | -5 | $0 + (-5) = -5$ | No | - | `INT_MIN` |
| 0 | 1 | 2 | $-5 + 2 = -3$ | No | - | `INT_MIN` |
| 0 | 2 | -1 | $-3 + (-1) = \mathbf{-4}$ | Yes | $-4 > \text{INT\_MIN}$ (True) | $\mathbf{-4}$ |
| 1 | 0 | -1 | $0 + (-1) = -1$ | No | - | `-4` |
| 1 | 1 | -2 | $-1 + (-2) = -3$ | No | - | `-4` |
| 1 | 2 | -3 | $-3 + (-3) = \mathbf{-6}$ | Yes | $-6 > -4$ (False) | $\mathbf{-4}$ |

*Final Return*: `maxSum = -4`.

### Spot-It-Fast Rule
Scan in 10 seconds: Check `maxSum` initialization (must be `INT_MIN`), row/col bounds (`i < n` and `j < m`), and make sure `rowSum = 0` is inside the outer loop.

### Edge Cases
- All negative matrix: Correctly finds the least negative row sum without returning 0.
- $1 \times 1$ matrix: Correctly returns `mat[0][0]`.

### Time & Space Complexity
- **Time**: $O(N \times M)$ — Every matrix cell is visited once.
- **Space**: $O(1)$ — Constant auxiliary memory.

---

## Problem 2: Search in Row-Wise and Column-Wise Sorted Matrix ($O(N+M)$)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: search-a-2d-matrix-ii](https://leetcode.com/problems/search-a-2d-matrix-ii/)

### Problem Statement
Write an efficient algorithm that searches for an integer `target` in an $n \times m$ matrix. This matrix has the following properties:
- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

### Sample Input & Output
- **Input**:
  ```text
  matrix = [
    [1,   4,  7, 11],
    [2,   5,  8, 12],
    [3,   6,  9, 16],
    [10, 13, 14, 17]
  ]
  target = 5
  ```
- **Output**: `true`
- **Target**: `20` $\implies$ `false`

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
public class MatrixSearch {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;

        // BUG 1: Wrong starting corner (top-left gives two increasing directions, preventing elimination)
        int row = 0;
        int col = 0;

        // BUG 2: Logical OR (||) causes out-of-bounds ArrayIndexOutOfBoundsException
        while (row < n || col < m) {
            if (matrix[row][col] == target) {
                return true;
            // BUG 3: Wrong movement direction leads to infinite loop or missing cells
            } else if (matrix[row][col] > target) {
                row++;
            } else {
                col++;
            }
        }
        return false;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Starting at top-left `(0, 0)`**:
   - *Why wrong*: From `(0, 0)`, moving right increases values and moving down also increases values. If `target > matrix[0][0]`, you cannot know which path to take. Starting at top-right `(0, m - 1)` provides two opposing directions: moving left decreases values, moving down increases values.
2. **`row < n || col < m` in boundary condition**:
   - *Why wrong*: Using `||` allows the loop to run even if `col` has exceeded array limits while `row < n` is still true, causing `ArrayIndexOutOfBoundsException`. Boundary checks must use `&&`.
3. **Inverted movement directions**:
   - *Why wrong*: At top-right, if `matrix[row][col] > target`, the current column contains elements even larger below; we must eliminate the column (`col--`). If `matrix[row][col] < target`, the row contains smaller elements to the left; we must eliminate the row (`row++`).

### Fixed Production Code

#### Java
```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }

        int n = matrix.length;
        int m = matrix[0].length;

        // FIX 1: Start at top-right corner
        int row = 0;
        int col = m - 1;

        // FIX 2: Strictly require both bounds to be valid (&&)
        while (row < n && col >= 0) {
            if (matrix[row][col] == target) {
                return true; // Target found
            } else if (matrix[row][col] > target) {
                col--; // FIX 3: Target is smaller, eliminate current column
            } else {
                row++; // FIX 3: Target is larger, eliminate current row
            }
        }

        return false;
    }
}
```

#### C++
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;

        int n = matrix.size();
        int m = matrix[0].size();

        // Start at top-right corner
        int row = 0;
        int col = m - 1;

        while (row < n && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target) {
                col--;
            } else {
                row++;
            }
        }

        return false;
    }
};
```

### Dry Run (Target = 5)
Matrix: Top-Right `(0, 3)` has value `11`. Target = `5`.

| Step | `(row, col)` | `matrix[row][col]` | Check vs Target | Decision / Action | Next `(row, col)` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | `(0, 3)` | 11 | $11 > 5$ | Target is smaller $\to$ `col--` | `(0, 2)` |
| 2 | `(0, 2)` | 7 | $7 > 5$ | Target is smaller $\to$ `col--` | `(0, 1)` |
| 3 | `(0, 1)` | 4 | $4 < 5$ | Target is larger $\to$ `row++` | `(1, 1)` |
| 4 | `(1, 1)` | 5 | $5 == 5$ | Match found! | **Returns `true`** |

### Spot-It-Fast Rule
Check the starting coordinate: It must be `row = 0, col = m - 1` (or `row = n - 1, col = 0`). If it starts at `(0, 0)` or uses `||` in the while loop, it is defective.

### Edge Cases
- Target smaller than top-left element: `col` decrements to `-1`, returns `false` in $O(M)$ steps.
- Target larger than bottom-right element: `row` increments to $n$, returns `false` in $O(N)$ steps.
- Single element matrix `[[5]]`: Returns `true` for 5, `false` for 3.

### Time & Space Complexity
- **Time**: $O(N + M)$ — In each step, we decrement `col` or increment `row`.
- **Space**: $O(1)$ — Modifies zero memory.
