[Home](../README.md) > [04-code-debugging](README.md) > 06_kadane_and_binary_search.md

# 06. Kadane's Subarrays & Binary Search Debugging

This module covers prefix accumulations, maximum subarray tracking with negative numbers, binary search midpoint overflows, and boundary pointer adjustments.

## 1. High-Speed Debugging Strategy (Within 20 Minutes)
1. **Kadane All-Negative Test**: Check whether max tracker is initialized to `nums[0]` or `Integer.MIN_VALUE`, never `0`.
2. **Binary Search Midpoint**: Check `mid = low + (high - low) / 2` to avoid integer overflow.
3. **Loop Termination**: Check `while (low <= high)` vs `while (low < high)`.
4. **Pointer Adjustment**: Ensure `low = mid + 1` and `high = mid - 1` to prevent infinite loops.

---

## Problem 1 (DBG-018): Kadane's Algorithm for Maximum Subarray (All Negative Array Bug)
**Tag**: [VIDEO] | **Difficulty**: Easy | **Topic**: Arrays / Kadane
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=YEZy2e_PARE)

### Problem Statement
Given an integer array `nums`, find the subarray with the largest sum, and return its sum.

### Sample Input & Output
- **Input**: `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]` -> `6` (`[4, -1, 2, 1]`)
- **Input**: `nums = [-3, -1, -5]` -> `-1`

### Buggy Exam Code
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSoFar = 0;     // Bug 1: 0 fails on all-negative arrays!
        int currentMax = 0;
        
        for (int i = 0; i < nums.length; i++) {
            currentMax += nums[i];
            if (currentMax > maxSoFar) {
                maxSoFar = currentMax;
            }
            if (currentMax < 0) {
                currentMax = 0;
            }
        }
        return maxSoFar;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Zero Initialization for Maximum**: If all elements in `nums` are negative (e.g. `[-3, -1, -5]`), `maxSoFar` remains `0`, which is greater than any element in the array. The algorithm must return `-1`, not `0`. `maxSoFar` must be initialized to `nums[0]`.

### Fixed Code
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSoFar = nums[0];
        int currentMax = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            currentMax = Math.max(nums[i], currentMax + nums[i]);
            maxSoFar = Math.max(maxSoFar, currentMax);
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
    int maxSubArray(const std::vector<int>& nums) {
        int maxSoFar = nums[0];
        int currentMax = nums[0];
        for (size_t i = 1; i < nums.size(); i++) {
            currentMax = std::max(nums[i], currentMax + nums[i]);
            maxSoFar = std::max(maxSoFar, currentMax);
        }
        return maxSoFar;
    }
};
```
</details>

### Dry-Run Table
| Element `nums[i]` | `currentMax + nums[i]` | `Math.max(nums[i], ...)` | `maxSoFar` Updated |
| :---: | :---: | :---: | :---: |
| -3 (Init) | - | -3 | -3 |
| -1 | -4 | -1 | -1 |
| -5 | -6 | -5 | -1 |

### Spot-It-Fast Rule
Kadane's tracker initialization: `maxSoFar = nums[0]`, loop starts at `i = 1`.

### Edge Cases
1. All negative numbers `[-5, -2, -8]`: Correctly returns `-2`.
2. Single element array `[-1]`: Returns `-1`.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 2 (DBG-019): Binary Search Lower Bound (Overflow & Infinite Loop)
**Tag**: [VIDEO] | **Difficulty**: Easy | **Topic**: Binary Search
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=YEZy2e_PARE)

### Problem Statement
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

### Sample Input & Output
- **Input**: `nums = [1, 3, 5, 6]`, `target = 5` -> `2`
- **Input**: `nums = [1, 3, 5, 6]`, `target = 2` -> `1`

### Buggy Exam Code
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0, high = nums.length; // Bug 1: high = length instead of length - 1
        
        while (low < high) { // Bug 2: misses when low == high
            int mid = (low + high) / 2; // Bug 3: Integer overflow on large indices
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                low = mid; // Bug 4: Infinite loop when high = low + 1
            } else {
                high = mid - 1;
            }
        }
        return low;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Integer Overflow in Mid Calculation**: `(low + high)` can exceed $2^{31} - 1$ when array indices are large. Must use `low + (high - low) / 2`.
2. **Infinite Loop via `low = mid`**: When `low` and `high` differ by 1, integer division truncates `mid` to `low`. Assigning `low = mid` never advances `low`, causing an infinite loop (TLE).

### Fixed Code
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return low;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int searchInsert(const std::vector<int>& nums, int target) {
        int low = 0, high = (int)nums.size() - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return low;
    }
};
```
</details>

