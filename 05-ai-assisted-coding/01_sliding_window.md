[Home](../README.md) > [05-ai-assisted-coding](README.md) > 01_sliding_window.md

# Sliding Window Algorithmic Patterns: Harmonic Subarrays & Extremes

**Exam Reference**: Capgemini Exceller AI Assist Coding Round  
**Video Reference**: [KN Academy 28 Sept Assessment Solution Video](https://youtu.be/Myw7Po8fyWw)

---

## Problem 1 (AIC-001): Maximum Sum Harmonic Subarray (AI-Assisted Coding)

**Tag**: [AI-ASSIST-CODING] [SLIDING-WINDOW] [VIDEO]  
**Video Link**: [KN Academy 28 Sept Assessment Solution Video](https://youtu.be/Myw7Po8fyWw)

### 1. Assessment Blueprint & Problem Definition

In the Capgemini Exceller Technical Assessment, the AI Assist Coding section evaluates whether a candidate can define algorithmic constraints, maintain token economy (~2,000 token budget), and resolve edge cases with the platform AI conversational bot.

#### Problem Statement
A continuous subarray is defined as **harmonic** if and only if the difference between its maximum element and minimum element is strictly equal to $1$:
$$\max(\text{subarray}) - \min(\text{subarray}) = 1$$

Given an array of non-negative integers `nums`, find and return the **maximum possible sum** across all valid continuous harmonic subarrays. If no valid harmonic subarray exists, return `0`.

#### Key Constraints & Mathematical Properties
1. **Contiguity**: Elements must be strictly contiguous (a continuous slice `nums[L...R]`), unlike the classic LeetCode problem *Longest Harmonious Subsequence*, which permits non-contiguous subsequences.
2. **Exact Number of Distinct Elements**: A valid harmonic subarray must contain **exactly two distinct values** of the form $\{x, x+1\}$:
   - If it contains only $1$ distinct element (e.g., `[2, 2, 2]`), then $\max - \min = 0 \ne 1$ (**Invalid**).
   - If it contains $\ge 3$ distinct elements (e.g., `[1, 2, 3]`), then $\max - \min \ge 2 \ne 1$ (**Invalid**).
3. **Return Zero on Failure**: If no slice satisfies the property, return `0`.

---

### 2. Test Case Breakdown & Execution Trace

| Input Array (`nums`) | Valid Harmonic Subarrays | Individual Sums | Max Sum Output |
| :--- | :--- | :---: | :---: |
| `[1, 3, 2, 5, 2, 3, 7]` | `[3, 2]` (sum 5), `[2, 3]` (sum 5) | 5, 5 | **5** (or 7 for `[3, 2, 2]`) |
| `[1, 2, 3, 4]` | `[1, 2]` (sum 3), `[2, 3]` (sum 5), `[3, 4]` (sum 7) | 3, 5, 7 | **7** (`[3, 4]`) |
| `[1, 1, 1]` | None ($\max - \min = 0$) | None | **0** |
| `[0, 1, 0, 1, 0]` | `[0, 1, 0, 1, 0]` ($\max=1, \min=0$) | $0+1+0+1+0 = 2$ | **2** |

---

### 3. Algorithmic Approaches: Naive vs. Optimal

```text
SLIDING WINDOW WORKFLOW
L                 R
▼                 ▼
[ 3, 2, 2, 2 ] ──> Expand R: map = {3:1, 2:3}, diff = 1, size = 2 (VALID)
[ 3, 2, 2, 5 ] ──> Expand R: element 5 makes diff = 3 > 1 (INVALID)
  ▲
  Shrink L until window contains elements with diff <= 1
```

#### Approach 1: Brute Force ($O(N^2)$ Time, $O(1)$ Space)
- Iterate through all pairs $(i, j)$ where $0 \le i \le j < N$.
- Track running $\min$, $\max$, and sum. If $\max - \min == 1$, update `maxSum`.
- **Verdict**: Fails with Time Limit Exceeded (TLE) when $N \ge 10^5$ ($10^{10}$ operations).

#### Approach 2: Optimal Variable-Size Sliding Window ($O(N)$ Time, $O(1)$ Space)
- Maintain two pointers: `left = 0` and `right` iterating from $0$ to $N - 1$.
- Use a frequency map / hash table to track distinct values in the current window `nums[left...right]`.
- For each incoming `nums[right]`, add it to `currentSum` and increment its count in the frequency map.
- **Shrink Condition**: While the window contains an invalid spread ($\max - \min > 1$):
  - Decrement `nums[left]` from the map and `currentSum`.
  - If its frequency reaches 0, remove the key from the map.
  - Increment `left`.
- **Update Condition**: If the window contains **exactly 2 distinct keys** and their absolute difference is $1$, update:
  $$\text{maxSum} = \max(\text{maxSum}, \text{currentSum})$$

---

### 4. Production Source Code

#### Java 17 Solution (Using `TreeMap` for Logarithmic Min/Max Access)
```java
import java.util.TreeMap;

public class MaximumSumHarmonicSubarray {

    public static long findMaxHarmonicSubarraySum(int[] nums) {
        if (nums == null || nums.length < 2) {
            return 0;
        }

        int n = nums.length;
        int left = 0;
        long currentSum = 0;
        long maxSum = 0;

        // TreeMap maintains keys in sorted order: firstKey() is min, lastKey() is max in O(log K)
        TreeMap<Integer, Integer> freqMap = new TreeMap<>();

        for (int right = 0; right < n; right++) {
            int val = nums[right];
            currentSum += val;
            freqMap.put(val, freqMap.getOrDefault(val, 0) + 1);

            // While the difference between max and min in current window exceeds 1, shrink from left
            while (!freqMap.isEmpty() && (freqMap.lastKey() - freqMap.firstKey() > 1)) {
                int leftVal = nums[left];
                currentSum -= leftVal;
                int count = freqMap.get(leftVal);
                if (count == 1) {
                    freqMap.remove(leftVal);
                } else {
                    freqMap.put(leftVal, count - 1);
                }
                left++;
            }

            // A valid harmonic subarray must contain exactly two distinct elements with difference == 1
            if (freqMap.size() == 2 && (freqMap.lastKey() - freqMap.firstKey() == 1)) {
                maxSum = Math.max(maxSum, currentSum);
            }
        }

        return maxSum;
    }

    public static void main(String[] args) {
        System.out.println(findMaxHarmonicSubarraySum(new int[]{1, 3, 2, 5, 2, 3, 7})); // Output: 5
        System.out.println(findMaxHarmonicSubarraySum(new int[]{1, 2, 3, 4}));             // Output: 7
        System.out.println(findMaxHarmonicSubarraySum(new int[]{1, 1, 1}));                // Output: 0
        System.out.println(findMaxHarmonicSubarraySum(new int[]{0, 1, 0, 1, 0}));          // Output: 2
    }
}
```

#### C++ Solution (Using `std::map`)
```cpp
#include <vector>
#include <map>
#include <algorithm>
#include <iostream>
using namespace std;

long long findMaxHarmonicSubarraySum(const vector<int>& nums) {
    int n = nums.size();
    if (n < 2) return 0;

    map<int, int> freqMap; // Ordered map maintains sorted keys
    int left = 0;
    long long currentSum = 0;
    long long maxSum = 0;

    for (int right = 0; right < n; right++) {
        int val = nums[right];
        freqMap[val]++;
        currentSum += val;

        // Shrink window if spread between max key and min key exceeds 1
        while (!freqMap.empty() && (freqMap.rbegin()->first - freqMap.begin()->first > 1)) {
            int leftVal = nums[left];
            freqMap[leftVal]--;
            if (freqMap[leftVal] == 0) {
                freqMap.erase(leftVal);
            }
            currentSum -= leftVal;
            left++;
        }

        // Valid harmonic window requires strictly max - min == 1 and exactly 2 distinct keys
        if (freqMap.size() == 2 && (freqMap.rbegin()->first - freqMap.begin()->first == 1)) {
            maxSum = max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

#### Complexity Analysis
- **Time Complexity**: $O(N)$ amortized. Since `freqMap` holds at most 3 distinct keys before triggering a shrink, `lastKey()` and `firstKey()` take $O(1)$ time. Each element enters and leaves the window at most once.
- **Space Complexity**: $O(1)$ auxiliary memory (at most 3 keys stored at any time).

---

### 5. Token-Economical AI Prompting Scripts

The Capgemini environment grants approximately **2,000 tokens**. Use these structured prompt templates to direct the bot accurately without wasting budget:

#### Prompt 1: Problem Definition & Constraints
```text
"I need to solve 'Maximum Sum Harmonic Subarray' in Java.
- Definition: A contiguous subarray where max(subarray) - min(subarray) == 1 strictly.
- Return: Maximum sum across all valid subarrays, or 0 if none exist.
- Approach: Variable-size Sliding Window with two pointers (left, right).
- State Tracking: TreeMap or HashMap to track frequency of window values and maintain currentSum.
- Invalidation Rule: While (max - min > 1), decrement nums[left], remove if count == 0, and increment left.
- Record Condition: Update maxSum only when map has exactly 2 distinct keys and (max - min == 1).
- Complexity target: O(N) time, O(1) auxiliary space.
Please provide the clean implementation."
```

#### Prompt 2: Edge-Case Verification
```text
"Verify these edge cases against the implementation:
1. nums = [2, 2, 2]: Identical elements must yield 0 (map.size() == 1, difference is 0).
2. nums = [1, 2, 3]: Adjacent harmonic pairs ([1, 2] and [2, 3]) must not merge into [1, 2, 3] (diff == 2).
3. Non-negative zeroes: nums = [0, 1, 0, 1] must yield 2.
Does the window properly shrink when a third distinct value enters?"
```

---

## Problem 2 (AIC-002): Sliding Window Maximum ($O(N)$ Monotonic Deque)

**Tag**: [SLIDING-WINDOW] [DEQUE]  
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

### Complexity
- **Time**: $O(N)$ — Each index is pushed and popped from the deque at most once.
- **Space**: $O(K)$ — Deque holds at most $K$ indices at any point.

---

## 3. High-Yield Practice Questions & Algorithmic Strategy

### Practice Problem 1 (AIC-003): Maximum Length of Subarray with At Most Two Distinct Elements
**Tag**: [SLIDING-WINDOW]  
**Practice Link**: [LeetCode 904: Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)

**Problem Statement**: Given an integer array `fruits`, return the length of the longest contiguous subarray that contains at most two distinct types of numbers.

**Key Difference from Harmonic**: Elements do not need to satisfy $|x - y| = 1$; any two distinct numbers (e.g., `[4, 7, 4, 7, 7]`) are valid.

#### Solution Template (Python 3)
```python
def totalFruit(fruits: list[int]) -> int:
    from collections import defaultdict
    count = defaultdict(int)
    left = 0
    max_len = 0

    for right in range(len(fruits)):
        count[fruits[right]] += 1

        while len(count) > 2:
            count[fruits[left]] -= 1
            if count[fruits[left]] == 0:
                del count[fruits[left]]
            left += 1

        max_len = max(max_len, right - left + 1)

    return max_len
```

---

### Practice Problem 2 (AIC-004): Consecutive Differences Condition Bug
**Tag**: [CODE-DEBUGGING] [MOCK-EXAM]

**Question**: An engineer implements the sliding window check for the harmonic subarray problem as follows:
```java
if (freqMap.lastKey() - freqMap.firstKey() == 1) {
    maxSum = Math.max(maxSum, currentSum);
}
```
For which of the following input arrays will this code produce an incorrect answer?
- **A)** `[1, 2, 1, 2]`
- **B)** `[5, 5, 5, 5]`
- **C)** `[3, 4, 3, 4]`
- **D)** `[10, 11]`

**Correct Answer**: **Option B**

**Deep Explanation**: For `[5, 5, 5, 5]`, `freqMap.lastKey()` is $5$ and `freqMap.firstKey()` is $5$. The difference is $5 - 5 = 0 \ne 1$. However, if the code omits `freqMap.size() == 2`, single-element arrays could pass depending on boundary configurations. More critically, if `freqMap` contains only 1 key or is misconfigured, evaluating differences without asserting `freqMap.size() == 2` permits false window states.

---

### Practice Problem 3 (AIC-005): Sliding Window vs. Prefix Sum Array Selection
**Tag**: [CS-FUNDAMENTALS] [ALGORITHM-STRATEGY]

**Scenario**: You are asked to find the maximum sum contiguous subarray whose sum is divisible by $K$. The array can contain both positive and negative values. Why does a standard two-pointer sliding window fail here, and what technique should be used instead?

- **A)** Sliding window works; sort the array first in $O(N \log N)$.
- **B)** Sliding window fails because negative numbers break the monotonicity of the window sum (expanding does not guarantee increasing sum, and shrinking does not guarantee decreasing sum); use Prefix Sum with Hash Map storing remainder indices ($O(N)$).
- **C)** Use a Max-Heap Priority Queue to reorder values dynamically.
- **D)** Convert all negative numbers to positive numbers using absolute values.

**Correct Answer**: **Option B**

**Deep Explanation**: Two-pointer sliding windows require monotonic growth properties (expanding the right pointer increases the metric, and shrinking the left pointer decreases it). When negative numbers are present, this monotonicity breaks. The standard approach is to track prefix sums modulo $K$: $\text{prefixSum}[i] \pmod K = \text{prefixSum}[j] \pmod K$.

---

### Practice Problem 4 (AIC-006): Bitwise Subarray Reduction Complexity
**Tag**: [BITWISE] [PSEUDOCODE]

**Pseudocode**:
```text
FUNCTION SubarrayBitwise(arr, N):
    max_val = 0
    FOR i FROM 0 TO N - 1:
        current_or = 0
        FOR j FROM i TO N - 1:
            current_or = current_or | arr[j]
            IF current_or > max_val THEN:
                max_val = current_or
            END IF
        END FOR
    END FOR
    RETURN max_val
```

**Question**: Given `arr = [3, 8, 4, 2]`, what is the minimum time complexity to compute `max_val` across all subarrays?
- **A)** $O(N^2)$ using the nested loop shown above.
- **B)** $O(N)$ by taking the bitwise OR of all elements across the entire array in a single pass.
- **C)** $O(N \log N)$ using merge sort divide-and-conquer.
- **D)** $O(2^N)$ using subset generation.

**Correct Answer**: **Option B**

**Deep Explanation**: The bitwise OR operation is monotonic: $A \mid B \ge A$. The maximum bitwise OR of any contiguous subarray is simply the bitwise OR of the entire array (`arr[0] | arr[1] | ... | arr[N - 1]`). A single pass of length $N$ computes this value in $O(N)$ time, making the nested loop completely redundant.

---

## 4. High-Yield Exam Summary

| Feature / Topic | Core Rule | Exam Pitfall to Avoid |
| :--- | :--- | :--- |
| **Harmonic Subarray** | $\max - \min == 1$ and contiguous. | Do not confuse with subsequences (which allow skipping elements). |
| **Window Invalidation** | Shrink when $\max - \min > 1$. | Decrement the map count and delete the key when it reaches 0. |
| **All-Identical Arrays** | `[3, 3, 3]` $\to$ Return 0. | Ensure you check `map.size() == 2` before updating `maxSum`. |
| **Token Conservation** | Keep bot prompts concise and structured. | Do not paste the full problem statement; provide only constraints and edge cases. |

---

Previous: [README.md](README.md) | Next: [02_prefix_sum_hashmap.md](02_prefix_sum_hashmap.md)
