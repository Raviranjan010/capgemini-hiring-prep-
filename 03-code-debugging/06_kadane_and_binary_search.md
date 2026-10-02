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

## 3. Common Bug Categories in Capgemini Assessments

| Bug Category | Common Manifestation | Quick Fix |
| :--- | :--- | :--- |
| **Strict Inequality** | Using `>=` instead of `>` or `<=` instead of `<` | Compare against exact boundary constraints (e.g., $\vert{}\Delta h\vert{} \le 1$ allows diff $= 1$). |
| **Sentinel Suppression** | Using `&&` instead of `\|\|` when propagating failure flags | Return immediately if *any* branch returns the error sentinel (`-1` or `false`). |
| **Backtracking Missing** | Leaving visited/stack state marked after returning | Always revert modified state before exiting the recursive frame (`inStack[u] = false;`). |
| **Premature Loop Returns** | Placing `return count;` inside the loop body | Ensure the final return sits outside the traversal loop. |
| **Integer Truncation/Overflow** | Calculating `(low + high) / 2` with large bounds | Use `low + (high - low) / 2` to prevent 32-bit signed overflow. |
| **Accumulator Initialization** | Setting `maxSum = 0` when arrays can have negative numbers | Initialize accumulators to `nums[0]` or `INT_MIN`. |
