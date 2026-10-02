# Greedy Algorithms & Interval Debugging

---

## Problem 1: Jump Game I (Reachable Check)

**Tag**: [VIDEO]  
**Video Reference**: [Capgemini 20-Min Debugging Live Gameplay](https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO)  
**Practice Link**: [LeetCode: jump-game](https://leetcode.com/problems/jump-game/)

### Problem Statement
You are given an integer array `nums`. You are initially positioned at the array's first index (`0`), and each element represents your maximum jump length at that position. Return `true` if you can reach the last index (`nums.length - 1`), or `false` otherwise.

### Sample Input & Output
- **Input**: `nums = [2, 3, 1, 1, 4]` $\implies$ **Output**: `true` (Jump 1 step from index 0 to 1, then 3 steps to the last index).
- **Input**: `nums = [3, 2, 1, 0, 4]` $\implies$ **Output**: `false` (All paths lead to index 3 where jump length is 0).

### Buggy Exam Code
```cpp
// BUGGY CODE PROVIDED IN EXAM PORTAL
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxReach = 0;
        int n = nums.size();

        for (int i = 0; i < n; i++) {
            // BUG 1: Inverted condition check (i < maxReach instead of i > maxReach)
            if (i < maxReach) {
                return false;
            }

            // Calculating reach
            maxReach = max(maxReach, i + nums[i]);

            // BUG 2: Missing early-exit optimization check for reaching last index
        }

        // BUG 3: Unconditional return false exits falsely on valid paths
        return false; 
    }
};
```

### Bugs Found & Why They Are Wrong
1. **Inverted reach check (`if (i < maxReach) return false;`)**:
   - *Why wrong*: If $i < \text{maxReach}$, the current index is completely reachable. Returning `false` terminates valid executions prematurely. The failure condition occurs when current index cannot be reached, i.e., $i > \text{maxReach}$.
2. **Missing early exit check (`maxReach >= n - 1`)**:
   - *Why wrong*: Once `maxReach` reaches or exceeds the final index ($n - 1$), the loop should immediately exit and return `true` without scanning redundant trailing elements.
3. **Unconditional `return false;` after loop**:
   - *Why wrong*: If the loop completes without ever encountering an unreachable index ($i > \text{maxReach}$), the end was reachable. It must return `true`.

### Fixed Production Code

#### C++
```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxReach = 0;
        int n = nums.size();

        for (int i = 0; i < n; i++) {
            // FIX 1: Unreachable index check
            if (i > maxReach) {
                return false;
            }

            // Update farthest reach
            maxReach = max(maxReach, i + nums[i]);

            // FIX 2: Early return if last index is reached
            if (maxReach >= n - 1) {
                return true;
            }
        }

        // FIX 3: Entire array traversed successfully
        return true;
    }
};
```

#### Java
```java
public class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (i > maxReach) { // FIX 1
                return false;
            }

            maxReach = Math.max(maxReach, i + nums[i]);

            if (maxReach >= n - 1) { // FIX 2
                return true;
            }
        }

        return true; // FIX 3
    }
}
```

### Full Dry-Run Table for Failing Case `[3, 2, 1, 0, 4]` ($n = 5$, Last Index = $4$)

| Loop Index ($i$) | Element `nums[i]` | Reach Check ($i > \text{maxReach}$?) | New `maxReach = max(maxReach, i + nums[i])` | `maxReach >= 4`? | Action / Evaluation |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | 3 | $0 > 0$ (False) | $\max(0, 0 + 3) = \mathbf{3}$ | No | Continue |
| **1** | 2 | $1 > 3$ (False) | $\max(3, 1 + 2) = \mathbf{3}$ | No | Continue |
| **2** | 1 | $2 > 3$ (False) | $\max(3, 2 + 1) = \mathbf{3}$ | No | Continue |
| **3** | 0 | $3 > 3$ (False) | $\max(3, 3 + 0) = \mathbf{3}$ | No | Continue (Trapped at 0) |
| **4** | 4 | **$4 > 3$ (TRUE!)** | — | — | **Returns `false`** |

*Result*: At index $4$, $i > \text{maxReach}$ ($4 > 3$). The function successfully returns `false`.

### Spot-It-Fast Rule
Look at the loop condition: if you see `if (i < maxReach) return false;` or a trailing `return false;` after the loop, invert the check to `i > maxReach` and change the default return to `true`.

### Edge Cases
- Single element array `[0]`: Loop runs for $i = 0$, `maxReach = 0 >= 0` $\implies$ returns `true` immediately.
- Multiple zeroes: Handles zero-traps seamlessly by detecting when $i > \text{maxReach}$.

### Time & Space Complexity
- **Time**: $O(N)$ — Single pass over array.
- **Space**: $O(1)$ — No auxiliary data structures.

---

## Problem 2: Jump Game II (Minimum Jumps Count)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: jump-game-ii](https://leetcode.com/problems/jump-game-ii/)

### Problem Statement
Given a 0-indexed array of integers `nums` of length $n$, you are initially positioned at `nums[0]`. Each element `nums[i]` represents the maximum length of a forward jump. Return the minimum number of jumps to reach `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

### Sample Input & Output
- **Input**: `nums = [2, 3, 1, 1, 4]` $\implies$ **Output**: `2` (Index 0 $\to$ Index 1 $\to$ Index 4).

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
class Solution {
    public int jump(int[] nums) {
        int jumps = 0;
        int currentEnd = 0;
        int farthest = 0;

        // BUG 1: Missing single-element check (nums.length <= 1 should return 0)

        // BUG 2: Loop condition i < nums.length increments jumps an extra time at the destination
        for (int i = 0; i < nums.length; i++) {
            // BUG 3: Storing raw jump value nums[i] instead of target index (i + nums[i])
            farthest = Math.max(farthest, nums[i]);

            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
            }
        }
        return jumps;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Single-Element Guard (`nums.length <= 1`)**:
   - If array is `[0]`, you are already at the destination; 0 jumps are needed. The buggy code executes and returns 1.
2. **Loop Boundary Off-By-One (`i < nums.length` vs `i < nums.length - 1`)**:
   - *Why the loop stops at $n - 2$*: When the loop visits $i = n - 1$ (the final index), $i == \text{currentEnd}$ triggers another `jumps++`. But arriving at the last index does NOT require another jump. Hence, the loop must terminate at $n - 2$ (`i < nums.length - 1`).
3. **Using `nums[i]` instead of `i + nums[i]`**:
   - `nums[i]` is only jump distance. The absolute reachable index is `i + nums[i]`.

### Comparison Table: Jump Game I vs Jump Game II

| Feature | Jump Game I | Jump Game II |
| :--- | :--- | :--- |
| **Output Type** | `boolean` (`true` / `false`) | `int` (minimum jumps count) |
| **Goal** | Determine reachability | Count minimum transitions |
| **Key Condition** | `if (i > maxReach) return false;` | `if (i == currentEnd) { jumps++; currentEnd = farthest; }` |
| **Loop Boundary** | Up to $n - 1$ (`i < n`) | Up to $n - 2$ (`i < n - 1`) |

### Fixed Production Code

#### Java
```java
class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length <= 1) return 0; // FIX 1

        int jumps = 0;
        int currentEnd = 0;
        int farthest = 0;

        // FIX 2: Loop strictly stops at nums.length - 1 (index n - 2)
        for (int i = 0; i < nums.length - 1; i++) {
            // FIX 3: Track farthest reachable target index
            farthest = Math.max(farthest, i + nums[i]);

            // When current jump range is exhausted, transition to next jump
            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;

                if (currentEnd >= nums.length - 1) {
                    break; // Early exit
                }
            }
        }
        return jumps;
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
    int jump(vector<int>& nums) {
        if (nums.size() <= 1) return 0;

        int jumps = 0, currentEnd = 0, farthest = 0;
        int n = nums.size();

        for (int i = 0; i < n - 1; i++) {
            farthest = max(farthest, i + nums[i]);

            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
                if (currentEnd >= n - 1) break;
            }
        }
        return jumps;
    }
};
```

### Dry Run Table for `[2, 3, 1, 1, 4]` ($n = 5$, Loop runs for $i = 0 \dots 3$)

| Index $i$ | `nums[i]` | New `farthest = max(farthest, i + nums[i])` | Condition: $i == \text{currentEnd}$? | `jumps` Updated | New `currentEnd` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | 2 | $\max(0, 0 + 2) = \mathbf{2}$ | Yes ($0 == 0$) | $0 + 1 = \mathbf{1}$ | **2** |
| **1** | 3 | $\max(2, 1 + 3) = \mathbf{4}$ | No ($1 \ne 2$) | 1 | 2 |
| **2** | 1 | $\max(4, 2 + 1) = \mathbf{4}$ | Yes ($2 == 2$) | $1 + 1 = \mathbf{2}$ | **4** (Break: $4 \ge 4$) |

*Final Answer*: `jumps = 2`.

### Spot-It-Fast Rule
Look at the for-loop condition: if it says `i < nums.length` instead of `i < nums.length - 1`, or `farthest = Math.max(farthest, nums[i])`, fix them immediately.

### Time & Space Complexity
- **Time**: $O(N)$ — Single scan through array.
- **Space**: $O(1)$ — Constant memory.

---

## Problem 3: Gas Station Circular Tour ($O(N)$ Greedy)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: gas-station](https://leetcode.com/problems/gas-station/)

### Problem Statement
There are $n$ gas stations along a circular route. You are given two integer arrays `gas` and `cost` where `gas[i]` is the amount of gas at the $i$-th station and `cost[i]` is the cost of gas to travel from station $i$ to station $(i + 1) \pmod n$. Return the starting gas station's index if you can travel around the circuit once, or `-1` if impossible.

### Two Core Mathematical Ideas
1. **Global Feasibility**: If the total sum of gas is greater than or equal to the total sum of cost ($\sum \text{gas} \ge \sum \text{cost}$), a valid starting station is mathematically guaranteed to exist.
2. **Local Restart Property**: If starting from station $A$, your tank becomes negative when attempting to reach station $B$, then *no station between $A$ and $B$ can possibly reach $B$ either*. Therefore, the next viable candidate start station must be $B + 1$.

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int totalGas = 0;
        int totalCost = 0;
        int currentTank = 0;
        int start = 0;

        for (int i = 0; i < gas.length; i++) {
            totalGas += gas[i];
            totalCost += cost[i];
            currentTank += gas[i] - cost[i];

            // BUG 1: Off-by-one pointer reset (start = i instead of start = i + 1)
            // BUG 2: Condition check using <= 0 resets a valid empty balance
            if (currentTank <= 0) {
                start = i; 
                currentTank = 0;
            }
        }

        // BUG 3: Missing or inverted global feasibility check
        if (totalGas < totalCost) return -1;

        return start;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **`start = i` instead of `start = i + 1`**:
   - If the tank fails at index $i$, station $i$ cannot be the start. The next candidate is $i + 1$.
2. **Checking `<= 0` instead of `< 0`**:
   - Arriving at a station with exactly $0$ gas in the tank is completely valid because you immediately collect that station's `gas[i]`. Resetting on `0` discards valid starting points.
3. **Missing or inverted feasibility check**:
   - Must verify whether total surplus ($\sum (\text{gas} - \text{cost})$) is non-negative.

### Fixed Production Code

#### Java
```java
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int totalSurplus = 0;
        int currentTank = 0;
        int startStation = 0;

        for (int i = 0; i < gas.length; i++) {
            int net = gas[i] - cost[i];
            totalSurplus += net;
            currentTank += net;

            // FIX 1 & 2: Reset strictly when tank becomes negative (< 0) to i + 1
            if (currentTank < 0) {
                startStation = i + 1;
                currentTank = 0;
            }
        }

        // FIX 3: Global feasibility check
        return (totalSurplus < 0) ? -1 : startStation;
    }
}
```

#### C++
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int totalSurplus = 0;
        int currentTank = 0;
        int startStation = 0;
        int n = gas.size();

        for (int i = 0; i < n; i++) {
            int net = gas[i] - cost[i];
            totalSurplus += net;
            currentTank += net;

            if (currentTank < 0) {
                startStation = i + 1;
                currentTank = 0;
            }
        }

        return (totalSurplus < 0) ? -1 : startStation;
    }
};
```

