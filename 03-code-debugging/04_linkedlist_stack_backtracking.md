# Linked List, Stack & Backtracking Debugging

---

## Problem 1: Linked List Cycle Detection (Floyd's Tortoise & Hare)

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: linked-list-cycle](https://leetcode.com/problems/linked-list-cycle/)

### Problem Statement
Given `head`, the head of a singly linked list, determine if the linked list has a cycle in it. There is a cycle if some node can be reached again by continuously following the `next` pointer. Return `true` if there is a cycle, otherwise `false`.

### Sample Input & Output
- **Input**: `head = [3, 2, 0, -4]`, pos = 1 (tail connects to 2nd node) $\implies$ **Output**: `true`
- **Input**: `head = [1]`, pos = -1 $\implies$ **Output**: `false`

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; next = null; }
}

public class CycleDebugger {
    public boolean hasCycle(ListNode head) {
        // BUG 1: Missing empty or single-node boundary guard check
        ListNode slow = head;
        ListNode fast = head;

        // BUG 2: NullPointerException on fast.next.next dereference when fast.next is null
        while (fast != null) { 
            slow = slow.next;
            fast = fast.next.next; // Crashes if fast.next is null!

            // BUG 3: Object value (.val) comparison instead of reference identity (==)
            if (slow.val == fast.val) { 
                return true;
            }
        }
        return false;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Missing Guard Clause (`head == null || head.next == null`)**:
   - Single-element or empty lists cannot contain a cycle. Calling `fast.next.next` on a single node without checks can lead to crashes.
2. **`while (fast != null)` without `fast.next != null`**:
   - In an odd-length acyclic list, `fast` lands on the last node (`fast.next == null`). Dereferencing `fast.next.next` throws a runtime `NullPointerException`.
3. **Comparing `.val` instead of reference identity `==`**:
   - Two distinct nodes in a linked list can hold identical data values (e.g., node A has `val = 5` and node B has `val = 5`). A cycle occurs if and only if both pointers point to the identical memory address (`slow == fast`).

### Fixed Production Code

#### Java
```java
public class CycleDebugger {
    public boolean hasCycle(ListNode head) {
        // FIX 1: Guard clause for empty or single-node acyclic lists
        if (head == null || head.next == null) {
            return false;
        }

        ListNode slow = head;
        ListNode fast = head;

        // FIX 2: Check both fast and fast.next are non-null
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            // FIX 3: Compare node memory references, not values
            if (slow == fast) {
                return true;
            }
        }

        return false;
    }
}
```

#### C++
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head || !head->next) return false;

        ListNode *slow = head;
        ListNode *fast = head;

        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
};
```

### In-Place Linked List Reversal Note & Pointer Order
**Tag**: [ADDED]  
**Practice Link**: [LeetCode: reverse-linked-list](https://leetcode.com/problems/reverse-linked-list/)  
When reversing a singly linked list in-place ($O(1)$ space), pointer operations must follow a strict 4-step sequence inside `while (curr != null)`: save next node, point backwards, shift previous, shift current.
```java
ListNode nextTemp = curr.next; // 1. Save future node
curr.next = prev;              // 2. Reverse link
prev = curr;                   // 3. Advance prev
curr = nextTemp;               // 4. Advance curr
```
*Tiny Example*: Reversing `1 -> 2 -> null` initializes `prev = null, curr = 1`. Iteration 1 reverses `1 -> null` (`prev = 1, curr = 2`). Iteration 2 reverses `2 -> 1 -> null` (`prev = 2, curr = null`). Returns `prev = 2`. Inverting step 2 and 3 severs the list and causes an infinite loop or lost nodes.

### Dry Run (Cyclic List $1 \to 2 \to 3 \to 2$)
List: Node 1 $\to$ Node 2 $\to$ Node 3 $\to$ Node 2.

| Step | `slow` Node | `fast` Node | `slow == fast`? | Action |
| :---: | :---: | :---: | :---: | :---: |
| Start | Node 1 | Node 1 | Yes (at start) | Loop initiates |
| 1 | Node 2 | Node 3 | No | Loop continues |
| 2 | Node 3 | Node 3 (via $2 \to 3$) | **Yes (Match!)** | **Returns `true`** |

### Spot-It-Fast Rule
Check the loop condition: `while (fast != null && fast.next != null)`. If `fast.next != null` is missing, or if `slow.val == fast.val` is used instead of `slow == fast`, fix them immediately.

### Time & Space Complexity
- **Time**: $O(N)$ — In an acyclic list, `fast` reaches the end in $N/2$ steps. In a cycle, `fast` catches `slow` within 1 full loop cycle.
- **Space**: $O(1)$ — Two pointers only.

---

## Problem 2: Next Greater Element (Monotonic Decreasing Stack)

**Tag**: [CHAT]

### Problem Statement
Given an array `arr` of $n$ integers, find the Next Greater Element (NGE) for each element in the array. The Next Greater Element for an element `x` is the first greater element on the right side of `x` in the array. If no greater element exists to the right, output `-1`.

### Sample Input & Output
- **Input**: `arr = [4, 5, 2, 25]` $\implies$ **Output**: `[5, 25, 25, -1]`
- **Input**: `arr = [13, 7, 6, 12]` $\implies$ **Output**: `[-1, 12, 12, -1]`

### Buggy Exam Code
```cpp
// BUGGY CODE PROVIDED IN EXAM PORTAL
#include <vector>
#include <stack>
using namespace std;

