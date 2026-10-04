[Home](../README.md) > [05-ai-assisted-coding](README.md) > 02_prefix_sum_hashmap.md

# Prefix Sum & Prefix XOR HashMap Patterns

---

## Problem 1 (AIC-007): Subarray Sum Equals K (Negatives Allowed)

**Tag**: [ADDED]  
**Practice Link**: [LeetCode: subarray-sum-equals-k](https://leetcode.com/problems/subarray-sum-equals-k/)

### Problem & Core Idea
Given an integer array `nums` and integer `k`, return total number of subarrays whose sum equals `k`. Since negatives exist, sliding window fails. Use running prefix sum: if `currentSum - k` exists in the frequency map, add its count. Initialize `map.put(0L, 1)`.

### Production Code (Java)
```java
import java.util.*;

public class SubarraySumK {
    public int subarraySum(int[] nums, int k) {
        Map<Long, Integer> prefixFreq = new HashMap<>();
        prefixFreq.put(0L, 1);
        long currentSum = 0;
        int count = 0;

        for (int num : nums) {
            currentSum += num;
            count += prefixFreq.getOrDefault(currentSum - k, 0);
            prefixFreq.put(currentSum, prefixFreq.getOrDefault(currentSum, 0) + 1);
        }
        return count;
    }
}
```

### Dry Run (`nums = [1, -1, 1]`, $k = 1$)
| $i$ | `num` | `currentSum` | Target `currentSum - 1` | Match Count Added | Map State |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 1 | $1 - 1 = 0$ | $+1$ (from 0) | `{0:1, 1:1}` |
| 1 | -1 | 0 | $0 - 1 = -1$ | $+0$ | `{0:2, 1:1}` |
| 2 | 1 | 1 | $1 - 1 = 0$ | $+2$ (from 0) | `{0:2, 1:2}` |

*Result*: Total count = $1 + 0 + 2 = \mathbf{3}$ (Subarrays: `[1]`, `[1, -1, 1]`, `[1]`).

### Shortcut, Edge Cases & Complexity
- **Shortcut**: `prefixSum - k` frequency lookup in HashMap.
- **Edge Cases**: All negatives, $k = 0$, single element matching $k$.
- **Complexity**: Time $O(N)$, Space $O(N)$.
- **Prompt**: `"Task: Subarray Sum Equals K in Java. Negatives allowed. Use running long sum and HashMap of prefix frequencies initialized with (0L, 1). Return count. Time O(N), Space O(N)."`

---

## Problem 2 (AIC-008): Continuous Subarray Sum (Multiple of K, Length $\ge 2$)

**Tag**: [ADDED]  
**Practice Link**: [LeetCode: continuous-subarray-sum](https://leetcode.com/problems/continuous-subarray-sum/)

### Problem & Core Idea
Return `true` if `nums` has a continuous subarray of size at least 2 whose sum is a multiple of $k$.  
*Math*: If $\text{prefix}[j] \pmod k == \text{prefix}[i] \pmod k$, the subarray sum from $i+1$ to $j$ is a multiple of $k$. Store first-seen index of each remainder. Initialize `map.put(0, -1)`. Return true if $j - \text{prevIndex} \ge 2$.

### Production Code (Java)
```java
import java.util.*;

public class ContinuousSubarraySum {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> remainderIndex = new HashMap<>();
        remainderIndex.put(0, -1);
        int runningSum = 0;

        for (int i = 0; i < nums.length; i++) {
            runningSum += nums[i];
            int rem = (k == 0) ? runningSum : runningSum % k;
            if (rem < 0) rem += k;

            if (remainderIndex.containsKey(rem)) {
                if (i - remainderIndex.get(rem) >= 2) return true;
            } else {
                remainderIndex.put(rem, i); // Only store earliest index
            }
        }
        return false;
    }
}
```

### Dry Run (`nums = [23, 2, 4, 6, 7]`, $k = 6$)
| $i$ | `num` | `runningSum` | `rem = sum % 6` | Map Contains `rem`? | Action / Check |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 23 | 23 | 5 | No | Store `{5: 0}` |
| 1 | 2 | 25 | 1 | No | Store `{1: 1}` |
| 2 | 4 | 29 | 5 | **Yes (index 0)** | $i - \text{prev} = 2 - 0 = 2 \ge 2 \implies \mathbf{true}$ |

*Result*: Returns `true` (Subarray `[2, 4]` has sum 6).

### Shortcut, Edge Cases & Complexity
- **Shortcut**: Same remainder at distance $\ge 2 \implies$ multiple of $k$.
- **Edge Cases**: Two zeroes `[0, 0]` with $k = 0$ or any $k$ returns `true`.
- **Complexity**: Time $O(N)$, Space $O(\min(N, K))$.
- **Prompt**: `"Task: Continuous Subarray Sum in Java. Modulo remainder map storing earliest index, initialized with (0, -1). If same remainder found with i - prev >= 2, return true. Time O(N)."`

---

## Problem 3 (AIC-009): Subarrays Divisible by K (Negative Modulo Normalization)

**Tag**: [ADDED]  
**Practice Link**: [LeetCode: subarray-sums-divisible-by-k](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

### Problem & Core Idea
Find the number of non-empty subarrays with a sum divisible by $k$.  
*Math*: If two prefix sums have the same remainder modulo $k$, their difference is divisible by $k$. In Java, `%` on negative numbers produces negative remainders. Normalize with `((r % k) + k) % k`. Count frequency of each remainder using `map.put(0, 1)`.

### Production Code (Java)
```java
import java.util.*;

public class SubarraysDivisibleByK {
    public int subarraysDivByK(int[] nums, int k) {
        int[] remCount = new int[k];
        remCount[0] = 1; // Base case: remainder 0 starts with count 1
        int prefixSum = 0, total = 0;

        for (int num : nums) {
            prefixSum += num;
            int rem = ((prefixSum % k) + k) % k; // Normalize negative remainder
            total += remCount[rem];
            remCount[rem]++;
        }
        return total;
    }
}
```

### Dry Run (`nums = [4, 5, 0, -2, -3, 1]`, $k = 5$)
| $i$ | `num` | `prefixSum` | Normalized `rem` | Add to `total` | `remCount` Updated |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 4 | 4 | 4 | $+0$ | `remCount[4] = 1` |
| 1 | 5 | 9 | 4 | $+1$ | `remCount[4] = 2` |
| 2 | 0 | 9 | 4 | $+2$ | `remCount[4] = 3` |
| 3 | -2 | 7 | 2 | $+0$ | `remCount[2] = 1` |
| 4 | -3 | 4 | 4 | $+3$ | `remCount[4] = 4` |
| 5 | 1 | 5 | 0 | $+1$ | `remCount[0] = 2` |

*Result*: Total = $0 + 1 + 2 + 0 + 3 + 1 = \mathbf{7}$.

### Shortcut, Edge Cases & Complexity
- **Shortcut**: `rem = ((prefixSum % k) + k) % k`; count occurrences of matching remainders.
- **Edge Cases**: Large negative numbers (e.g. `num = -10000`).
- **Complexity**: Time $O(N)$, Space $O(K)$.
- **Prompt**: `"Task: Subarrays Divisible by K in Java. Use normalized remainder array rem = ((sum % k) + k) % k. Initialize remCount[0] = 1. Add remCount[rem] to total. Time O(N), Space O(K)."`

---

## Problem 4 (AIC-010): Contiguous Array (Equal 0s and 1s)

**Tag**: [ADDED]  
**Practice Link**: [LeetCode: contiguous-array](https://leetcode.com/problems/contiguous-array/)

### Problem & Core Idea
Find the maximum length of a contiguous subarray with an equal number of `0` and `1`.  
*Transformation*: Replace every `0` with `-1`. The problem reduces to finding the longest subarray with sum $= 0$. Store earliest seen prefix sum index in a HashMap initialized with `map.put(0, -1)`. Max length is $i - \text{firstSeen}(sum)$.

### Production Code (Java)
```java
import java.util.*;

public class ContiguousArray {
    public int findMaxLength(int[] nums) {
        Map<Integer, Integer> firstSeen = new HashMap<>();
        firstSeen.put(0, -1);
        int runningSum = 0, maxLen = 0;

        for (int i = 0; i < nums.length; i++) {
            runningSum += (nums[i] == 0) ? -1 : 1; // 0 becomes -1

            if (firstSeen.containsKey(runningSum)) {
                maxLen = Math.max(maxLen, i - firstSeen.get(runningSum));
            } else {
                firstSeen.put(runningSum, i); // Only store earliest index
            }
        }
        return maxLen;
    }
}
```

### Dry Run (`nums = [0, 1, 0, 1]`)
| $i$ | `nums[i]` | Value (`0 -> -1`) | `runningSum` | In `firstSeen`? | Length Evaluated | `maxLen` |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | -1 | -1 | No $\implies$ Put `{-1: 0}` | — | 0 |
| 1 | 1 | +1 | 0 | Yes (at index -1) | $1 - (-1) = 2$ | **2** |
| 2 | 0 | -1 | -1 | Yes (at index 0) | $2 - 0 = 2$ | 2 |
| 3 | 1 | +1 | 0 | Yes (at index -1) | $3 - (-1) = 4$ | **4** |

*Result*: Maximum length = $\mathbf{4}$ (entire array).

### Shortcut, Edge Cases & Complexity
- **Shortcut**: `0 -> -1` transformation; longest subarray with sum 0.
- **Edge Cases**: All zeroes `[0, 0, 0]` (returns 0); all ones `[1, 1, 1]` (returns 0).
- **Complexity**: Time $O(N)$, Space $O(N)$.
- **Prompt**: `"Task: Contiguous Array (Equal 0s and 1s) in Java. Transform 0 to -1. Store earliest index of prefix sum with map.put(0, -1). If sum repeats, maxLen = max(maxLen, i - map.get(sum)). Time O(N)."`

---

## Problem 5 (AIC-011): Subarrays with XOR Equal to K

**Tag**: [CHAT]

### Problem & Core Idea
Given an integer array `nums` and integer `k`, count subarrays whose bitwise XOR equals `k`.  
*Math*: Let cumulative XOR from 0 to $i$ be $XR$. For a subarray $(j, i)$ to have $\text{XOR} = k$, the previous prefix $P$ must satisfy $P \oplus k = XR \implies P = XR \oplus k$. Look up $XR \oplus k$ in frequency map initialized with `xorFreq.put(0, 1)`.

### Production Code (Java)
```java
import java.util.*;

public class SubarraysWithXORk {
    public static int countSubarraysWithXOR(int[] nums, int k) {
        if (nums == null || nums.length == 0) return 0;

        Map<Integer, Integer> xorFreq = new HashMap<>();
        xorFreq.put(0, 1); // Base case: prefix XOR 0 before array start

        int xr = 0, count = 0;
        for (int num : nums) {
            xr ^= num;
            int requiredPrefix = xr ^ k;
            count += xorFreq.getOrDefault(requiredPrefix, 0);
            xorFreq.put(xr, xorFreq.getOrDefault(xr, 0) + 1);
        }
        return count;
    }
}
```

### Dry Run (`nums = [4, 2, 2, 6, 4]`, $k = 6$)
| $i$ | `num` | `xr` (Cumulative XOR) | `requiredPrefix = xr ^ 6` | Matches Added | `xorFreq` Map |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 4 | 4 | $4 \oplus 6 = 2$ | $+0$ | `{0:1, 4:1}` |
| 1 | 2 | $4 \oplus 2 = 6$ | $6 \oplus 6 = 0$ | $+1$ (from 0) | `{0:1, 4:1, 6:1}` |
| 2 | 2 | $6 \oplus 2 = 4$ | $4 \oplus 6 = 2$ | $+0$ | `{0:1, 4:2, 6:1}` |
| 3 | 6 | $4 \oplus 6 = 2$ | $2 \oplus 6 = 4$ | $+2$ (from 4) | `{0:1, 4:2, 6:1, 2:1}` |
| 4 | 4 | $2 \oplus 4 = 6$ | $6 \oplus 6 = 0$ | $+1$ (from 0) | `{0:1, 4:2, 6:2, 2:1}` |

*Result*: Count = $0 + 1 + 0 + 2 + 1 = \mathbf{4}$ (Subarrays: `[4, 2]`, `[2, 2, 6]`, `[6]`, `[2, 6, 4]`).

### Shortcut, Edge Cases & Complexity
- **Shortcut**: `requiredPrefix = xr ^ k`; lookup count in frequency map.
- **Edge Cases**: $k = 0$ (counts subarrays where elements cancel out); empty array.
- **Complexity**: Time $O(N)$, Space $O(N)$.
- **Prompt**: `"Task: Count subarrays with XOR equal to K in Java. Running cumulative XOR xr ^= num. Add xorFreq.getOrDefault(xr ^ k, 0) to count. Update xorFreq. Time O(N), Space O(N)."`

---

## Problem 6 (AIC-012): Single Number III (Two Unique Elements via XOR Partition)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: single-number-iii](https://leetcode.com/problems/single-number-iii/)

### Problem & Core Idea
Given an integer array where every element appears twice except for two unique elements $u_1$ and $u_2$, find both unique numbers.  
*Bit Partition*: XOR sum of entire array gives $u_1 \oplus u_2$. Any set bit in this XOR sum indicates $u_1$ and $u_2$ differ at that bit position. Isolate the lowest set bit using `long diffBit = overallXor & (-overallXor)` (using `long` prevents `Integer.MIN_VALUE` negation overflow). Partition the array into two buckets and XOR each bucket separately.

### Production Code (Java)
```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        long overallXor = 0;
        for (int num : nums) overallXor ^= num;

        // Isolate lowest set bit using long to avoid Integer.MIN_VALUE overflow
        long diffBit = overallXor & (-overallXor);

        int u1 = 0, u2 = 0;
        for (int num : nums) {
            if ((num & diffBit) != 0) {
                u1 ^= num; // Bucket with diffBit set
            } else {
                u2 ^= num; // Bucket with diffBit unset
            }
        }
        return new int[]{u1, u2};
    }
}
```

### Dry Run (`nums = [1, 2, 1, 3, 2, 5]`)
- Overall XOR: $1 \oplus 2 \oplus 1 \oplus 3 \oplus 2 \oplus 5 = 3 \oplus 5 = 011_2 \oplus 101_2 = 110_2 = \mathbf{6}$.
- Lowest set bit: `diffBit = 6 & (-6) = 2` ($010_2$).
- Partitioning:
  - `(num & 2) != 0`: Numbers $2, 3, 2 \implies u_1 = 2 \oplus 3 \oplus 2 = \mathbf{3}$.
  - `(num & 2) == 0`: Numbers $1, 1, 5 \implies u_2 = 1 \oplus 1 \oplus 5 = \mathbf{5}$.
- *Output*: `[3, 5]`.

### Shortcut, Edge Cases & Complexity
- **Shortcut**: `overallXor = u1 ^ u2`; split array using `diffBit = overallXor & -overallXor`.
- **Edge Cases**: One unique number is `Integer.MIN_VALUE` (prevent overflow via `long`).
- **Complexity**: Time $O(N)$ (two passes), Space $O(1)$.
- **Prompt**: `"Task: Single Number III in Java. XOR all elements to find u1 ^ u2. Isolate lowest set bit using long diffBit = xor & (-xor) to prevent MIN_VALUE overflow. Partition elements into two buckets and XOR each. Time O(N), Space O(1)."`

---

Previous: [01_sliding_window.md](01_sliding_window.md) | Next: [03_tree_lca_bitwise_palindrome_partition.md](03_tree_lca_bitwise_palindrome_partition.md)
