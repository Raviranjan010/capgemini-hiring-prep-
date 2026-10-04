[Home](../README.md) > [04-code-debugging](README.md) > 03_greedy_and_intervals.md

# 03. Greedy & Interval Algorithms Debugging

This module covers reachability calculations, greedy boundary expansions, circular accumulation resets, and interval overlaps.

## Problem 1 (DBG-007): Jump Game I (Reachable Check)
**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Greedy
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO)

### Problem Statement
You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position. Return `true` if you can reach the last index, or `false` otherwise.

### Sample Input & Output
- **Input**: `nums = [2, 3, 1, 1, 4]` -> `true`
- **Input**: `nums = [3, 2, 1, 0, 4]` -> `false`

### Buggy Exam Code
```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) { // Bug 1: wrong condition check in some exam variants
                return false;
            }
            maxReach = Math.max(maxReach, nums[i]); // Bug 2: misses adding current index i!
        }
        return true;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Index Offset in Reach Formula**: `nums[i]` is only the jump length, not the destination index. The reachable position from index $i$ is `i + nums[i]`. Using only `nums[i]` fails completely on arrays where jump lengths are smaller than indices.

### Fixed Code
```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) return false;
            maxReach = Math.max(maxReach, i + nums[i]);
            if (maxReach >= nums.length - 1) return true;
        }
        return true;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    bool canJump(const std::vector<int>& nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i > maxReach) return false;
            maxReach = std::max(maxReach, i + nums[i]);
            if (maxReach >= (int)nums.size() - 1) return true;
        }
        return true;
    }
};
```
</details>

### Dry-Run Table
| Index `i` | Value `nums[i]` | Reachable From Here (`i + nums[i]`) | Updated `maxReach` | Condition `i > maxReach` |
| :---: | :---: | :---: | :---: | :---: |
| 0 | 2 | 2 | 2 | False |
| 1 | 3 | 4 | 4 ($4 \ge 4$ -> Early Return!) | False |

### Spot-It-Fast Rule
In Jump Game, the reach update is always `Math.max(maxReach, i + nums[i])`. If `i` is missing, it is an instant bug.

### Edge Cases
1. Single element `[0]`: Already at destination; loop terminates returning `true`.
2. Immediate trap `[0, 2, 3]`: At `i = 1`, `1 > 0` triggers `return false`.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 2 (DBG-008): Jump Game II (Minimum Jumps Count)
**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Greedy
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO)

### Problem Statement
Given a 0-indexed array of integers `nums` of length `n`, return the minimum number of jumps to reach `nums[n - 1]`.

### Sample Input & Output
- **Input**: `nums = [2, 3, 1, 1, 4]` -> `2`

### Buggy Exam Code
```java
class Solution {
    public int jump(int[] nums) {
        int jumps = 0, curEnd = 0, curFarthest = 0;
        for (int i = 0; i < nums.length; i++) { // Bug 1: loops to n instead of n - 1
            curFarthest = Math.max(curFarthest, i + nums[i]);
            if (i == curEnd) {
                jumps++;
                curEnd = curFarthest;
            }
        }
        return jumps;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Looping to Last Index (`n` instead of `n - 1`)**: When `i` reaches `nums.length - 1`, we have already arrived at the destination. Incrementing `jumps++` at the final index counts an extra unnecessary jump.

### Fixed Code
```java
class Solution {
    public int jump(int[] nums) {
        int jumps = 0, curEnd = 0, curFarthest = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            curFarthest = Math.max(curFarthest, i + nums[i]);
            if (i == curEnd) {
                jumps++;
                curEnd = curFarthest;
            }
        }
        return jumps;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int jump(const std::vector<int>& nums) {
        int jumps = 0, curEnd = 0, curFarthest = 0;
        for (int i = 0; i < (int)nums.size() - 1; i++) {
            curFarthest = std::max(curFarthest, i + nums[i]);
            if (i == curEnd) {
                jumps++;
                curEnd = curFarthest;
            }
        }
        return jumps;
    }
};
```
</details>

### Dry-Run Table
| Index `i` | Value | `curFarthest` | `i == curEnd` | `jumps` | `curEnd` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 2 | 2 | True | 1 | 2 |
| 1 | 3 | 4 | False | 1 | 2 |
| 2 | 1 | 4 | True | 2 | 4 |

### Spot-It-Fast Rule
Jump Game II loop MUST iterate to `i < nums.length - 1`. Reaching the final index must never trigger a jump.

### Edge Cases
1. Array of size 1 `[0]`: Loop runs 0 times; returns `0` jumps.
2. Direct jump `[5, 1, 1, 1]`: Jump 1 immediately reaches index 5; returns 1.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 3 (DBG-009): Gas Station Circular Tour ($O(N)$ Greedy)
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Greedy
**Source**: Added practice

### Problem Statement
There are $n$ gas stations along a circular route. You are given two integer arrays `gas` and `cost`. Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return `-1`.

### Sample Input & Output
- **Input**: `gas = [1,2,3,4,5]`, `cost = [3,4,5,1,2]` -> `3`

### Buggy Exam Code
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int totalGas = 0, totalCost = 0;
        int currentTank = 0, startStation = 0;
        
        for (int i = 0; i < gas.length; i++) {
            totalGas += gas[i];
            totalCost += cost[i];
            currentTank += gas[i] - cost[i];
            
            if (currentTank <= 0) { // Bug 1: resets on 0 instead of < 0
                startStation = i;   // Bug 2: sets start to i instead of i + 1
                currentTank = 0;
            }
        }
        return totalGas >= totalCost ? startStation : -1;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Resetting on Zero Tank (`<= 0`)**: Running out of gas means `currentTank < 0`. Having exactly 0 gas is valid if the car can still refuel at station $i$.
2. **Wrong Station Reset Index (`startStation = i`)**: If tank fails travelling from station $i$, station $i$ cannot be the start. The candidate start must be `i + 1`.

### Fixed Code
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int totalGas = 0, totalCost = 0;
        int currentTank = 0, startStation = 0;
        
        for (int i = 0; i < gas.length; i++) {
            totalGas += gas[i];
            totalCost += cost[i];
            currentTank += gas[i] - cost[i];
            
            if (currentTank < 0) {
                startStation = i + 1;
                currentTank = 0;
            }
        }
        return totalGas >= totalCost ? startStation : -1;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int canCompleteCircuit(const std::vector<int>& gas, const std::vector<int>& cost) {
        int totalGas = 0, totalCost = 0;
        int currentTank = 0, startStation = 0;
        for (int i = 0; i < (int)gas.size(); i++) {
            totalGas += gas[i];
            totalCost += cost[i];
            currentTank += gas[i] - cost[i];
            if (currentTank < 0) {
                startStation = i + 1;
                currentTank = 0;
            }
        }
        return totalGas >= totalCost ? startStation : -1;
    }
};
```
</details>

### Dry-Run Table
| Station `i` | `gas[i]` | `cost[i]` | Net `gas - cost` | `currentTank` | Action |
| :---: | :---: | :---: | :---: | :---: | :--- |
| 0 | 1 | 3 | -2 | -2 | Reset `start = 1`, `tank = 0` |
| 1 | 2 | 4 | -2 | -2 | Reset `start = 2`, `tank = 0` |
| 2 | 3 | 5 | -2 | -2 | Reset `start = 3`, `tank = 0` |
| 3 | 4 | 1 | +3 | +3 | `tank >= 0` -> continue |
| 4 | 5 | 2 | +3 | +6 | `tank >= 0` -> continue |

### Spot-It-Fast Rule
If total gas $\ge$ total cost, a solution is guaranteed to exist. Resetting the start point MUST advance to `i + 1` when `currentTank < 0`.

### Edge Cases
1. `totalGas < totalCost`: Impossible to complete circle; returns `-1`.
2. Single station `[2]`, `[2]`: Returns `0`.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 4 (DBG-010): Merge Overlapping Intervals ($O(N \log N)$)
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Intervals
**Source**: Added practice

### Problem Statement
Given an array of `intervals` where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return an array of the non-overlapping intervals.

### Sample Input & Output
- **Input**: `intervals = [[1,3],[2,6],[8,10],[15,18]]`
- **Output**: `[[1,6],[8,10],[15,18]]`

### Buggy Exam Code
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1])); // Bug 1: sorts by END instead of START
        List<int[]> merged = new ArrayList<>();
        
        for (int[] interval : intervals) {
            if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0]) {
                merged.add(interval);
            } else {
                merged.get(merged.size() - 1)[1] = interval[1]; // Bug 2: misses Math.max
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Sorting by End Time**: Greedy merging requires intervals sorted by **start time** `a[0]`. Sorting by end time allows an interval with a very early start to appear later in the iteration, failing to merge.
2. **Direct End Assignment**: Setting end time directly to `interval[1]` instead of `Math.max(lastEnd, interval[1])` fails when an interval is completely engulfed by a previous longer interval (e.g., `[1, 5]` and `[2, 3]`).

### Fixed Code
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) return intervals;
        
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();
        
        for (int[] interval : intervals) {
            if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0]) {
                merged.add(interval);
            } else {
                int[] last = merged.get(merged.size() - 1);
                last[1] = Math.max(last[1], interval[1]);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<std::vector<int>> merge(std::vector<std::vector<int>>& intervals) {
        if (intervals.size() <= 1) return intervals;
        std::sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
            return a[0] < b[0];
        });
        std::vector<std::vector<int>> merged;
        for (const auto& interval : intervals) {
            if (merged.empty() || merged.back()[1] < interval[0]) {
                merged.push_back(interval);
            } else {
                merged.back()[1] = std::max(merged.back()[1], interval[1]);
            }
        }
        return merged;
    }
};
```
</details>

### Dry-Run Table
| Interval | Last Merged End | Overlap Check (`lastEnd >= start`) | New Merged End (`Math.max`) |
| :---: | :---: | :---: | :---: |
| `[1, 3]` | None | New interval added | `[1, 3]` |
| `[2, 6]` | 3 | $3 \ge 2$ (Overlap!) | `Math.max(3, 6) = 6` |
| `[8, 10]` | 6 | $6 < 8$ (No overlap) | New interval `[8, 10]` added |
| `[15, 18]` | 10 | $10 < 15$ (No overlap) | New interval `[15, 18]` added |

### Spot-It-Fast Rule
Interval merging: Sort by `a[0]`, update end with `Math.max(last[1], interval[1])`.

### Edge Cases
1. Fully engulfed interval `[[1, 10], [2, 3]]`: Retains end `10`.
2. Touching boundaries `[[1, 4], [4, 5]]`: $4 \ge 4$ merges into `[1, 5]`.

- **Complexity**: Time: $O(N \log N)$, Space: $O(N)$.

---

## Problem 5 (DBG-028): Non-Overlapping Intervals Count
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Intervals
**Source**: Added practice

### Problem Statement
Given an array of intervals `intervals`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

### Sample Input & Output
- **Input**: `intervals = [[1,2],[2,3],[3,4],[1,3]]` -> `1` (remove `[1,3]`)

### Buggy Exam Code
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0])); // Bug 1: sorts by start instead of end
        int removals = 0;
        int prevEnd = intervals[0][1];
        
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < prevEnd) {
                removals++;
                prevEnd = intervals[i][1]; // Bug 2: keeps the wrong interval
            } else {
                prevEnd = intervals[i][1];
            }
        }
        return removals;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Sorting by Start Time**: In interval scheduling, to maximize non-overlapping intervals, you must sort by **end time** so that intervals finishing earliest leave the most room for subsequent intervals.
