<!-- #region 207-Balanced Brackets -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q207: Balanced Brackets</h1>

## 1. Problem Statement

- You are given a string s of length n consisting only of the characters
  * '(' , ')' , '{' , '}' , '[' , ']'.
- Your task is to determine whether the given string is balanced.
- A string is said to be balanced if:
  * Every opening bracket has a corresponding closing bracket of the same type
  * Brackets are closed in the correct order
- Input Format
  * Line 1: An integer n — length of the string
  * Line 2: A string s
- Output Format
  * Print "YES" if brackets are balanced
  * Print "NO" otherwise
- Constraints
  * 1 <= n <= 10^6
  * s contains only '()[]{}'
---

## 2. Problem Understanding

- We must validate pairing + order of brackets.
- Order matters more than count.
- Nested and sequential brackets must both work.
- Any mismatch → invalid immediately.
---

## 3. Constraints

- Large input → must be O(N)
- Stack solution fits perfectly.
---

## 4. Edge Cases

- Closing bracket appears when stack is empty → NO
- Stack not empty after traversal → NO
- Single character string → NO
- Wrong type closure like (] → NO
- Wrong order like ([)] → NO
---

## 5. Examples

```text
Input
()
Output
YES

Input
(]
Output
NO

Input
{[()]}
Output
YES

Input
([)]
Output
NO
```

---

## 6. Approaches

### Approach 1: Stack (Optimal)

**Idea:**
- Use stack to store opening brackets.
- Closing bracket must match top of stack.

**Steps:**
- Steps
- Create empty stack
- Traverse string:
  * If opening bracket → push
  * If closing bracket:
    * stack empty → NO
    * pop and check matching
- After traversal:
  * stack empty → YES
  * else → NO

**Java Code:**
```java
public String isBalanced(String s) {
    Stack<Character> st = new Stack<>();

    for (char ch : s.toCharArray()) {
        if (ch == '(' || ch == '{' || ch == '[') {
            st.push(ch);
        } else {
            if (st.isEmpty()) return "NO";

            char top = st.pop();
            if ((ch == ')' && top != '(') ||
                (ch == '}' && top != '{') ||
                (ch == ']' && top != '[')) {
                return "NO";
            }
        }
    }
    return st.isEmpty() ? "YES" : "NO";
}
```

**💭 Intuition Behind the Approach:**
- Stack follows LIFO, perfect for nested structures.
- Latest opening bracket must be closed first.
- Any violation breaks balance.

**Complexity (Time & Space):**
- Time: O(N) — one pass
- Space: O(N) — stack in worst case

---

## 7. Justification / Proof of Optimality

- Stack ensures correct order and type matching.
- Immediate failure on mismatch.
- Optimal and scalable.
---

## 8. Variants / Follow-Ups

- Valid Parentheses
- Remove Invalid Parentheses
- Minimum Add to Make Parentheses Valid
- Score of Parentheses
---

## 9. Tips & Observations

- Always check stack before pop
- Store only opening brackets
- Matching logic must be strict
---

## 10. Pitfalls

- Ignoring order
- Only counting brackets
- Forgetting final stack empty check
- Using wrong matching conditions
---

<!-- #endregion -->