### Dry Run: `gas = [1, 2, 3, 4, 5]`, `cost = [3, 4, 5, 1, 2]`

| Index ($i$) | `gas[i]` | `cost[i]` | Net (`gas - cost`) | `totalSurplus` | `currentTank` | Action ($< 0$?) | `startStation` |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 1 | 3 | -2 | -2 | -2 | Tank $< 0 \implies \text{reset}$ | **1** (tank = 0) |
| 1 | 2 | 4 | -2 | -4 | -2 | Tank $< 0 \implies \text{reset}$ | **2** (tank = 0) |
| 2 | 3 | 5 | -2 | -6 | -2 | Tank $< 0 \implies \text{reset}$ | **3** (tank = 0) |
| 3 | 4 | 1 | +3 | -3 | +3 | $3 \ge 0 \implies \text{continue}$ | 3 |
| 4 | 5 | 2 | +3 | **0** | **+6** | $6 \ge 0 \implies \text{continue}$ | 3 |

*After Loop*: `totalSurplus = 0 >= 0` $\implies$ **Returns `startStation = 3`**.

### Token-Saving Prompt (For Exam AI Assistant)
```text
Task: Find starting gas station index for circular tour in Java.
Approach: Single-pass Greedy. Track totalSurplus += gas[i] - cost[i] and currentTank += gas[i] - cost[i].
If currentTank < 0, reset startStation = i + 1 and currentTank = 0.
After the loop, if totalSurplus < 0 return -1, otherwise return startStation.
Complexity: Time O(N), Space O(1).
```