2. **Greedy Retention of Wrong Interval**: When two intervals overlap, the one with the larger end time should be removed.

### Fixed Code
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length <= 1) return 0;
        
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
        int nonOverlapping = 1;
        int prevEnd = intervals[0][1];
        
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= prevEnd) {
                nonOverlapping++;
                prevEnd = intervals[i][1];
            }
        }
        return intervals.length - nonOverlapping;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int eraseOverlapIntervals(std::vector<std::vector<int>>& intervals) {
        if (intervals.size() <= 1) return 0;
        std::sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
            return a[1] < b[1];
        });
        int nonOverlapping = 1;
        int prevEnd = intervals[0][1];
        for (size_t i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] >= prevEnd) {
                nonOverlapping++;
                prevEnd = intervals[i][1];
            }
        }
        return intervals.size() - nonOverlapping;
    }
};
```
</details>

### Dry-Run Table
| Sorted Interval | `prevEnd` | Start $\ge$ `prevEnd` | Retained Count |
| :---: | :---: | :---: | :---: |
| `[1, 2]` | 2 | Base | 1 |
| `[2, 3]` | 3 | $2 \ge 2$ -> True | 2 |
| `[1, 3]` | 3 | $1 \ge 3$ -> False (Skipped) | 2 |
| `[3, 4]` | 4 | $3 \ge 3$ -> True | 3 |

### Spot-It-Fast Rule
Interval scheduling maximization: Sort by `end` time (`a[1]`). Count non-overlapping, then `removals = total - nonOverlapping`.

### Edge Cases
1. Empty or single interval: Returns `0`.
2. All intervals identical: Returns `N - 1`.

- **Complexity**: Time: $O(N \log N)$, Space: $O(1)$.

---

## Problem 6 (DBG-029): Assign Cookies Greedy Sorting Bug
**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Two-Pointer Greedy
**Source**: Added practice

### Problem Statement
Assume you are an awesome parent and want to give your children some cookies. Each child $i$ has a greed factor $g[i]$, and each cookie $j$ has a size $s[j]$. If $s[j] \ge g[i]$, we can assign cookie $j$ to child $i$. Maximize the number of satisfied children.

### Sample Input & Output
- **Input**: `g = [1, 2, 3]`, `s = [1, 1]` -> `1`

### Buggy Exam Code
```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        // Bug 1: forgets to sort the arrays!
        int child = 0, cookie = 0;
        while (child < g.length && cookie < s.length) {
            if (s[cookie] >= g[child]) {
                child++;
            }
            cookie++;
        }
        return child;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Sorting Step**: Greedy pairing requires matching the least greedy children with the smallest possible satisfying cookies. Without sorting `g` and `s`, a small cookie might be compared against a high-greed child and wasted.

