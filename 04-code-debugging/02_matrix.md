[Home](../README.md) > [04-code-debugging](README.md) > 02_matrix.md

# 02. 2D Matrix Algorithms Debugging

This module covers multi-dimensional array traversals, row accumulator resets, boundary checking, and in-place transformations.

## Problem 1 (DBG-005): 2D Matrix Maximum Row Sum (All 7 Bugs Fixed)
**Tag**: [VIDEO] | **Difficulty**: Hard | **Topic**: 2D Arrays & Matrices
**Source**: [YouTube Reference](https://youtu.be/SjbedEKjacM)

### Problem Statement
Given an $N \times M$ matrix of integers that can contain negative numbers, write a function that finds the maximum row sum and prints both the maximum sum and the 0-based row index that achieved it.

### Sample Input & Output
- **Input**:
  ```text
  3 3
  -5 -2 -9
  -1 -8 -3
  -4 -7 -6
  ```
- **Output**: `Row 1 has maximum sum -12`

### Buggy Exam Code (Demonstrating All 7 Bugs)
```c
#include <stdio.h>

void findMaxRowSum(int mat[][100], int n, int m) {
    int maxRow = 0;
    int maxSum = 0;           // Bug 1: 0 initial max fails on all-negative matrices
    int rowSum;               // Bug 2: uninitialized outside, never reset in loop

    for (int i = 0; i <= n; i++) {       // Bug 3: <= n causes out-of-bounds row access
        for (int j = 0; j < n; j++) {    // Bug 4: iterates up to n instead of m columns
            rowSum += mat[j][i];         // Bug 5: inverted indices (column-major)
        }
        if (rowSum > maxSum) {           // Bug 6: misses equal sums and fails negative max
            maxSum = rowSum;
            maxRow = i;
        }
    }
    printf("Row %d has maximum sum %d\n", maxRow, maxSum);
    return;                              // Bug 7: unnecessary return in void, placed inside loop in some exam variants
}
```

### The 7 Bugs Identified & Why They Are Wrong
1. **`maxSum = 0` Initialization**: Fails when all rows have negative sums. Maximum sum remains 0, which was never in the matrix.
2. **Missing `rowSum` Reset**: Declared outside the loop and not reset to 0 at the start of each row, accumulating sums across rows.
3. **Outer Loop Boundary (`<= n`)**: Accesses index $n$, which is out-of-bounds in 0-indexed array with $n$ rows.
4. **Inner Loop Bound (`j < n`)**: Matrix has $m$ columns; looping up to $n$ crashes on non-square matrices where $n > m$.
5. **Inverted Indexing (`mat[j][i]`)**: Accesses column $i$ and row $j$, transposing the traversal order.
6. **Strict Comparison on First Pass**: Because `maxSum` was initialized to 0, negative row sums never satisfy `rowSum > maxSum`.
7. **Misplaced Premature Loop Return**: In several student test instances, `return` was placed inside the outer loop body, exiting after row 0.

### Fixed Code
```java
public class MatrixDebug {
    public static void findMaxRowSum(int[][] mat, int n, int m) {
        if (n == 0 || m == 0) return;
        
        int maxRow = 0;
        int maxSum = Integer.MIN_VALUE;
        
        for (int i = 0; i < n; i++) {
            int rowSum = 0;
            for (int j = 0; j < m; j++) {
                rowSum += mat[i][j];
            }
            if (rowSum > maxSum) {
                maxSum = rowSum;
                maxRow = i;
            }
        }
        System.out.println("Row " + maxRow + " has maximum sum " + maxSum);
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
#include <iostream>
#include <vector>
#include <climits>

void findMaxRowSum(const std::vector<std::vector<int>>& mat, int n, int m) {
    if (n == 0 || m == 0) return;
    int maxRow = 0;
    int maxSum = INT_MIN;
    for (int i = 0; i < n; i++) {
        int rowSum = 0;
        for (int j = 0; j < m; j++) {
            rowSum += mat[i][j];
        }
        if (rowSum > maxSum) {
            maxSum = rowSum;
            maxRow = i;
        }
    }
    std::cout << "Row " << maxRow << " has maximum sum " << maxSum << "\n";
}
```
</details>

### Dry-Run Table
| Row Index `i` | Elements Traversed | Reset `rowSum` | Calculated `rowSum` | `maxSum` Updated | `maxRow` |
| :---: | :--- | :---: | :---: | :---: | :---: |
| 0 | `[-5, -2, -9]` | 0 | -16 | -16 | 0 |
| 1 | `[-1, -8, -3]` | 0 | -12 | -12 | 1 |
| 2 | `[-4, -7, -6]` | 0 | -17 | -12 (unchanged) | 1 |

### Spot-It-Fast Rule
In 2D matrix problems: Outer loop `< n`, inner loop `< m`, accumulator reset *inside* the outer loop, and `maxSum = Integer.MIN_VALUE`.

### Edge Cases
1. All elements negative: Correctly identifies the row with least negative sum.
2. Single row ($1 \times M$): Iterates once and reports row 0.
3. Rectangular matrix ($N \neq M$): Distinct $N$ and $M$ bounds avoid IndexOutOfBounds.

- **Complexity**: Time: $O(N \times M)$, Space: $O(1)$.

---

## Problem 2 (DBG-006): Staircase Search in Sorted Matrix ($O(N+M)$)
**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Matrix Search
**Source**: [YouTube Reference](https://youtu.be/SjbedEKjacM)

### Problem Statement
Write an efficient algorithm that searches for a target value in an $M \times N$ integer matrix where each row is sorted in ascending order and each column is sorted in ascending order.

### Sample Input & Output
- **Input**: `matrix = [[1,4,7],[2,5,8],[3,6,9]]`, `target = 5` -> `true`
- **Input**: `matrix = [[1,4,7],[2,5,8],[3,6,9]]`, `target = 20` -> `false`

### Buggy Exam Code
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = 0;
        int col = matrix[0].length; // Bug 1: out-of-bounds start
        
        while (row < matrix.length || col >= 0) { // Bug 2: logical OR instead of AND
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] < target) {
                col--; // Bug 3: wrong direction; smaller values are left, target is larger
            } else {
                row++; // Bug 4: wrong direction; larger values are down
            }
        }
        return false;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Starting Column Index Out of Bounds**: In 0-indexed matrix, top-right element is at `col = matrix[0].length - 1`. Initializing to `.length` throws `ArrayIndexOutOfBoundsException`.
