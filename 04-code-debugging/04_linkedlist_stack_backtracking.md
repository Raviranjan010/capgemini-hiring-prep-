[Home](../README.md) > [04-code-debugging](README.md) > 04_linkedlist_stack_backtracking.md

# 04. Linked Lists, Stacks & Backtracking Debugging

This module covers pointer manipulation, cycle guards, monotonic stack underflows, and backtracking state restoration.

## Problem 1 (DBG-011): Linked List Cycle Detection (Floyd's Tortoise & Hare)
**Tag**: [VIDEO] | **Difficulty**: Easy | **Topic**: Linked Lists
**Source**: [YouTube Reference](https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO)

### Problem Statement
Given `head`, the head of a linked list, determine if the linked list has a cycle in it in $O(1)$ memory.

### Sample Input & Output
- **Input**: `head = [3, 2, 0, -4]`, pos = 1 (points to node with value 2) -> `true`
- **Input**: `head = [1]`, no cycle -> `false`

### Buggy Exam Code
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head.next; // Bug 1: Throws NPE if head is null!
        
        while (slow != fast) {
            if (fast == null) {    // Bug 2: Fails to check fast.next != null!
                return false;
            }
            slow = slow.next;
            fast = fast.next.next; // Throws NPE when fast.next is null
        }
        return true;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Unchecked Null Pointer Dereference**: If `head == null`, executing `head.next` immediately crashes with a `NullPointerException`.
2. **Missing `fast.next != null` Guard**: `fast` advances by 2 steps (`fast.next.next`). Checking only `fast == null` allows `fast.next` to be null, causing `fast.next.next` to throw an NPE.
3. **Comparing Node Values instead of Pointers**: (In some student exam variants): `slow.val == fast.val` flags false cycles when distinct nodes happen to have the same integer data.