### Fixed Code
```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        
        int child = 0, cookie = 0;
        while (child < g.length && cookie < s.length) {
            if (s[cookie] >= g[child]) {
                child++;
            }
            cookie++;
        }
        return child;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    int findContentChildren(std::vector<int>& g, std::vector<int>& s) {
        std::sort(g.begin(), g.end());
        std::sort(s.begin(), s.end());
        int child = 0, cookie = 0;
        while (child < (int)g.size() && cookie < (int)s.size()) {
            if (s[cookie] >= g[child]) {
                child++;
            }
            cookie++;
        }
        return child;
    }
};
```
</details>

### Dry-Run Table
| Cookie Size `s[j]` | Child Greed `g[i]` | Satisfied? | Next Pointer |
| :---: | :---: | :---: | :---: |
| 1 | 1 | Yes ($1 \ge 1$) | `child=1, cookie=1` |
| 1 | 2 | No ($1 < 2$) | `child=1, cookie=2` |

### Spot-It-Fast Rule
Greedy bipartite matching algorithms require sorting both arrays before applying two pointers.

### Edge Cases
1. Empty cookie array `s = []`: Loop does not run, returns `0`.
2. All cookies smaller than all greed values: Returns `0`.

- **Complexity**: Time: $O(N \log N + M \log M)$, Space: $O(1)$.