### Spot-It-Fast Rule
Look for `if (currentTank <= 0)` and `start = i`: change `<= 0` to `< 0`, and `start = i` to `start = i + 1`.

### Time & Space Complexity
- **Time**: $O(N)$ — Single pass through both arrays.
- **Space**: $O(1)$ — Constant memory.

---

## Problem 4: Merge Intervals ($O(N \log N)$ Sorting Greedy)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: merge-intervals](https://leetcode.com/problems/merge-intervals/)

### Overlap Condition
For two intervals sorted by start time: Interval 1 $[s_1, e_1]$ and Interval 2 $[s_2, e_2]$ with $s_1 \le s_2$:
$$\text{Overlap Condition}: \mathbf{s_2 \le e_1}$$
When overlapping: $[\text{New Start}, \text{New End}] = [s_1, \max(e_1, e_2)]$.

### The 4-Point Interval Checklist
1. **Sort by Start Time**: Ensure comparator sorts by $a[0]$ ascending, never $a[1]$.
2. **Inclusive Overlap**: Use $\le$ (`next[0] <= current[1]`) so touching intervals like $[1, 4]$ and $[4, 5]$ merge into $[1, 5]$.
3. **Max End Boundary**: Use $\max(e_1, e_2)$ so subsumed intervals like $[1, 8]$ and $[2, 4]$ do not shrink to end at $4$.
4. **Flush Trailing Interval**: Always add the final `current` interval after the loop terminates.