### Fixed Code
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
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
    bool hasCycle(ListNode *head) {
        if (!head || !head->next) return false;
        ListNode *slow = head;
        ListNode *fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) return true;
        }
        return false;
    }
};
```
</details>

### Dry-Run Table
| Iteration | Slow Pointer | Fast Pointer | `slow == fast` |
| :---: | :---: | :---: | :---: |
| 0 | Node 3 | Node 3 | Start |
| 1 | Node 2 | Node 0 | False |
| 2 | Node 0 | Node 2 | False |
| 3 | Node -4 | Node -4 | True (Cycle confirmed!) |

### Spot-It-Fast Rule
Floyd's cycle detection loop guard MUST evaluate both `fast != null && fast.next != null`. Compare object references (`slow == fast`), never `.val`.

### Edge Cases
1. `head == null`: Handled cleanly by base condition; returns `false`.
2. Single-node list without cycle `[1]`: Returns `false`.
3. Two-node list with cycle: Meets at node 2 on iteration 1; returns `true`.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 2 (DBG-012): Next Greater Element (Monotonic Stack)
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Monotonic Stack
**Source**: Added practice

### Problem Statement
Given an array `arr`, find the Next Greater Element (NGE) for every element. The NGE for an element $x$ is the first greater element to the right. If none exists, output `-1`.

### Sample Input & Output
- **Input**: `arr = [4, 5, 2, 25]` -> `[5, 25, 25, -1]`

### Buggy Exam Code
```java
class Solution {
    public int[] nextGreaterElements(int[] arr) {
        int n = arr.length;
        int[] nge = new int[n];
        Stack<Integer> st = new Stack<>();
        
        for (int i = 0; i < n; i++) { // Bug 1: forward traversal without reverse stack
            while (st.peek() <= arr[i]) { // Bug 2: EmptyStackException when stack is empty!
                st.pop();
            }
            nge[i] = st.isEmpty() ? -1 : st.peek();
            st.push(arr[i]);
        }
        return nge;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Unchecked `st.peek()` on Empty Stack**: Calling `st.peek()` before checking `!st.isEmpty()` throws `EmptyStackException` on the very first iteration.
2. **Forward Loop for Right Greater Element**: To find the next greater element to the *right*, we must iterate from right-to-left (`i = n - 1 down to 0`) so that elements to the right are already present in the stack.

### Fixed Code
```java
class Solution {
    public int[] nextGreaterElements(int[] arr) {
        int n = arr.length;
        int[] nge = new int[n];
        Stack<Integer> st = new Stack<>();
        
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && st.peek() <= arr[i]) {
                st.pop();
            }
            nge[i] = st.isEmpty() ? -1 : st.peek();
            st.push(arr[i]);
        }
        return nge;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<int> nextGreaterElements(const std::vector<int>& arr) {
        int n = arr.size();
        std::vector<int> nge(n);
        std::stack<int> st;
        for (int i = n - 1; i >= 0; i--) {
            while (!st.empty() && st.top() <= arr[i]) {
                st.pop();
            }
            nge[i] = st.empty() ? -1 : st.top();
            st.push(arr[i]);
        }
        return nge;
    }
};
```
</details>

### Dry-Run Table
| Index `i` | `arr[i]` | Stack State Before Pop | Popped Elements | Stack Top (`nge[i]`) | Stack After Push |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 3 | 25 | `[]` | None | -1 | `[25]` |
| 2 | 2 | `[25]` | None | 25 | `[25, 2]` |
| 1 | 5 | `[25, 2]` | 2 | 25 | `[25, 5]` |
| 0 | 4 | `[25, 5]` | None | 5 | `[25, 5, 4]` |

### Spot-It-Fast Rule
Next greater to the RIGHT requires right-to-left loop (`n - 1 down to 0`). Always guard `while (!st.isEmpty() && st.peek() <= arr[i])`.

### Edge Cases
1. Empty array: Returns empty array.
2. Strictly decreasing array `[5, 4, 3]`: Every element has NGE `-1`.
3. Strictly increasing array `[1, 2, 3]`: Next element is adjacent right.

- **Complexity**: Time: $O(N)$, Space: $O(N)$.

---

## Problem 3 (DBG-013): Subsets Generation & State Restoration
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Backtracking
**Source**: Added practice

### Problem Statement
Given an integer array `nums` of unique elements, return all possible subsets (the power set).

### Sample Input & Output
- **Input**: `nums = [1, 2, 3]` -> `[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]`

### Buggy Exam Code
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), res);
        return res;
    }
    
    private void backtrack(int[] nums, int start, List<Integer> current, List<List<Integer>> res) {
        res.add(current); // Bug 1: adds reference instead of shallow copy!
        for (int i = start; i < nums.length; i++) {
            current.add(nums[i]);
            backtrack(nums, start + 1, current, res); // Bug 2: uses start + 1 instead of i + 1
            // Bug 3: missing state restoration current.remove(current.size() - 1)!
        }
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Adding Reference to Result**: Adding `current` adds the object reference. As `current` mutates, all previously inserted lists in `res` mutate into empty lists or final lists. Must add `new ArrayList<>(current)`.
2. **Passing `start + 1` instead of `i + 1`**: Passing `start + 1` re-processes index `i` repeatedly, producing duplicate and out-of-order subsets.
3. **Missing Backtracking Pop**: Omitting `current.remove(current.size() - 1)` fails to backtrack state, causing the list to grow indefinitely.

### Fixed Code
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), res);
        return res;
    }
    
    private void backtrack(int[] nums, int start, List<Integer> current, List<List<Integer>> res) {
        res.add(new ArrayList<>(current));
        for (int i = start; i < nums.length; i++) {
            current.add(nums[i]);
            backtrack(nums, i + 1, current, res);
            current.remove(current.size() - 1);
        }
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    std::vector<std::vector<int>> subsets(const std::vector<int>& nums) {
        std::vector<std::vector<int>> res;
        std::vector<int> current;
        backtrack(nums, 0, current, res);
        return res;
    }
private:
    void backtrack(const std::vector<int>& nums, int start, std::vector<int>& current, std::vector<std::vector<int>>& res) {
        res.push_back(current);
        for (int i = start; i < (int)nums.size(); i++) {
            current.push_back(nums[i]);
            backtrack(nums, i + 1, current, res);
            current.pop_back();
        }
    }
};
```
</details>

### Dry-Run Table
| Function Call | `current` State | Copied into `res` | Recursive Next Step | After Backtrack Pop |
| :--- | :--- | :--- | :--- | :--- |
| `bt(0)` | `[]` | `[]` | Add `1` -> `bt(1)` | `[]` |
| `bt(1)` | `[1]` | `[1]` | Add `2` -> `bt(2)` | `[1]` |
| `bt(2)` | `[1, 2]` | `[1, 2]` | `i` loop finishes | Pop 2 -> `[1]` |

### Spot-It-Fast Rule
In backtracking:
1. Copy into result: `res.add(new ArrayList<>(cur));`
2. Advance index: `backtrack(..., i + 1, ...)`
3. Clean state: `cur.remove(cur.size() - 1);`

### Edge Cases
1. Empty array: Produces `[[]]`.
2. Array of size 1 `[1]`: Produces `[[], [1]]` ($2^1 = 2$).

- **Complexity**: Time: $O(2^N \times N)$, Space: $O(N)$ recursion depth.

---

## Problem 4 (DBG-031): Reverse Linked List Pointer Loss Bug
**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Linked Lists
**Source**: Added practice

### Problem Statement
Given the `head` of a singly linked list, reverse the list, and return the reversed list.

### Sample Input & Output
- **Input**: `head = [1, 2, 3, 4, 5]` -> `[5, 4, 3, 2, 1]`

### Buggy Exam Code
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            curr.next = prev;     // Bug: Overwrites curr.next before saving it!
            curr = curr.next;     // Moves to prev (null) immediately!
            prev = curr;
        }
        return prev;
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Pointer Loss Before Reversal**: `curr.next` is overwritten with `prev` before storing `curr.next` in a temporary variable (`nextTemp`). This severs access to the remainder of the linked list.
2. **Pointer Step Order**: In `curr = curr.next`, `curr` takes the newly assigned `prev` (null), terminating the loop after 1 step.

### Fixed Code
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
};
```
</details>