2. **Loop Condition (`||` instead of `&&`)**: Using `||` continues execution even when one dimension is out-of-bounds, throwing exceptions.
3. **Inverted Search Navigation**: At `(row, col)`, if `matrix[row][col] < target`, the target is larger, so we must advance downward (`row++`), not `col--`. Conversely, if `matrix[row][col] > target`, we must move left (`col--`).

### Fixed Code
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return false;
        
        int row = 0;
        int col = matrix[0].length - 1;
        
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] < target) {
                row++;
            } else {
                col--;
            }
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
        int row = 0;
        int col = matrix[0].size() - 1;
        while (row < matrix.size() && col >= 0) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] < target) {
                row++;
            } else {
                col--;
            }
        }
        return false;
    }
};
```
</details>

### Dry-Run Table
| Step | Current Position `(row, col)` | Value at Position | Comparison with Target `5` | Next Action |
| :---: | :---: | :---: | :---: | :---: |
| 1 | `(0, 2)` | 7 | $7 > 5$ | `col--` (move left to 1) |
| 2 | `(0, 1)` | 4 | $4 < 5$ | `row++` (move down to 1) |
| 3 | `(1, 1)` | 5 | $5 == 5$ | Match found! Return `true` |

### Spot-It-Fast Rule
Top-right staircase search: Start at `(0, m - 1)`. If `val < target` move down (`row++`); if `val > target` move left (`col--`).

### Edge Cases
1. Empty matrix: Immediate check avoids NPE.
2. Target smaller than `matrix[0][0]` or larger than `matrix[n-1][m-1]`: Exits loop in $\le 1$ steps.

- **Complexity**: Time: $O(N + M)$, Space: $O(1)$.

---

## Problem 3 (DBG-025): Rotate Image 90 Degrees Clockwise
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Matrix Transformation
**Source**: Added practice

### Problem Statement
Given an $N \times N$ 2D matrix, rotate the matrix by 90 degrees clockwise in-place without allocating a second matrix.

### Sample Input & Output
- **Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
- **Output**: `[[7,4,1],[8,5,2],[9,6,3]]`

### Buggy Exam Code
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // Transpose
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) { // Bug: swaps twice!
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        // Reverse rows
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) { // Bug: reverses twice!
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
            }
        }
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Full-Grid Transpose Swap**: Looping `j` from `0` to `n` swaps `(i, j)` with `(j, i)` and then later swaps `(j, i)` with `(i, j)`, undoing the transpose. Inner loop must start at `j = i + 1`.
2. **Full-Row Reversal Loop**: Looping `j` from `0` to `n` reverses the row elements and then reverses them back. Inner loop must stop at `j < n / 2`.

### Fixed Code
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 1. Transpose across main diagonal
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        // 2. Reverse each row horizontally
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
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
    void rotate(std::vector<std::vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                std::swap(matrix[i][j], matrix[j][i]);
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                std::swap(matrix[i][j], matrix[i][n - 1 - j]);
            }
        }
    }
};
```
</details>

