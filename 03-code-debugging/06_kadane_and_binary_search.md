# Debugging Kadane's Algorithm & Binary Search

**Tag**: [CODE-DEBUGGING]  
**Video Reference**: [KN Academy Code Debugging Assessment Breakdown](http://www.youtube.com/watch?v=YEZy2e_PARE)

---

## 1. High-Speed Debugging Strategy (Within 20 Minutes)

1. **Pre-visualize the Optimal Solution (First 3–4 mins)**: Read the problem constraints and recall the standard algorithm (e.g., $O(N)$ Kadane with negative bounds, $O(\log N)$ binary search with strict boundary bounds).
2. **Scan Return Types & Sentinel Flags**: Verify error conditions or boundary indices propagate properly.
3. **Inspect Comparison Operators Strictly**: Check for `>` vs. `>=` or `<` vs. `<=` mismatches.
4. **Identify Loop Updating & Mid Calculations**: Watch for `high = mid` infinite loops on two-element arrays and integer overflow `(low + high) / 2`.

---

## Problem 1: Kadane's Algorithm for Maximum Subarray (All Negative Array Bug)

**Tag**: [MOCK-EXAM]  
**Practice Link**: [LeetCode: maximum-subarray](https://leetcode.com/problems/maximum-subarray/)

### Problem Statement
Given an integer array `nums`, find the subarray with the largest sum, and return its sum.

### Buggy Exam Code
```java
// BUGGY: Fails when all elements are negative
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSoFar = 0; // BUG: Initialized to 0
        int currMax = 0;

        for (int x : nums) {
            currMax = currMax + x;
            if (currMax < 0) {
                currMax = 0;
            }
            if (currMax > maxSoFar) {
                maxSoFar = currMax;
            }
        }
        return maxSoFar; // Returns 0 for [-3, -2, -5], but answer should be -2!
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Zero-Initialization of Max Accumulator (`maxSoFar = 0`)**:
   - *Why wrong*: If the array contains exclusively negative numbers (e.g., `nums = [-3, -2, -5]`), the maximum subarray sum must be the largest single negative element (`-2`). Initializing `maxSoFar = 0` causes the function to return `0`, which is not even an element or subarray sum of `nums`.
   - *Fix*: Initialize both `maxSoFar = nums[0]` and `currMax = nums[0]`, and begin the iteration from index 1.

### Fixed Production Code

#### Java
```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        int maxSoFar = nums[0];
        int currMax = nums[0];

        for (int i = 1; i < nums.length; i++) {
            currMax = Math.max(nums[i], currMax + nums[i]);
            maxSoFar = Math.max(maxSoFar, currMax);
        }

        return maxSoFar;
    }
}
```

#### C++
```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxSubArray(const vector<int>& nums) {
        if (nums.empty()) return 0;
        int maxSoFar = nums[0];
        int currMax = nums[0];

        for (size_t i = 1; i < nums.size(); ++i) {
            currMax = max(nums[i], currMax + nums[i]);
            maxSoFar = max(maxSoFar, currMax);
        }
        return maxSoFar;
    }
};
```

---

## Problem 2: Binary Search First Occurrence (Lower Bound Mid Update Bug)

**Tag**: [MOCK-EXAM]  
**Practice Link**: [LeetCode: find-first-and-last-position-of-element-in-sorted-array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### Problem Statement
Given a sorted array `arr` and a target value `target`, find the index of the first occurrence of `target`. If not found, return `-1`.

### Buggy Exam Code
```cpp
// BUGGY: Infinite loop on low + 1 == high and integer overflow
int lowerBound(vector<int>& arr, int target) {
    int low = 0, high = arr.size() - 1;
    int ans = -1;

    while (low <= high) {
        int mid = (low + high) / 2; // BUG 1: Potential integer overflow

        if (arr[mid] >= target) {
            ans = mid;
            high = mid; // BUG 2: Infinite loop when arr[mid] == target and low == mid
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

### Bugs Found & Why They Are Wrong
1. **Integer Overflow in Midpoint Calculation (`(low + high) / 2`)**:
   - *Why wrong*: When `low + high` exceeds $2^{31} - 1$ ($2,147,483,647$), it wraps around to negative values, causing an `IndexOutOfBounds` exception or memory segmentation fault.
   - *Fix*: Use `int mid = low + (high - low) / 2;`.
2. **Infinite Loop via Stale Boundary Assignment (`high = mid`)**:
   - *Why wrong*: When `low == high` and `arr[mid] >= target`, setting `high = mid` leaves `high` completely unchanged. The condition `low <= high` continues to hold indefinitely, locking the search into a Time Limit Exceeded (TLE) infinite loop.
   - *Fix*: Record `ans = mid;` and strictly shrink the right search boundary to `high = mid - 1;`.

### Fixed Production Code

#### C++
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int lowerBound(const vector<int>& arr, int target) {
        int low = 0, high = static_cast<int>(arr.size()) - 1;
        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2; // Overflow-safe

            if (arr[mid] >= target) {
                if (arr[mid] == target) {
                    ans = mid; // Record candidate
                }
                high = mid - 1; // Strictly reduce boundary
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }
};
```

---

## Problem 3: Standard Binary Search Boundary Conditions (Multiple Boundary & Update Bugs)

**Tag**: [MOCK-EXAM]  
**Practice Link**: [LeetCode: binary-search](https://leetcode.com/problems/binary-search/)

### Problem Statement
Given a sorted array `arr` and a target value `target`, search for `target` in `arr`. If `target` exists, return its index. Otherwise, return `-1`.

### Buggy Exam Code
```java
// BUGGY: Multiple boundary and pointer advancement flaws
int binarySearch(int[] arr, int target) {
    int low = 0;
    int high = arr.length; // BUG 1: Out of bounds on direct index access

    while (low < high) { // BUG 2: Misses single-element search range
        int mid = (low + high) / 2; // BUG 3: Potential integer overflow
        if (arr[mid] == target) 
            return mid;
        else if (arr[mid] < target) 
            low = mid; // BUG 4: Infinite loop when high - low == 1
        else 
            high = mid; // BUG 5: Does not eliminate mid from next search
    }
    return -1;
}
```

### Bugs Found & Why They Are Wrong
1. **Out-of-Bounds High Pointer (`int high = arr.length`)**:
   - *Why wrong*: In 0-indexed arrays, valid indices are $0$ to $n-1$. If `mid = (0 + n) / 2`, accessing `arr[n]` when target is not found or when `arr.length == 1` causes `ArrayIndexOutOfBoundsException`.
   - *Fix*: Set `int high = arr.length - 1;`.
2. **Premature Loop Termination (`while (low < high)`)**:
   - *Why wrong*: When `low == high`, the search window contains exactly one element that has not yet been examined. Using `<` skips this element, returning `-1` even if that single element equals `target`.
   - *Fix*: Use `while (low <= high)`.
3. **Integer Overflow in Midpoint Calculation (`(low + high) / 2`)**:
   - *Why wrong*: If `low + high` exceeds $2^{31} - 1$, it wraps to negative, throwing index exceptions.
   - *Fix*: Use `int mid = low + (high - low) / 2;`.
4. **Stale Pointer Updates (`low = mid` and `high = mid`)**:
   - *Why wrong*: Since `arr[mid]` is already verified not to equal `target`, keeping `mid` inside the search range causes an infinite loop whenever `low` and `high` are adjacent.
   - *Fix*: Advance strictly beyond `mid`: `low = mid + 1;` and `high = mid - 1;`.

### Fixed Production Code

#### Java
```java
class Solution {
    public int binarySearch(int[] arr, int target) {
        if (arr == null || arr.length == 0) return -1;

        int low = 0;
        int high = arr.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return -1;
    }
}
```

#### C++
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int binarySearch(const vector<int>& arr, int target) {
        int low = 0;
        int high = static_cast<int>(arr.size()) - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return -1;
    }
};
```

### Dry-Run Table (`arr = [5]`, `target = 5`)
| Iteration | `low` | `high` | Loop Cond (`low <= high`) | `mid` | `arr[mid]` | Action |
| :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Start** | 0 | 0 | `0 <= 0` (True) | 0 | 5 | `arr[0] == target` $\implies$ Return index `0`. |

> **Spot-It-Fast Rule (10 Seconds)**: Whenever you see `int high = arr.length` combined with `low = mid;`, immediately flag an **infinite loop / off-by-one trap**. In standard closed-interval search, always enforce `high = n - 1`, `while (low <= high)`, and `low = mid + 1; high = mid - 1;`.

---

## Problem 4: Array Frequency & Duplicate Counter (Index Out of Bounds & Visited Trap)

**Tag**: [MOCK-EXAM]  
**Practice Link**: [LeetCode: find-all-duplicates-in-an-array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)

### Problem Statement
Given an integer array `arr`, calculate and print the frequency of each distinct element in the array. Each distinct element should only have its frequency reported once.

### Buggy Exam Code
```java
// BUGGY: Array index out of bounds and redundant duplicate reporting
public class FrequencyCounter {
    public static void printFrequencies(int[] arr) {
        // Intended to count distinct element frequencies
        for (int i = 0; i <= arr.length; i++) { // BUG 1: IndexOutOfBoundsException (<=)
            int count = 0;
            for (int j = 0; j < arr.length; j++) {
                if (arr[i] == arr[j]) { // Will throw exception when i == arr.length
                    count++;
                }
            }
            // BUG 2: Prints duplicates multiple times without visited tracking
            System.out.println(arr[i] + ": " + count);
        }
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Loop Boundary Overflow (`i <= arr.length`)**:
   - *Why wrong*: Array indices range from `0` to `arr.length - 1`. When `i == arr.length`, evaluating `arr[i]` triggers an immediate `ArrayIndexOutOfBoundsException`.
   - *Fix*: Change the loop condition to `i < arr.length`.
2. **Redundant Duplicate Output ($O(N^2)$ without Visited State)**:
   - *Why wrong*: For an array like `[2, 3, 2, 2]`, the inner loop recalculates and prints `"2: 3"` three separate times instead of once.
   - *Fix*: Use a `LinkedHashMap<Integer, Integer>` to track frequencies in $O(N)$ time, or sort the array and iterate using two pointers.

### Fixed Production Code

#### Java (Optimal $O(N)$ Time & $O(N)$ Space using LinkedHashMap)
```java
import java.util.LinkedHashMap;
import java.util.Map;

public class FrequencyCounter {
    public static void printFrequencies(int[] arr) {
        if (arr == null || arr.length == 0) return;

        Map<Integer, Integer> freqMap = new LinkedHashMap<>();
        for (int num : arr) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }

        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }
}
```

#### C++ (Optimal $O(N)$ Time & $O(N)$ Space using unordered_map)
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

void printFrequencies(const vector<int>& arr) {
    if (arr.empty()) return;

    unordered_map<int, int> freq;
    for (int num : arr) {
        freq[num]++;
    }

    for (const auto& pair : freq) {
        cout << pair.first << ": " << pair.second << "\n";
    }
}
```

### Dry-Run Table (`arr = [2, 3, 2]`)
| Step / Element | Operation | `freqMap` State | Output Line |
| :---: | :---: | :---: | :---: |
| Element `2` | `freqMap.put(2, 1)` | `{2: 1}` | None |
| Element `3` | `freqMap.put(3, 1)` | `{2: 1, 3: 1}` | None |
| Element `2` | `freqMap.put(2, 2)` | `{2: 2, 3: 1}` | None |
| Output Phase | Print Entry 1 | - | `"2: 2"` |
| Output Phase | Print Entry 2 | - | `"3: 1"` |

> **Spot-It-Fast Rule (10 Seconds)**: In nested frequency counters, look immediately at the outer `for` loop condition (`<= arr.length` is an instant crash) and verify if an element is marked visited to prevent repeated logging.

---

## Problem 5: Alternating Odd-Even Subarray Sequence (Index Out-of-Bounds & Negative Modulo Bug)

**Tag**: [CODE-DEBUGGING] [VIDEO]  
**Timestamp**: `[00:15:28]` - `[00:17:35]`  
**Exam Source**: Capgemini Exceller Code Debugging Section

### Problem Statement
Given an integer array `arr[]` of size `n`, verify whether the sequence forms a contiguous alternating parity pattern starting from index 0:
$$\text{Odd} \longrightarrow \text{Even} \longrightarrow \text{Odd} \dots \quad \text{or} \quad \text{Even} \longrightarrow \text{Odd} \longrightarrow \text{Even} \dots$$
Count the total number of valid elements in the unbroken alternating sequence starting from index 0.

### Buggy Exam Code
```java
// Faulty snippet given to candidates in Capgemini assessment
public static int countAlternating(int[] arr, int n) {
    if (n == 0) return 0;
    int count = 1;
    for (int i = 0; i < n; i++) {              // BUG 1: Out of bounds check & wrong offset
        if (arr[i] % 2 != arr[i + 1] % 2) {     // BUG 2: Negative number modulo bug
            count++;
        } else {
            break;
        }
    }
    return count;
}
```

### Bugs Found & Root Cause Analysis

1. **Loop Index Out-of-Bounds (`i < n`)**:
   - *Why wrong*: The loop checks `arr[i + 1]`. When `i = n - 1` (the last valid index), accessing `arr[n]` triggers a runtime crash:
     $$\text{ArrayIndexOutOfBoundsException: Index } n \text{ out of bounds for length } n$$
   - *Fix*: Terminate the loop strictly at `i < n - 1`.
2. **Negative Number Modulo Bug (`arr[i] % 2 != arr[i + 1] % 2`)**:
   - *Why wrong*: In Java, C++, and C#, the remainder operator retains the sign of the dividend. For example, `-3 % 2 == -1`, but `3 % 2 == 1`. If an array contains `[-3, 1]`, both are odd, but `-1 != 1` evaluates to `true`, erroneously treating them as alternating parity!
   - *Fix*: Use bitwise parity testing: `((arr[i] ^ arr[i + 1]) & 1) == 1` or `(arr[i] & 1) != (arr[i + 1] & 1)`.

### Fixed Production Code

#### Java
```java
public class AlternatingSequence {
    public static int countAlternating(int[] arr, int n) {
        if (arr == null || n == 0) return 0;
        int count = 1;

        for (int i = 0; i < n - 1; i++) {
            // Bitwise parity check: immune to negative number modulo issues
            if (((arr[i] ^ arr[i + 1]) & 1) == 1) {
                count++;
            } else {
                break; // Stop at first parity violation
            }
        }
        return count;
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 6};
        System.out.println(countAlternating(arr1, arr1.length)); // Output: 4 ([1,2,3,4])

        int[] arr2 = {-3, 2, -5, 4};
        System.out.println(countAlternating(arr2, arr2.length)); // Output: 4 (handles negatives)
    }
}
```

#### C++
```cpp
#include <vector>
#include <iostream>
using namespace std;

int countAlternating(const vector<int>& arr) {
    int n = arr.size();
    if (n == 0) return 0;
    int count = 1;

    for (int i = 0; i < n - 1; i++) {
        // Bitwise XOR of lowest bits: 1 if different parity, 0 if same parity
        if (((arr[i] ^ arr[i + 1]) & 1) == 1) {
            count++;
        } else {
            break;
        }
    }
    return count;
}
```

### Dry-Run Table (`arr = [1, 2, 4, 5]`, $n = 4$)
| Loop Index $i$ | Pair Checked (`arr[i]`, `arr[i+1]`) | Bitwise Parity `((arr[i]^arr[i+1]) & 1)` | Action Taken | `count` Value |
| :---: | :---: | :---: | :---: | :---: |
| - | - | - | Initialized | `1` |
| `i = 0` | `(1, 2)` (Odd, Even) | `(1 ^ 0) & 1 = 1` (Alternating) | `count++` | `2` |
| `i = 1` | `(2, 4)` (Even, Even) | `(0 ^ 0) & 1 = 0` (Violation) | `break;` | `2` (Final) |

> **Spot-It-Fast Rule (10 Seconds)**: In any question testing adjacent element parity, immediately look for:
> 1. `i < n` accessing `arr[i + 1]` $\implies$ change to `i < n - 1`.
> 2. `x % 2` on signed integers $\implies$ replace with `(x & 1)` or `((x ^ y) & 1)`.

---

## 5. Common Bug Categories in Capgemini Assessments

| Bug Category | Common Manifestation | Quick Fix |
| :--- | :--- | :--- |
| **Strict Inequality** | Using `>=` instead of `>` or `<=` instead of `<` | Compare against exact boundary constraints (e.g., $\vert{}\Delta h\vert{} \le 1$ allows diff $= 1$). |
| **Sentinel Suppression** | Using `&&` instead of `\|\|` when propagating failure flags | Return immediately if *any* branch returns the error sentinel (`-1` or `false`). |
| **Backtracking Missing** | Leaving visited/stack state marked after returning | Always revert modified state before exiting the recursive frame (`inStack[u] = false;`). |
| **Premature Loop Returns** | Placing `return count;` inside the loop body | Ensure the final return sits outside the traversal loop. |
| **Integer Truncation/Overflow** | Calculating `(low + high) / 2` with large bounds | Use `low + (high - low) / 2` to prevent 32-bit signed overflow. |
| **Accumulator Initialization** | Setting `maxSum = 0` when arrays can have negative numbers | Initialize accumulators to `nums[0]` or `INT_MIN`. |
| **Off-by-One Array Bound** | Writing `i <= arr.length` instead of `i < arr.length` | Remember that 0-indexed arrays terminate strictly at `arr.length - 1`. |
| **Binary Search Pointer Staleness** | Setting `low = mid` or `high = mid` without shifting | Always exclude `mid` in closed intervals: `low = mid + 1; high = mid - 1;`. |