### Dry-Run Table
| `low` | `high` | `mid` | `nums[mid]` | Target `2` Comparison | Next Pointers |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 3 | 1 | 3 | $3 > 2$ | `high = 1 - 1 = 0` |
| 0 | 0 | 0 | 1 | $1 < 2$ | `low = 0 + 1 = 1` |
| 1 | 0 | - | - | Loop terminates | Return `low = 1` |

### Spot-It-Fast Rule
Binary search invariant: `mid = low + (high - low) / 2`, `low = mid + 1`, `high = mid - 1`.

### Edge Cases
1. Target smaller than all elements: Returns `0`.
2. Target larger than all elements: Returns `nums.length`.

- **Complexity**: Time: $O(\log N)$, Space: $O(1)$.

---

## Problem 3 (DBG-020): Maximum Circular Subarray Sum
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Arrays / Kadane
**Source**: Added practice

### Problem Statement
Given a circular integer array `nums` of length `n`, return the maximum possible sum of a non-empty subarray of `nums`.

### Sample Input & Output
- **Input**: `nums = [1, -2, 3, -2]` -> `3`
- **Input**: `nums = [5, -3, 5]` -> `10` ($5 + 5$ wrapping around)

### Buggy Exam Code
```java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int total = 0, maxSum = nums[0], curMax = 0, minSum = nums[0], curMin = 0;
        for (int x : nums) {
            curMax = Math.max(x, curMax + x);
            maxSum = Math.max(maxSum, curMax);
            curMin = Math.min(x, curMin + x);
            minSum = Math.min(minSum, curMin);
            total += x;
        }
        // Bug: If all numbers are negative, total - minSum = 0, returning 0 instead of max negative!
        return Math.max(maxSum, total - minSum);
    }
}
```

### Bugs Found & Why They Are Wrong
1. **All-Negative Edge Case Bug**: When all elements are negative, `total == minSum`, so `total - minSum = 0` (representing an empty subarray, which is disallowed). `maxSum` holds the largest negative element. The code must check `if (maxSum < 0) return maxSum;`.

### Fixed Code
```java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int total = 0, maxSum = nums[0], curMax = 0, minSum = nums[0], curMin = 0;
        for (int x : nums) {
            curMax = Math.max(x, curMax + x);
            maxSum = Math.max(maxSum, curMax);
            curMin = Math.min(x, curMin + x);
            minSum = Math.min(minSum, curMin);
            total += x;
        }
        return maxSum < 0 ? maxSum : Math.max(maxSum, total - minSum);
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int maxSubarraySumCircular(const std::vector<int>& nums) {
        int total = 0, maxSum = nums[0], curMax = 0, minSum = nums[0], curMin = 0;
        for (int x : nums) {
            curMax = std::max(x, curMax + x);
            maxSum = std::max(maxSum, curMax);
            curMin = std::min(x, curMin + x);
            minSum = std::min(minSum, curMin);
            total += x;
        }
        return maxSum < 0 ? maxSum : std::max(maxSum, total - minSum);
    }
};
```
</details>

### Dry-Run Table
| Array | Total | `maxSum` | `minSum` | `maxSum < 0`? | Result |
| :---: | :---: | :---: | :---: | :---: | :---: |
| `[5, -3, 5]` | 7 | 7 | -3 | False | `max(7, 7 - (-3)) = 10` |
| `[-3, -2, -3]` | -8 | -2 | -8 | **True** | `-2` (Not $0$) |

### Spot-It-Fast Rule
Circular Kadane: Return `maxSum < 0 ? maxSum : Math.max(maxSum, total - minSum)`.

### Edge Cases
1. All negative numbers: Handled cleanly by `maxSum < 0`.
2. Array of size 1: Returns that single element.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 4 (DBG-021): Binary Search in Rotated Sorted Array
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Search
**Source**: Added practice

### Problem Statement
Given an integer array `nums` sorted in ascending order with distinct values, possibly rotated at an unknown pivot, find the index of `target`. If not found, return `-1`.

### Sample Input & Output
- **Input**: `nums = [4,5,6,7,0,1,2]`, `target = 0` -> `4`