> Corrected: Note on in-place array modification: `current = intervals[0]` modifies the input array directly; standard for competitive assessments, but worth noting.

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
import java.util.*;

class Solution {
    public int[][] merge(int[][] intervals) {
        // BUG 1: Missing null or single-element guard clause

        // BUG 2: Sorting by end time (a[1] - b[1]) instead of start time (a[0] - b[0])
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

        List<int[]> merged = new ArrayList<>();
        int[] current = intervals[0];

        for (int i = 1; i < intervals.length; i++) {
            // BUG 3: Strict less-than (<) fails to merge touching intervals like [1,4] and [4,5]
            if (intervals[i][0] < current[1]) {
                // BUG 4: Overwrites end without Math.max(), shrinking subsumed intervals
                current[1] = intervals[i][1]; 
            } else {
                merged.add(current);
                current = intervals[i];
            }
        }

        // BUG 5: Forgetting to add the final 'current' interval after loop finishes!
        return merged.toArray(new int[merged.size()][]);
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Guard Clause**: `intervals.length == 0` causes `intervals[0]` to throw `ArrayIndexOutOfBoundsException`.
2. **Sorting by End Time `a[1] - b[1]`**: Breaks interval sequencing (e.g., `[[2,3], [4,5], [1,10]]` leaves `[1,10]` at the end, missing intermediate merges). Must sort by `a[0]`.
3. **Strict `<` instead of `<=`**: Touching intervals $[1, 4]$ and $[4, 5]$ share a boundary point ($4 \le 4$) and must merge.
4. **`current[1] = intervals[i][1]` without `Math.max`**: For $[1, 8]$ and $[2, 4]$, assigning $4$ corrupts the interval into $[1, 4]$.
5. **Missing Final `merged.add(current)`**: The last active interval is never appended to the result list.

### Fixed Production Code

#### Java
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals == null || intervals.length <= 1) { // FIX 1
            return intervals;
        }