vector<int> nextGreaterElement(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n);
    stack<int> st;

    // BUG 1: Traversing forward without maintaining unresolved indices
    for (int i = 0; i < n; i++) {
        
        // BUG 2: Dereferencing st.top() without !st.empty() check (crashes on empty stack)
        while (st.top() <= arr[i]) {
            st.pop();
        }

        // BUG 3: Storing st.top() directly without fallback -1 when stack becomes empty
        result[i] = st.top();
        st.push(arr[i]);
    }
    return result;
}
```

### Bugs Found & Why They Are Wrong
1. **Forward Traversal Without Index Tracking**:
   - When iterating $0 \to n - 1$, the stack holds elements to the *left*, but the problem requires elements to the *right*. Traversing backwards ($n - 1 \to 0$) naturally makes right-hand candidates available.
2. **`while (st.top() <= arr[i])` Without Empty Check**:
   - On the first element or whenever all elements are smaller, the stack becomes empty. Calling `st.top()` on an empty stack crashes with a segmentation fault.
3. **No `-1` Default Fallback**:
   - If no element to the right is larger, the result must be `-1`. Assigning `st.top()` on empty triggers a crash.

### Fixed Production Code

#### C++
```cpp
#include <vector>
#include <stack>
using namespace std;

vector<int> nextGreaterElement(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n);
    stack<int> st;

    // FIX 1: Traverse backward from right to left
    for (int i = n - 1; i >= 0; i--) {
        // FIX 2: Guard st.top() with !st.empty()
        while (!st.empty() && st.top() <= arr[i]) {
            st.pop();
        }

        // FIX 3: Fallback to -1 if stack is empty
        result[i] = st.empty() ? -1 : st.top();

        // Push current element for upcoming elements to the left
        st.push(arr[i]);
    }

    return result;
}
```

#### Java
```java
import java.util.*;

public class Solution {
    public int[] nextGreaterElement(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        Deque<Integer> st = new ArrayDeque<>();

        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && st.peek() <= arr[i]) {
                st.pop();
            }

            result[i] = st.isEmpty() ? -1 : st.peek();
            st.push(arr[i]);
        }

        return result;
    }
}
```

### Valid Parentheses Empty-Stack Guard Note
**Tag**: [ADDED]  
**Practice Link**: [LeetCode: valid-parentheses](https://leetcode.com/problems/valid-parentheses/)  
A classic Capgemini stack debugging bug occurs when encountering closing brackets:
```java
// BUG: Calling st.pop() when stack is empty crashes with EmptyStackException on strings like "]"
char ch = s.charAt(i);
if (ch == ')' || ch == '}' || ch == ']') {
    if (st.isEmpty()) return false; // FIX: Verify non-empty before pop()
    char top = st.pop();
    if ((ch == ')' && top != '(') || (ch == '}' && top != '{') || (ch == ']' && top != '[')) return false;
}
```
*Rule*: Every `pop()` or `peek()` call must be preceded by an `isEmpty()` check.

### Dry Run: `arr = [4, 5, 2, 25]` ($n = 4$, Iterating $i = 3 \dots 0$)

| Index $i$ | `arr[i]` | Stack State Before Popping | Pop Action (`<= arr[i]`) | Stack State After Pop | `result[i]` (`empty ? -1 : top`) | Push to Stack |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **3** | 25 | `[]` | None | `[]` | **-1** | `[25]` |
| **2** | 2 | `[25]` | None ($25 > 2$) | `[25]` | **25** | `[2, 25]` |
| **1** | 5 | `[2, 25]` | Pop 2 ($2 \le 5$) | `[25]` | **25** | `[5, 25]` |
| **0** | 4 | `[5, 25]` | None ($5 > 4$) | `[5, 25]` | **5** | `[4, 5, 25]` |

*Final Result*: `result = [5, 25, 25, -1]`.

### Spot-It-Fast Rule
Look at the loop direction: if iterating forward without index storage, change it to backwards (`n - 1 down to 0`). Verify every `st.top()` or `st.pop()` has `!st.empty()`.

### Time & Space Complexity
- **Time**: $O(N)$ — Each element is pushed and popped at most once.
- **Space**: $O(N)$ — To hold elements in stack.

---

## Problem 3: Subsets Backtracking & State Restoration

**Tag**: [CHAT]  
**Practice Link**: [LeetCode: subsets](https://leetcode.com/problems/subsets/)

### Problem Statement
Given an integer array `nums` of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets.

### Sample Input & Output
- **Input**: `nums = [1, 2, 3]`
- **Output**: `[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]`

### Buggy Exam Code
```java
// BUGGY CODE PROVIDED IN EXAM PORTAL
import java.util.*;

