<!-- #region 205-Implement two Stacks in an Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q205: Implement two Stacks in an Array</h1>

## 1. Problem Statement

- You are required to implement two stacks using a single array in a space-efficient manner.
- You need to support the following operations:
  * pop1() → Pop an element from Stack 1 and print it
  * push1(x) → Push element x into Stack 1
  * pop2() → Pop an element from Stack 2 and print it
  * push2(x) → Push element x into Stack 2
- Rules
  * Both stacks must use the same array
  * Print -1 in case of overflow or underflow
---

## 2. Problem Understanding

- We need two independent stacks
- We are allowed only one array
- Space must be used efficiently
- Naive approach (splitting array in half) wastes space.
- Instead, we let:
  * Stack 1 grow from the left
  * Stack 2 grow from the right
---

## 3. Constraints

- 1 <= n <= 500
- Stack operations must be O(1)
- No extra arrays allowed
---

## 4. Edge Cases

- Pop from empty stack → print -1
- Push when array is full → print -1
- Interleaved operations on both stacks
---

## 5. Examples

```text
Input

5
1
4 5156
2 989
3
1


Output

-1
5156
989
```

---

## 6. Approaches

### Approach 1: Two Pointers (Optimal)

**Idea:**
- Use a single array arr[]
- Maintain two pointers:
  * top1 = -1 → Stack 1 (left to right)
  * top2 = size → Stack 2 (right to left)
- Stack overflow occurs when top1 + 1 == top2

**Steps:**
- push1(x):
  * If top1 + 1 == top2 → overflow → print -1
  * Else → arr[++top1] = x
- push2(x):
  * If top1 + 1 == top2 → overflow → print -1
  * Else → arr[--top2] = x
- pop1():
  * If top1 == -1 → underflow → print -1
  * Else → print arr[top1--]
- pop2():
  * If top2 == size → underflow → print -1
  * Else → print arr[top2++]

**Java Code:**
```java
class TwoStacks {
    int[] arr;
    int size;
    int top1, top2;

    TwoStacks(int n) {
        size = n;
        arr = new int[n];
        top1 = -1;
        top2 = n;
    }

    void push1(int x) {
        if (top1 + 1 == top2) {
            System.out.println(-1);
            return;
        }
        arr[++top1] = x;
    }

    void push2(int x) {
        if (top1 + 1 == top2) {
            System.out.println(-1);
            return;
        }
        arr[--top2] = x;
    }

    void pop1() {
        if (top1 == -1) {
            System.out.println(-1);
            return;
        }
        System.out.println(arr[top1--]);
    }

    void pop2() {
        if (top2 == size) {
            System.out.println(-1);
            return;
        }
        System.out.println(arr[top2++]);
    }
}
```

**💭 Intuition Behind the Approach:**
- Stack 1 grows left → right
- Stack 2 grows right → left
- Both stacks expand towards the center
- Space is fully utilized with zero wastage

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * push1 / push2: O(1) — pointer movement only
  * pop1 / pop2: O(1) — direct access
- 💾 Space Complexity
  * O(N) — single shared array
  * No extra space used

---

## 7. Justification / Proof of Optimality

- This is the most space-efficient solution:
  * Uses one array
  * No wasted space
  * All operations are constant time
---

## 8. Variants / Follow-Ups

- Fixed half-array stacks (wastes space)
- Dynamic resizing (not allowed here)
---

## 9. Tips & Observations

- Overflow condition is shared
- top1 + 1 == top2 is the key check
- This is a classic interview problem
---

## 10. Pitfalls

- Forgetting shared overflow condition
- Confusing stack directions
- Off-by-one pointer errors
---

<!-- #endregion -->