### Buggy Exam Code
```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            
            // Bug: Uses strict '<' instead of '<=' for left sorted portion
            if (nums[low] < nums[mid]) {
                if (target >= nums[low] && target < nums[mid]) high = mid - 1;
                else low = mid + 1;
            } else {
                if (target > nums[mid] && target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
        }
        return -1;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Strict Inequality on Half Identification**: When `low == mid` (e.g. 2 elements remaining), `nums[low] < nums[mid]` evaluates to false, erroneously forcing execution into the right-half logic. Must check `nums[low] <= nums[mid]`.

### Fixed Code
```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            
            if (nums[low] <= nums[mid]) {
                if (target >= nums[low] && target < nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            } else {
                if (target > nums[mid] && target <= nums[high]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int search(const std::vector<int>& nums, int target) {
        int low = 0, high = (int)nums.size() - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            if (nums[low] <= nums[mid]) {
                if (target >= nums[low] && target < nums[mid]) high = mid - 1;
                else low = mid + 1;
            } else {
                if (target > nums[mid] && target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
        }
        return -1;
    }
};
```
</details>

### Dry-Run Table
| Iteration | `low` | `high` | `mid` | `nums[mid]` | Left Sorted? | Target In Range? | Next Action |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| 1 | 0 | 6 | 3 | 7 | Yes ($4 \le 7$) | No ($0 \notin [4, 7)$) | `low = 4` |
| 2 | 4 | 6 | 5 | 1 | Yes ($0 \le 1$) | Yes ($0 \in [0, 1)$) | `high = 4` |
| 3 | 4 | 4 | 4 | 0 | Match! | - | Return `4` |

### Spot-It-Fast Rule
Rotated binary search: Half check is `nums[low] <= nums[mid]` (with `<=`), not strictly `<`.

### Edge Cases
1. Array size 2 `[3, 1]`, `target = 1`: `low = 0, mid = 0` requires `<=` to identify left half correctly.
2. Target not present: Returns `-1`.

- **Complexity**: Time: $O(\log N)$, Space: $O(1)$.

---

## Problem 5 (DBG-022): Peak Element Finding in Array
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Search
**Source**: Added practice

### Problem Statement
A peak element is an element that is strictly greater than its neighbors. Given a 0-indexed integer array `nums`, find a peak element, and return its index.

### Sample Input & Output
- **Input**: `nums = [1, 2, 3, 1]` -> `2` (index of value 3)

### Buggy Exam Code
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int low = 0, high = nums.length - 1;
        while (low <= high) { // Bug 1: <= causes out-of-bounds mid + 1 access!
            int mid = low + (high - low) / 2;
            if (nums[mid] < nums[mid + 1]) { // ArrayIndexOutOfBoundsException when mid = high!
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Loop Termination and Out-of-Bounds Access**: When `low <= high`, if `low == high`, `mid` equals `nums.length - 1`. `nums[mid + 1]` throws `ArrayIndexOutOfBoundsException`. In binary search for peak, termination must be `while (low < high)`.

### Fixed Code
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int low = 0, high = nums.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] < nums[mid + 1]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int findPeakElement(const std::vector<int>& nums) {
        int low = 0, high = (int)nums.size() - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] < nums[mid + 1]) low = mid + 1;
            else high = mid;
        }
        return low;
    }
};
```
</details>

### Dry-Run Table
| Iteration | `low` | `high` | `mid` | `nums[mid] < nums[mid+1]` | Next Bounds |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 3 | 1 | $2 < 3$ (True) | `low = 2` |
| 2 | 2 | 3 | 2 | $3 < 1$ (False) | `high = 2` |
| End | 2 | 2 | - | `low == high` | Return `2` |

### Spot-It-Fast Rule
Peak search comparing `mid` with `mid + 1` MUST use `while (low < high)`.

### Edge Cases
1. Strictly increasing array `[1, 2, 3]`: Returns last index `2`.
2. Strictly decreasing array `[3, 2, 1]`: Returns index `0`.

- **Complexity**: Time: $O(\log N)$, Space: $O(1)$.

---

## Problem 6 (DBG-038): First and Last Position of Element in Sorted Array
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Search Bounds
**Source**: Added practice

### Problem Statement
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value. If target is not found, return `[-1, -1]`.

### Sample Input & Output
- **Input**: `nums = [5,7,7,8,8,10]`, `target = 8` -> `[3, 4]`

### Buggy Exam Code
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int first = findBound(nums, target, true);
        int last = findBound(nums, target, false);
        return new int[]{first, last};
    }
    
    private int findBound(int[] nums, int target, boolean isFirst) {
        int low = 0, high = nums.length - 1, ans = -1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                ans = mid;
                // Bug: Does not adjust high or low to continue searching!
                break;
            } else if (nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return ans;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Premature Break on First Match**: Standard binary search breaks upon finding `nums[mid] == target`. To find the first occurrence, the search must continue left (`high = mid - 1`). To find the last occurrence, it must continue right (`low = mid + 1`).

### Fixed Code
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int first = findBound(nums, target, true);
        int last = findBound(nums, target, false);
        return new int[]{first, last};
    }
    
    private int findBound(int[] nums, int target, boolean isFirst) {
        int low = 0, high = nums.length - 1, ans = -1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                ans = mid;
                if (isFirst) high = mid - 1; // Continue searching left
                else low = mid + 1;          // Continue searching right
            } else if (nums[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return ans;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<int> searchRange(const std::vector<int>& nums, int target) {
        return {findBound(nums, target, true), findBound(nums, target, false)};
    }
private:
    int findBound(const std::vector<int>& nums, int target, boolean isFirst) {
        int low = 0, high = (int)nums.size() - 1, ans = -1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                ans = mid;
                if (isFirst) high = mid - 1;
                else low = mid + 1;
            } else if (nums[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return ans;
    }
};
```
</details>

### Dry-Run Table
| Target `8` Search | Match Found at Index | Search Direction | Final Index |
| :---: | :---: | :---: | :---: |
| First Bound | `mid = 3` | `high = 2` (Left) | 3 |
| Last Bound | `mid = 4` | `low = 5` (Right) | 4 |

### Spot-It-Fast Rule
To find first occurrence: on match set `high = mid - 1`. To find last: on match set `low = mid + 1`.

### Edge Cases
1. Target not present: Returns `[-1, -1]`.
2. Single occurrence: Both first and last return the same index.

- **Complexity**: Time: $O(\log N)$, Space: $O(1)$.

---

## Problem 7 (DBG-039): Find Minimum in Rotated Sorted Array
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Binary Search
**Source**: Added practice

### Problem Statement
Given the sorted rotated array `nums` of unique elements, return the minimum element of this array in $O(\log N)$ time.

### Sample Input & Output
- **Input**: `nums = [3,4,5,1,2]` -> `1`

### Buggy Exam Code
```java
class Solution {
    public int findMin(int[] nums) {
        int low = 0, high = nums.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            // Bug: Compares with nums[low] instead of nums[high]!
            if (nums[mid] > nums[low]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return nums[low];
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Comparing with `nums[low]` Instead of `nums[high]`**: When the array is completely sorted (not rotated, e.g. `[1, 2, 3]`), `nums[mid] > nums[low]` causes `low = mid + 1`, skipping the minimum at index 0. The inflection point must be detected by comparing `nums[mid]` against `nums[high]`.

### Fixed Code
```java
class Solution {
    public int findMin(int[] nums) {
        int low = 0, high = nums.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] > nums[high]) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return nums[low];
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int findMin(const std::vector<int>& nums) {
        int low = 0, high = (int)nums.size() - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] > nums[high]) low = mid + 1;
            else high = mid;
        }
        return nums[low];
    }
};
```
</details>

### Dry-Run Table
| Iteration | `low` | `high` | `mid` | `nums[mid]` | `nums[high]` | Action |
| :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| 1 | 0 | 4 | 2 | 5 | 2 | $5 > 2$ -> `low = 3` |
| 2 | 3 | 4 | 3 | 1 | 2 | $1 < 2$ -> `high = 3` |
| End | 3 | 3 | - | - | - | Return `nums[3] = 1` |

### Spot-It-Fast Rule
Compare `mid` with `high`: if `nums[mid] > nums[high]`, minimum is strictly right (`low = mid + 1`), else `high = mid`.

### Edge Cases
1. Array not rotated `[1, 2, 3]`: `nums[mid] < nums[high]` always moves `high = mid`, correctly returning `1`.
2. Single-element array: Returns `nums[0]`.

- **Complexity**: Time: $O(\log N)$, Space: $O(1)$.

---

## Problem 8 (DBG-040): Search Insert Position Boundary Bug
**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Binary Search
**Source**: Added practice

### Problem Statement
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

### Sample Input & Output
- **Input**: `nums = [1, 3, 5, 6]`, `target = 7` -> `4`

### Buggy Exam Code
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            if (nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return high; // Bug: returns high instead of low!
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Returning `high` on Insertion**: When `target` is not found, `high` ends up at `low - 1` (the last element smaller than `target`). The insertion position is `low`, not `high`.

### Fixed Code
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            if (nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return low; // Correct insertion position
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int searchInsert(const std::vector<int>& nums, int target) {
        int low = 0, high = (int)nums.size() - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) return mid;
            if (nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return low;
    }
};
```
</details>

### Dry-Run Table
| Iteration | `low` | `high` | `mid` | `nums[mid]` | Target `7` | Bounds Update |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | 3 | 1 | 3 | $3 < 7$ | `low = 2` |
| 2 | 2 | 3 | 2 | 5 | $5 < 7$ | `low = 3` |
| 3 | 3 | 3 | 3 | 6 | $6 < 7$ | `low = 4` |
| End | 4 | 3 | - | - | Terminated | Return `low = 4` |

### Spot-It-Fast Rule
When binary search fails to find target, `low` points to the exact insertion index.

### Edge Cases
1. Target larger than all elements: Returns `nums.length`.
2. Target smaller than all elements: Returns `0`.

- **Complexity**: Time: $O(\log N)$, Space: $O(1)$.

---

Previous: [05_graphs_and_dp.md](05_graphs_and_dp.md) | Next: [05-ai-assisted-coding/README.md](../05-ai-assisted-coding/README.md)