### Dry-Run Table
| Phase | Matrix State |
| :--- | :--- |
| Initial | `[[1,2,3], [4,5,6], [7,8,9]]` |
| After Transpose (`j = i+1..n-1`) | `[[1,4,7], [2,5,8], [3,6,9]]` |
| After Reversing Rows (`j < n/2`) | `[[7,4,1], [8,5,2], [9,6,3]]` |

### Spot-It-Fast Rule
In-place 90 degree rotate = Transpose (`j = i + 1`) + Horizontal Reflection (`j < n / 2`).

### Edge Cases
1. $1 \times 1$ matrix: Stays identical.
2. Even $N$ ($4 \times 4$): Row reversal midpoint handles cleanly.

- **Complexity**: Time: $O(N^2)$, Space: $O(1)$.

---

## Problem 4 (DBG-026): Set Matrix Zeroes In-Place Flag Overwrite
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Matrix Modification
**Source**: Added practice

### Problem Statement
Given an $M \times N$ integer matrix, if an element is 0, set its entire row and column to 0 in-place with $O(1)$ extra space.

### Sample Input & Output
- **Input**: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`
- **Output**: `[[1,0,1],[0,0,0],[1,0,1]]`

### Buggy Exam Code
```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    // Instantly zeroes out row and column!
                    for (int c = 0; c < n; c++) matrix[i][c] = 0;
                    for (int r = 0; r < m; r++) matrix[r][j] = 0;
                }
            }
        }
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Cascading Zero Propagation**: Setting row and column elements to 0 during the traversal turns non-zero cells into 0 prematurely, causing subsequent loop steps to zero out the entire matrix.

### Fixed Code
```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean firstColZero = false;
        
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) firstColZero = true;
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 1; j--) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
            if (firstColZero) matrix[i][0] = 0;
        }
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    void setZeroes(std::vector<std::vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        bool firstColZero = false;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) firstColZero = true;
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 1; j--) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
            if (firstColZero) matrix[i][0] = 0;
        }
    }
};
```
</details>

### Dry-Run Table
| Iteration | First Row/Col Marker Updates | Backward Scan Zero Infill |
| :--- | :--- | :--- |
| `matrix[1][1] == 0` | `matrix[1][0] = 0`, `matrix[0][1] = 0` | Row 1 and Col 1 marked |
| Backward Pass | Checks markers from bottom-right up | Correctly zeroes cells without cascading |

### Spot-It-Fast Rule
Use row 0 and column 0 as markers, and process cells in reverse order (`i = m-1 down to 0`) to prevent marker overwrite.

