<!-- #region 251-Validate Stack Sequences -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q251: Validate Stack Sequences</h1>

## 1. Problem Statement

- You are given two integer arrays of length n, both containing distinct values.
  * The first array pushed[] represents the order in which elements are pushed onto a stack.
  * The second array popped[] represents the order in which elements are popped from the stack.
- Initially, the stack is empty.
- Your task is to determine whether the sequence popped[] can be obtained from the sequence pushed[] using valid stack operations (push and pop).
- Print true if it is possible, otherwise print false.
---

## 2. Problem Understanding

- Stack follows LIFO (Last In, First Out).
- You must push elements in the given order.
- You can pop only from the top of the stack.
- Push and pop operations can be interleaved.
- We need to check whether the popping order violates stack rules at any point.
---

## 3. Constraints

- 1 <= n <= 1000
- 0 <= pushed[i] <= 1000
- All values are distinct
- popped is a permutation of pushed
---

## 4. Edge Cases

- n == 1 → always true
- pushed equals popped → always true
- Trying to pop an element that is not on top → invalid
---

## 5. Examples

```text
Example 1
Input:
5
1 2 3 4 5
4 5 3 2 1

Output:
true


Explanation:
Push 1,2,3,4 → pop 4 → push 5 → pop 5,3,2,1.

Example 2
Input:
5
1 2 3 4 5
4 3 5 1 2

Output:
false


Explanation:
Element 1 cannot be popped before 2 as it is below it in the stack.
```

---

## 6. Approaches

### Approach 1: Stack Simulation (Optimal)

**Idea:**
- Simulate the exact stack behavior:
  * Push elements from pushed[]
  * Whenever the top of stack matches the next element to be popped, pop it
  * Continue this process throughout the sequence

**Steps:**
- Create an empty stack
- Use pointer j for popped[]
- For each element in pushed[]:
  * Push it onto stack
  * While stack top equals popped[j], pop and increment j
- At the end, if stack is empty → valid sequence

**Java Code:**
```java
static boolean validateStackSequences(int[] pushed, int[] popped) {
    Stack<Integer> st = new Stack<>();
    int j = 0;

    for (int x : pushed) {
        st.push(x);

        while (!st.isEmpty() && j < popped.length && st.peek() == popped[j]) {
            st.pop();
            j++;
        }
    }

    return st.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- A stack only allows access to its top element.
- If the next required pop value is not on top, the only option is to push more elements.
- If it is on top, you must pop it immediately.
- Any violation means the sequence is impossible.

**Complexity (Time & Space):**
- Time: O(N) — each element is pushed and popped at most once
- Space: O(N) — stack may store all elements

### Approach 2: Using Array as Stack (Without Java Stack)

**Idea:**
- Using Array as Stack (Without Java Stack)

**Java Code:**
```java
static boolean validateStackSequences(int[] pushed, int[] popped) {
    int n = pushed.length;
    int[] stack = new int[n];
    int top = -1;
    int j = 0;

    for (int x : pushed) {
        stack[++top] = x;

        while (top >= 0 && j < n && stack[top] == popped[j]) {
            top--;
            j++;
        }
    }

    return top == -1;
}
```

**Complexity (Time & Space):**
- Time: O(N) — same simulation
- Space: O(N) — array stack

---

## 7. Justification / Proof of Optimality

- The simulation mirrors real stack operations exactly.
- If at any point a required pop cannot happen from the top, the sequence is invalid.
---

## 8. Variants / Follow-Ups

- Validate queue push-pop sequences
- Find whether any push sequence can generate a given pop sequence
- Count minimum pushes required for a given pop order
---

## 9. Tips & Observations

- Validate queue push-pop sequences
- Find whether any push sequence can generate a given pop sequence
- Count minimum pushes required for a given pop order
---

## 10. Pitfalls

- Forgetting the inner while loop
- Comparing wrong indices
- Trying to reorder elements (not allowed)
- Missing stack emptiness check at the end
---

<!-- #endregion -->