        // FIX 2: Sort ascending by start time
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> merged = new ArrayList<>();
        int[] current = intervals[0];

        for (int i = 1; i < intervals.length; i++) {
            // FIX 3: <= ensures touching intervals merge
            if (intervals[i][0] <= current[1]) {
                // FIX 4: Maintain maximum end boundary
                current[1] = Math.max(current[1], intervals[i][1]);
            } else {
                merged.add(current);
                current = intervals[i];
            }
        }

        // FIX 5: Append the trailing interval
        merged.add(current);

        return merged.toArray(new int[merged.size()][]);
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
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.size() <= 1) return intervals;

        // Default C++ sort sorts pairs/vectors by first element ascending
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> merged;
        vector<int> current = intervals[0];

        for (size_t i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] <= current[1]) {
                current[1] = max(current[1], intervals[i][1]);
            } else {
                merged.push_back(current);
                current = intervals[i];
            }
        }
        merged.push_back(current);

        return merged;
    }
};
```

### Dry Run: `intervals = [[1, 3], [8, 10], [2, 6], [15, 18]]`
Sorted by Start Time: `[[1, 3], [2, 6], [8, 10], [15, 18]]`. Initial `current = [1, 3]`.

| Step / Index $i$ | Next Interval | Check: $s_2 \le e_1$? | Action | `current` State | `merged` List |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **Start** | — | — | — | `[1, 3]` | `[]` |
| **$i = 1$** | `[2, 6]` | $2 \le 3$ (True) | Merge: $\max(3, 6) = 6$ | `[1, 6]` | `[]` |
| **$i = 2$** | `[8, 10]` | $8 \le 6$ (False) | Flush `current`, reset `current` | `[8, 10]` | `[[1, 6]]` |
| **$i = 3$** | `[15, 18]` | $15 \le 10$ (False) | Flush `current`, reset `current` | `[15, 18]` | `[[1, 6], [8, 10]]` |
| **After Loop** | — | — | Flush trailing `current` | — | `[[1, 6], [8, 10], [15, 18]]` |

*Final Output*: `[[1, 6], [8, 10], [15, 18]]`.

### Spot-It-Fast Rule
Look for the comparator (`a[0]` vs `a[1]`), the overlap operator (`<=` vs `<`), and check if `merged.add(current);` is present after the loop exits.

### Time & Space Complexity
- **Time**: $O(N \log N)$ — Dominated by interval sorting.
- **Space**: $O(N)$ — To store merged intervals output.