### Dry-Run Table
| Iteration | `curr` | `nextTemp` Saved | Pointer Reversal | `prev` Updated | Next `curr` |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | Node 1 | Node 2 | `1 -> null` | Node 1 | Node 2 |
| 2 | Node 2 | Node 3 | `2 -> 1` | Node 2 | Node 3 |
| 3 | Node 3 | null | `3 -> 2` | Node 3 | null (Loop ends) |

### Spot-It-Fast Rule
Standard 4-step pointer reversal:
1. `next = curr.next;`
2. `curr.next = prev;`
3. `prev = curr;`
4. `curr = next;`

### Edge Cases
1. Empty list (`head == null`): Returns `null`.
2. Single-node list (`head = [1]`): Returns `[1]`.

- **Complexity**: Time: $O(N)$, Space: $O(1)$.

---

## Problem 5 (DBG-032): Valid Parentheses Stack Underflow Bug
**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Stack
**Source**: Added practice

### Problem Statement
Given a string `s` containing `'(', ')', '{', '}', '[' and ']'`, determine if the input string is valid.

### Sample Input & Output
- **Input**: `s = "()[]{}"` -> `true`
- **Input**: `s = "(]"` -> `false`

### Buggy Exam Code
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> st = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == '(') st.push(')');
            else if (c == '{') st.push('}');
            else if (c == '[') st.push(']');
            else if (st.pop() != c) { // Bug: EmptyStackException when closing bracket has no open!
                return false;
            }
        }
        return true; // Bug: returns true even if open brackets remain in stack!
    }
}
```

### Bugs Found & Why They Are Wrong
1. **Stack Underflow on Premature Closing Bracket**: If `s` begins with a closing bracket (e.g., `"]"`), calling `st.pop()` on an empty stack crashes with `EmptyStackException`.
2. **Unchecked Remaining Open Brackets**: If `s` has unmatched open brackets (e.g., `"("`), `return true;` ignores non-empty stack state. Must return `st.isEmpty()`.

### Fixed Code
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> st = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == '(') st.push(')');
            else if (c == '{') st.push('}');
            else if (c == '[') st.push(']');
            else {
                if (st.isEmpty() || st.pop() != c) return false;
            }
        }
        return st.isEmpty();
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class Solution {
public:
    bool isValid(const std::string& s) {
        std::stack<char> st;
        for (char c : s) {
            if (c == '(') st.push(')');
            else if (c == '{') st.push('}');
            else if (c == '[') st.push(']');
            else {
                if (st.empty() || st.top() != c) return false;
                st.pop();
            }
        }
        return st.empty();
    }
};
```
</details>

