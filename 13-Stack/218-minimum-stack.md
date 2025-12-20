<!-- #region 218-Minimum Stack -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q218: Minimum Stack</h1>

## 1. Problem Statement

- Design a stack that supports the following operations in constant time O(1):
  * push(x) → insert element x
  * pop() → remove and return top element (-1 if empty)
  * getMin() → return the minimum element in the stack (-1 if empty)
---

## 2. Problem Understanding

- You are implementing a custom stack
- Along with normal stack behavior (LIFO), you must track the minimum element at every point
- getMin() must be O(1), not by scanning the stack
- Stack values are positive integers
- Key challenge:
  * How do we always know the minimum without traversing the stack?
---

## 3. Constraints

- 1 <= Number of operations <= 100
- 1 <= stack values <= 100
- Stack size is small, but time complexity still matters
---

## 4. Edge Cases

- Calling pop() on empty stack → return -1
- Calling getMin() on empty stack → return -1
- Popping the current minimum
- Duplicate minimum values
---

## 5. Examples

```text
Example 1
push(2)
push(3)
pop()      → 3
getMin()   → 2
push(1)
getMin()   → 1

Example 2
push(5)
push(8)
push(4)
push(6)
getMin()   → 4
pop()      → 6
pop()      → 4
getMin()   → 5
```

---

## 6. Approaches

### Approach 1: Brute Force (Not Optimal)

**Idea:**
- Use a normal stack
- For getMin(), scan the entire stack to find minimum

**Steps:**
- Push → normal push
- Pop → normal pop
- getMin → traverse stack to find min

**Java Code:**
```java
int getMin(Stack<Integer> st) {
    if (st.isEmpty()) return -1;
    int min = Integer.MAX_VALUE;
    for (int x : st) {
        min = Math.min(min, x);
    }
    return min;
}
```

**💭 Intuition Behind the Approach:**
- Simple but inefficient — each getMin() recomputes the minimum from scratch.

**Complexity (Time & Space):**
- Time: O(N) — scanning stack every time
- Space: O(1) — no extra data structure

### Approach 2: : Two Stacks (Standard Optimal Solution)

**Idea:**
- Maintain two stacks:
  * mainStack → stores all values
  * minStack → stores minimum so far at each level

**Steps:**
- push(x)
  * push x to mainStack
  * push min(x, minStack.peek()) to minStack
- pop()
  * pop from both stacks
- getMin()
  * return minStack.peek()

**Java Code:**
```java
Stack<Integer> st = new Stack<>();
Stack<Integer> minSt = new Stack<>();

void push(int x) {
    st.push(x);
    if (minSt.isEmpty()) {
        minSt.push(x);
    } else {
        minSt.push(Math.min(x, minSt.peek()));
    }
}

int pop() {
    if (st.isEmpty()) return -1;
    minSt.pop();
    return st.pop();
}

int getMin() {
    if (minSt.isEmpty()) return -1;
    return minSt.peek();
}
```

**💭 Intuition Behind the Approach:**
- At every level, we remember the minimum till that point, so we never recompute.

**Complexity (Time & Space):**
- Time: O(1) — all operations
- Space: O(N) — extra stack for min values

### Approach 3: Single Stack with Encoding (Most Optimized)

**Idea:**
- Use one stack
- Encode values when pushing a new minimum
- Formula:
  * encodedValue = 2*x - currentMin

**Steps:**
- Maintain min variable
- push(x)
  * if stack empty → push x, min = x
  * if x >= min → push x
  * else → push encoded value, update min
- pop()
  * if popped < min → decode previous min
- getMin()
  * return min

**Java Code:**
```java
Stack<Integer> st = new Stack<>();
int min = Integer.MAX_VALUE;

void push(int x) {
    if (st.isEmpty()) {
        st.push(x);
        min = x;
    } else if (x >= min) {
        st.push(x);
    } else {
        st.push(2 * x - min);
        min = x;
    }
}

int pop() {
    if (st.isEmpty()) return -1;

    int top = st.pop();
    if (top >= min) {
        return top;
    } else {
        int originalMin = min;
        min = 2 * min - top;
        return originalMin;
    }
}

int getMin() {
    if (st.isEmpty()) return -1;
    return min;
}
```

**💭 Intuition Behind the Approach:**
- Encoded values act as markers
- They allow restoring the previous minimum during pop

**Complexity (Time & Space):**
- Time: O(1) — all operations
- Space: O(1) — no extra stack

---

## 7. Justification / Proof of Optimality

- Approach 2 is most readable and interview-safe
- Approach 3 is space-optimal and shows deep understanding
- Brute force fails the O(1) requirement
---

## 8. Variants / Follow-Ups

- Max Stack (track maximum instead of minimum)
- Min Stack with duplicate handling
- Stack supporting getMin() and getMax()
---

## 9. Tips & Observations

- If interviewer allows extra space → Two Stack approach
- If space optimized solution needed → Encoding approach
- Always clarify whether duplicate minimums exist (they do here)
---

## 10. Pitfalls

- Forgetting to sync minStack during pop
- Not updating min correctly during encoded pop
- Returning wrong value when popping encoded numbers
- Treating this like a normal stack without min tracking
---

<!-- #endregion -->