### Edge Cases
1. First column contains a zero: Handled via isolated boolean flag `firstColZero`.
2. All zeroes matrix: Stays all zeroes.

- **Complexity**: Time: $O(M \times N)$, Space: $O(1)$.

---

## Problem 5 (DBG-027): Spiral Matrix Boundary Condition Bug
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Matrix Traversal
**Source**: Added practice

### Problem Statement
Given an $M \times N$ matrix, return all elements of the matrix in spiral order (clockwise starting from top-left).

### Sample Input & Output
- **Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
- **Output**: `[1,2,3,6,9,8,7,4,5]`

### Buggy Exam Code
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int top = 0, bottom = matrix.length - 1;
        int left = 0, right = matrix[0].length - 1;
        
        while (top <= bottom && left <= right) {
            for (int i = left; i <= right; i++) res.add(matrix[top][i]);
            top++;
            for (int i = top; i <= bottom; i++) res.add(matrix[i][right]);
            right--;
            for (int i = right; i >= left; i--) res.add(matrix[bottom][i]); // Bug: duplicate on single row!
            bottom--;
            for (int i = bottom; i >= top; i--) res.add(matrix[i][left]);   // Bug: duplicate on single col!
            left++;
        }
        return res;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Boundary Guard in Bottom and Left Traversal**: After `top++` or `right--`, `top <= bottom` or `left <= right` may no longer hold. Without re-checking, single-row or single-column matrices duplicate elements.

### Fixed Code
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        if (matrix == null || matrix.length == 0) return res;
        
        int top = 0, bottom = matrix.length - 1;
        int left = 0, right = matrix[0].length - 1;
        
        while (top <= bottom && left <= right) {
            for (int i = left; i <= right; i++) res.add(matrix[top][i]);
            top++;
            for (int i = top; i <= bottom; i++) res.add(matrix[i][right]);
            right--;
            
            if (top <= bottom) {
                for (int i = right; i >= left; i--) res.add(matrix[bottom][i]);
                bottom--;
            }
            if (left <= right) {
                for (int i = bottom; i >= top; i--) res.add(matrix[i][left]);
                left++;
            }
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
    std::vector<int> spiralOrder(const std::vector<std::vector<int>>& matrix) {
        std::vector<int> res;
        if (matrix.empty()) return res;
        int top = 0, bottom = matrix.size() - 1;
        int left = 0, right = matrix[0].size() - 1;
        while (top <= bottom && left <= right) {
            for (int i = left; i <= right; i++) res.push_back(matrix[top][i]);
            top++;
            for (int i = top; i <= bottom; i++) res.push_back(matrix[i][right]);
            right--;
            if (top <= bottom) {
                for (int i = right; i >= left; i--) res.push_back(matrix[bottom][i]);
                bottom--;
            }
            if (left <= right) {
                for (int i = bottom; i >= top; i--) res.push_back(matrix[i][left]);
                left++;
            }
        }
        return res;
    }
};
```
</details>

### Dry-Run Table
| Iteration | Direction | Elements Added | Updated Bounds |
| :---: | :--- | :--- | :--- |
| 1 | Top row | `1, 2, 3` | `top = 1` |
| 1 | Right col | `6, 9` | `right = 1` |
| 1 | Bottom row (`top <= bottom`) | `8, 7` | `bottom = 1` |
| 1 | Left col (`left <= right`) | `4` | `left = 1` |
| 2 | Center element | `5` | `top = 2 > bottom = 1` -> Exit |

### Spot-It-Fast Rule
Always guard the bottom and left loops with `if (top <= bottom)` and `if (left <= right)` inside the spiral loop.

### Edge Cases
1. Single row matrix `[[1, 2, 3]]`: Guard prevents reading backwards across row 0 twice.
2. Single column matrix `[[1], [2], [3]]`: Guard prevents reading upwards across col 0 twice.

- **Complexity**: Time: $O(M \times N)$, Space: $O(1)$ auxiliary.

---

Previous: [01_trees.md](01_trees.md) | Next: [03_greedy_and_intervals.md](03_greedy_and_intervals.md)