### Dry-Run Table
| Character `c` | Stack Action | Stack State | Valid Step? |
| :---: | :---: | :---: | :---: |
| `(` | Push `)` | `[')']` | Yes |
| `[` | Push `]` | `[')', ']']` | Yes |
| `]` | Pop `]` and match | `[')']` | Yes ($] == ]$) |
| `)` | Pop `)` and match | `[]` | Yes ($) == )$) |

### Spot-It-Fast Rule
Never pop a stack without checking `st.isEmpty()`. End of method must return `st.isEmpty()`.

### Edge Cases
1. Leading closing bracket `")"`: `st.isEmpty()` triggers `return false`.
2. Unclosed opening bracket `"("`: At end `st.isEmpty()` returns `false`.

- **Complexity**: Time: $O(N)$, Space: $O(N)$.

---

## Problem 6 (DBG-033): Min Stack Out-of-Sync Tracker Bug
**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Stack Design
**Source**: Added practice

### Problem Statement
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time $O(1)$.

### Sample Input & Output
- **Input**: `push(-2)`, `push(0)`, `push(-3)`, `getMin()`, `pop()`, `top()`, `getMin()`
- **Output**: `[null, null, null, -3, null, 0, -2]`

### Buggy Exam Code
```java
class MinStack {
    private Stack<Integer> st = new Stack<>();
    private Stack<Integer> minSt = new Stack<>();

    public void push(int val) {
        st.push(val);
        if (minSt.isEmpty() || val < minSt.peek()) { // Bug: strict '<' fails duplicate minimums!
            minSt.push(val);
        }
    }

    public void pop() {
        // Bug: compares object reference or pops blindly
        if (st.pop() == minSt.peek()) {
            minSt.pop();
        }
    }

    public int top() { return st.peek(); }
    public int getMin() { return minSt.peek(); }
}
```

### Bugs Found & Why They Are Wrong
1. **Strict Inequality on Minimum Push (`val < minSt.peek()`)**: When equal minimums are pushed (e.g., `-2, 0, -2`), the second `-2` is not pushed to `minSt`. When the first `-2` is popped, the minimum tracker loses all knowledge of the second `-2`.
2. **Object Reference Equality on Integer Pop**: In Java, `Integer` values outside `[-128, 127]` are distinct objects. Using `==` evaluates reference inequality instead of value equality (`equals()`).

### Fixed Code
```java
class MinStack {
    private Stack<Integer> st = new Stack<>();
    private Stack<Integer> minSt = new Stack<>();

    public void push(int val) {
        st.push(val);
        if (minSt.isEmpty() || val <= minSt.peek()) {
            minSt.push(val);
        }
    }

    public void pop() {
        if (!st.isEmpty()) {
            int val = st.pop();
            if (val == minSt.peek()) {
                minSt.pop();
            }
        }
    }

    public int top() {
        return st.peek();
    }

    public int getMin() {
        return minSt.peek();
    }
}
```

<details>
<summary>C++ version</summary>

```cpp
class MinStack {
    std::stack<int> st;
    std::stack<int> minSt;
public:
    void push(int val) {
        st.push(val);
        if (minSt.empty() || val <= minSt.top()) {
            minSt.push(val);
        }
    }
    void pop() {
        if (!st.empty()) {
            if (st.top() == minSt.top()) {
                minSt.pop();
            }
            st.pop();
        }
    }
    int top() { return st.top(); }
    int getMin() { return minSt.top(); }
};
```
</details>

### Dry-Run Table
| Operation | Main Stack `st` | Min Stack `minSt` | `getMin()` Value |
| :--- | :--- | :--- | :---: |
| `push(-2)` | `[-2]` | `[-2]` | -2 |
| `push(0)` | `[-2, 0]` | `[-2]` | -2 |
| `push(-2)` | `[-2, 0, -2]` | `[-2, -2]` (Stored <=) | -2 |
| `pop()` | `[-2, 0]` | `[-2]` (One -2 popped) | -2 |

### Spot-It-Fast Rule
Push to minimum stack if `val <= minSt.peek()`. Use `<=` to track duplicates.

### Edge Cases
1. Consecutive duplicate minimums: Both stored; popping one retains the other.
2. Popping until empty: Guards against NPE.

- **Complexity**: Time: $O(1)$ all operations, Space: $O(N)$.

---

Previous: [03_greedy_and_intervals.md](03_greedy_and_intervals.md) | Next: [05_graphs_and_dp.md](05_graphs_and_dp.md)