---

## Problem 7 (DBG-030): Partition Labels String Intervals Bug
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Greedy Intervals
**Source**: Added practice

### Problem Statement
You are given a string `s`. Partition the string into as many parts as possible so that each letter appears in at most one part. Return a list of integers representing the size of these parts.

### Sample Input & Output
- **Input**: `s = "ababcbacadefegdehijhklij"`
- **Output**: `[9, 7, 8]`

### Buggy Exam Code
```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        List<Integer> res = new ArrayList<>();
        int[] last = new int[26];
        for (int i = 0; i < s.length(); i++) {
            last[s.charAt(i) - 'a'] = i;
        }
        
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            end = Math.max(end, last[s.charAt(i) - 'a']);
            if (i == end) {
                res.add(end - start); // Bug: off-by-one length!
                start = i;            // Bug: start should be i + 1
            }
        }
        return res;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Off-by-One Substring Length**: The length of interval `[start, end]` inclusive is `end - start + 1`. Using `end - start` undercounts by 1.
2. **Wrong Next Start Index**: The next partition starts at `i + 1`. Setting `start = i` includes index `i` in both partitions.

### Fixed Code
```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        List<Integer> res = new ArrayList<>();
        int[] last = new int[26];
        for (int i = 0; i < s.length(); i++) {
            last[s.charAt(i) - 'a'] = i;
        }
        
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            end = Math.max(end, last[s.charAt(i) - 'a']);
            if (i == end) {
                res.add(end - start + 1);
                start = i + 1;
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
    std::vector<int> partitionLabels(const std::string& s) {
        std::vector<int> res;
        std::vector<int> last(26, 0);
        for (int i = 0; i < (int)s.length(); i++) {
            last[s[i] - 'a'] = i;
        }
        int start = 0, end = 0;
        for (int i = 0; i < (int)s.length(); i++) {
            end = std::max(end, last[s[i] - 'a']);
            if (i == end) {
                res.push_back(end - start + 1);
                start = i + 1;
            }
        }
        return res;
    }
};
```
</details>

### Dry-Run Table
| Characters Seen | Running `end` Tracker | Current Index `i` | Partition Closed? | Segment Length |
| :--- | :---: | :---: | :---: | :---: |
| `"ababcbaca"` | 8 | 8 | Yes (`8 == 8`) | `8 - 0 + 1 = 9` |
| `"defegde"` | 15 | 15 | Yes (`15 == 15`) | `15 - 9 + 1 = 7` |
| `"hijhklij"` | 23 | 23 | Yes (`23 == 23`) | `23 - 16 + 1 = 8` |

### Spot-It-Fast Rule
Inclusive interval length is always `end - start + 1`. Next start point must advance to `i + 1`.

### Edge Cases
1. String with all identical characters `"aaaa"`: Returns `[4]`.
2. All distinct characters `"abcd"`: Returns `[1, 1, 1, 1]`.

- **Complexity**: Time: $O(N)$, Space: $O(1)$ (26-element array).

---

Previous: [02_matrix.md](02_matrix.md) | Next: [04_linkedlist_stack_backtracking.md](04_linkedlist_stack_backtracking.md)
