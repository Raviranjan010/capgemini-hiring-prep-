# Sliding Window Algorithmic Patterns

---

## Problem 1: Maximum Sum Harmonic Subarray ($\max - \min == 1$)

**Tag**: [VIDEO]  
*(Note: unverified link, see [RESOURCES.md](../RESOURCES.md#video-references); confirm exact problem wording in video)*

### Problem Statement
A harmonic subarray is defined as a contiguous subarray where the difference between the maximum value and the minimum value is strictly equal to 1 ($\max - \min == 1$), meaning the subarray contains exactly two distinct integers that differ by 1. Find the maximum possible sum among all contiguous harmonic subarrays. If no harmonic subarray exists, return 0.

### Idea in Easy Words
1. Use an ordered map (or frequency table) tracking the elements currently inside the window `[left to right]`.
2. Expand `right` by adding `nums[right]`.
3. If the spread $\max - \min > 1$, shrink from `left` until the condition $\max - \min \le 1$ is restored.
4. When the window contains at least two distinct keys and $\max - \min == 1$, update `maxSum = max(maxSum, currentSum)`.

### Production Code (C++)
```cpp
#include <vector>
#include <map>
#include <algorithm>
using namespace std;

long long maxHarmonicSubarraySum(vector<int>& nums) {
    int n = nums.size();
    if (n < 2) return 0;

    map<int, int> windowMap; // Ordered map maintains sorted keys
    int left = 0;
    long long currentSum = 0;
    long long maxSum = 0;

    for (int right = 0; right < n; right++) {
        int val = nums[right];
        windowMap[val]++;
        currentSum += val;

        // Shrink window if spread between max key and min key exceeds 1
        while (!windowMap.empty() && (windowMap.rbegin()->first - windowMap.begin()->first > 1)) {
            int leftVal = nums[left];
            windowMap[leftVal]--;
            if (windowMap[leftVal] == 0) {
                windowMap.erase(leftVal);
            }
            currentSum -= leftVal;
            left++;
        }

        // Valid harmonic window requires strictly max - min == 1 and at least 2 distinct keys
        if (windowMap.size() >= 2 && (windowMap.rbegin()->first - windowMap.begin()->first == 1)) {
            maxSum = max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

### Dry Run
Input: `nums = [1, 2, 2, 3, 1]`, $n = 5$.

| `right` | `nums[right]` | `windowMap` | $\max - \min$ | Action | `currentSum` | `maxSum` |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | `{1: 1}` | $1 - 1 = 0$ | 1 distinct key $\implies$ invalid | 1 | 0 |
| 1 | 2 | `{1: 1, 2: 1}` | $2 - 1 = 1$ | 2 keys, diff 1 $\implies$ valid | $1 + 2 = 3$ | **3** |
| 2 | 2 | `{1: 1, 2: 2}` | $2 - 1 = 1$ | 2 keys, diff 1 $\implies$ valid | $3 + 2 = 5$ | **5** |
| 3 | 3 | `{1: 1, 2: 2, 3: 1}` | $3 - 1 = 2 > 1$ | Shrink left: erase 1, `left = 1` | $7 - 1 = 6$ | 5 |
| | | `{2: 2, 3: 1}` | $3 - 2 = 1$ | 2 keys, diff 1 $\implies$ valid | 6 | **6** |
| 4 | 1 | `{2: 2, 3: 1, 1: 1}` | $3 - 1 = 2 > 1$ | Shrink until diff $\le 1$ | — | 6 |

*Output*: `6` (Subarray `[2, 2, 3]` with sum $2 + 2 + 3 = 6$).

### 5-Second Shortcut
Harmonic condition $\max - \min == 1$ requires an ordered map or min/max tracking: shrink `left` as soon as $\max - \min > 1$, and only record `maxSum` when exactly 2 keys exist with diff 1.

### Edge Cases
- All identical elements `[2, 2, 2]`: $\max - \min = 0 \ne 1$, returns `0`.
- Array length $< 2$: Returns `0` immediately.

### Complexity
- **Time**: $O(N \log K)$ where $K \le 3$ distinct keys in window $\implies O(N)$ effectively.
- **Space**: $O(1)$ auxiliary memory (at most 3 keys stored in map).

### Prompt to Give the Chatbot
```text
Task: Maximum Sum Harmonic Subarray in C++.
Constraints: Contiguous subarray where max - min == 1 strictly. Return maximum sum as long long.
Approach: Two pointers sliding window with std::map. When map.rbegin()->first - map.begin()->first > 1, shrink left. If valid (size >= 2 and diff == 1), update maxSum.
Complexity target: Time O(N), Space O(1).
```

---

## Problem 2: Sliding Window Maximum ($O(N)$ Monotonic Deque)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: sliding-window-maximum](https://leetcode.com/problems/sliding-window-maximum/)

### Problem Statement
You are given an array of integers `nums`, and a sliding window of size `k` moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position, return the maximum element in the window.

### The 4 Steps in Easy Words
1. **Evict Expired Indices**: Check the front of the deque; if the index is $\le i - k$, it has slipped outside the window. Poll it from the front.
2. **Maintain Decreasing Monotonicity**: While the deque is not empty and the element at the back is $\le nums[i]$, poll from the back. Smaller older elements can never be the maximum again.
3. **Enqueue Current Index**: Push index $i$ to the back of the deque.
4. **Collect Window Maximum**: Once $i \ge k - 1$ (first complete window formed), the largest element is always at the front (`nums[deque.peekFirst()]`).

### Production Code (Java)
```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || k <= 0 || nums.length == 0) return new int[0];

        int n = nums.length;
        int[] result = new int[n - k + 1];
        int resIdx = 0;

        // Stores indices of array elements in decreasing order of their values
        Deque<Integer> deque = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {
            // Step 1: Evict indices that have fallen out of the current window [i - k + 1, i]
            while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
                deque.pollFirst();
            }

            // Step 2: Remove smaller elements from the back to maintain monotonic decrease
            while (!deque.isEmpty() && nums[deque.peekLast()] <= nums[i]) {
                deque.pollLast();
            }

            // Step 3: Insert current index at back
            deque.offerLast(i);

            // Step 4: Record maximum from front once window has size k
            if (i >= k - 1) {
                result[resIdx++] = nums[deque.peekFirst()];
            }
        }

        return result;
    }
}
```

### Dry Run
Input: `nums = [1, 3, -1, -3, 5, 3, 6, 7]`, $k = 3$.

| $i$ | `nums[i]` | Evict ($<= i - 3$) | Pop Smaller from Back | Deque (Indices) | Deque (Values) | $i \ge 2$? Window Max (`peekFirst`) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | None | None | `[0]` | `[1]` | No ($i = 0 < 2$) |
| 1 | 3 | None | Pop 0 ($1 \le 3$) | `[1]` | `[3]` | No ($i = 1 < 2$) |
| 2 | -1 | None | None ($-1 < 3$) | `[1, 2]` | `[3, -1]` | Yes $\implies \mathbf{3}$ |
| 3 | -3 | None | None ($-3 < -1$) | `[1, 2, 3]` | `[3, -1, -3]` | Yes $\implies \mathbf{3}$ |
| 4 | 5 | Evict 1 ($1 \le 4 - 3$) | Pop 3, 2 ($-3, -1 \le 5$) | `[4]` | `[5]` | Yes $\implies \mathbf{5}$ |
| 5 | 3 | None | None ($3 < 5$) | `[4, 5]` | `[5, 3]` | Yes $\implies \mathbf{5}$ |
| 6 | 6 | None | Pop 5, 4 ($3, 5 \le 6$) | `[6]` | `[6]` | Yes $\implies \mathbf{6}$ |
| 7 | 7 | None | Pop 6 ($6 \le 7$) | `[7]` | `[7]` | Yes $\implies \mathbf{7}$ |

*Output*: `[3, 3, 5, 5, 6, 7]`.

### 5-Second Shortcut
Sliding window maximum = Double-Ended Queue (Deque) storing indices in monotonic decreasing order. Front element is always the maximum.

### Edge Cases
- $k = 1$: Output is identical to `nums`.
- Monotonically decreasing array `[5, 4, 3, 2, 1]`: No back elements popped; front elements cleanly evict one by one.

### Complexity
- **Time**: $O(N)$ — Each index is pushed and popped from the deque at most once.
- **Space**: $O(K)$ — Deque holds at most $K$ indices at any point.

### Prompt to Give the Chatbot
```text
Task: Sliding Window Maximum in Java.
Approach: Monotonic decreasing Deque storing indices.
Steps: 
1. Evict deque.peekFirst() <= i - k. 
2. While nums[deque.peekLast()] <= nums[i], deque.pollLast(). 
3. deque.offerLast(i). 
4. If i >= k - 1, output nums[deque.peekFirst()].
Complexity target: Time O(N), Space O(K).
```