public class SubsetDebugger {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> current = new ArrayList<>();
        backtrack(nums, 0, current, result);
        return result;
    }

    private void backtrack(int[] nums, int index, List<Integer> current, List<List<Integer>> result) {
        // BUG 1: Adding reference to 'current' instead of a snapshot copy!
        result.add(current); 

        for (int i = index; i < nums.length; i++) {
            current.add(nums[i]);

            // BUG 2: Passing 'index + 1' instead of 'i + 1' (Infinite Loop / Duplicates)
            backtrack(nums, index + 1, current, result);

            // BUG 3: Missing backtracking state undo step!
            // current.remove(current.size() - 1); is missing!
        }
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Reference Insertion Trap (`result.add(current)`)**:
   - In Java, `current` is an object reference. Adding `current` directly adds a pointer to the single mutable list. When backtracking clears the list, all entries in `result` become empty lists `[[], [], []]`. Must add a snapshot copy: `result.add(new ArrayList<>(current))`.
2. **Passing `index + 1` instead of `i + 1`**:
   - The loop variable is `i`. Passing `index + 1` causes the recursive branch to repeatedly re-add earlier elements from the current loop, producing infinite loops or duplicate combinations.
3. **Missing Backtrack Undo (`current.remove(current.size() - 1)`)**:
   - Backtracking requires restoring the state before testing the next branch. Failing to pop the last element leaves elements permanently in the list, corrupting sibling recursive paths.

### Fixed Production Code

#### Java
```java
import java.util.*;

public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> current = new ArrayList<>();
        backtrack(nums, 0, current, result);
        return result;
    }

    private void backtrack(int[] nums, int start, List<Integer> current, List<List<Integer>> result) {
        // FIX 1: Add a deep snapshot copy of the current state
        result.add(new ArrayList<>(current));

        for (int i = start; i < nums.length; i++) {
            current.add(nums[i]);

            // FIX 2: Advance to next element using loop variable i + 1
            backtrack(nums, i + 1, current, result);

            // FIX 3: Backtrack state undo: pop the last added element
            current.remove(current.size() - 1);
        }
    }
}
```

#### C++
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> current;
        backtrack(nums, 0, current, result);
        return result;
    }

private:
    void backtrack(vector<int>& nums, int start, vector<int>& current, vector<vector<int>>& result) {
        // In C++, push_back creates an independent copy
        result.push_back(current);

        for (int i = start; i < nums.size(); i++) {
            current.push_back(nums[i]);
            backtrack(nums, i + 1, current, result);
            current.pop_back(); // Undo state
        }
    }
};
```

### N-Queens "Undo the Placement" Note
**Tag**: [ADDED]  
**Practice Link**: [LeetCode: n-queens](https://leetcode.com/problems/n-queens/)  
In N-Queens backtracking, placing a Queen on row $r$ and col $c$ marks column and diagonal sets: `cols.add(c); diag1.add(r - c); diag2.add(r + c);`.  
A standard bug is forgetting to undo all three tracking structures upon recursive return.  
To undo placement: `board[r][c] = '.'; cols.remove(c); diag1.remove(r - c); diag2.remove(r + c);`.

### Dry Run: `nums = [1, 2]`

| Step | Action | `current` List | `result` Snapshot Added | Undo Action |
| :---: | :---: | :---: | :---: | :---: |
| 1 | Start at `start = 0` | `[]` | `[[]]` | — |
| 2 | Loop $i = 0$: add 1 $\to$ backtrack `start = 1` | `[1]` | `[[], [1]]` | — |
| 3 | Loop $i = 1$: add 2 $\to$ backtrack `start = 2` | `[1, 2]` | `[[], [1], [1, 2]]` | — |
| 4 | Return from `start = 2`, loop ends | `[1, 2]` | — | Remove 2 $\to$ `[1]` |
| 5 | Return from $i = 0$, loop advances to $i = 1$ | `[1]` | — | Remove 1 $\to$ `[]` |
| 6 | Loop $i = 1$: add 2 $\to$ backtrack `start = 2` | `[2]` | `[[], [1], [1, 2], [2]]` | — |
| 7 | Return from `start = 2`, remove 2 | `[2]` | — | Remove 2 $\to$ `[]` |

*Final Result*: `[[], [1], [1, 2], [2]]`.

### Spot-It-Fast Rule
Look inside the recursive helper: if `result.add(current)` is used without `new ArrayList<>(current)`, or `backtrack(..., index + 1, ...)` uses `index` instead of `i`, or `current.remove(current.size() - 1);` is missing, fix them immediately.

### Time & Space Complexity
- **Time**: $O(N \times 2^N)$ — Generating all $2^N$ subsets, each taking $O(N)$ copy time.
- **Space**: $O(N)$ — Recursion call stack depth bounded by $N$.
