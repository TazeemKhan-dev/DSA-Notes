<!-- #region 219-Reverse Substrings Between Each Pair of Parentheses -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q219: Reverse Substrings Between Each Pair of Parentheses</h1>

## 1. Problem Statement

- You are given a string s consisting of:
  * lowercase English alphabets
  * parentheses '(' and ')'
- You must:
  * reverse the substring inside each pair of matching parentheses
  * start reversing from the innermost parentheses
  * remove all parentheses in the final result
- The input string is guaranteed to be balanced.
---

## 2. Problem Understanding

- Parentheses indicate a region that must be reversed
- Innermost parentheses are processed first
- After reversing, parentheses are removed
- Nested parentheses cause multiple reversals
- Key observation:
  * This is a nested structure problem → stack fits naturally.
---

## 3. Constraints

- 1 <= s.length <= 2000
- String is balanced
- Only lowercase letters and parentheses
---

## 4. Edge Cases

- No parentheses → return string as is
- Single pair of parentheses
- Deeply nested parentheses
- Consecutive parentheses
- Entire string inside parentheses
---

## 5. Examples

```text
Input:  (abcd)
Output: dcba


Input:  (accio(job))
Output: joboicca
```

---

## 6. Approaches

### Approach 1: Brute Force (Repeated String Reversal)

**Idea:**
- Find the innermost () pair
- Reverse the substring inside it
- Remove the parentheses
- Repeat until no parentheses remain

**Steps:**
- While string contains '(':
- Find the last '('
- Find the first ')' after it
- Reverse the substring in between
- Replace (sub) with reverse(sub)

**Java Code:**
```java
public String reverseBrute(String s) {
    while (s.contains("(")) {
        int l = s.lastIndexOf('(');
        int r = s.indexOf(')', l);
        String rev = new StringBuilder(s.substring(l + 1, r)).reverse().toString();
        s = s.substring(0, l) + rev + s.substring(r + 1);
    }
    return s;
}
```

**💭 Intuition Behind the Approach:**
- Innermost parentheses have no nested brackets inside
- Replacing them simplifies the problem step by step

**Complexity (Time & Space):**
- Time: O(N^2) — repeated substring operations
- Space: O(N) — string rebuilding

### Approach 2: Stack of Characters

**Idea:**
- Push characters into a stack
- On encountering ')', pop characters until '('
- Reverse popped characters and push back

**Steps:**
- Traverse string character by character
- Push characters onto stack
- When ')' is found:
  * pop until '('
  * reverse collected substring
  * push it back

**Java Code:**
```java
public String reverseStackChar(String s) {
    Stack<Character> st = new Stack<>();

    for (char c : s.toCharArray()) {
        if (c == ')') {
            StringBuilder sb = new StringBuilder();
            while (st.peek() != '(') {
                sb.append(st.pop());
            }
            st.pop(); // remove '('
            for (char ch : sb.toString().toCharArray()) {
                st.push(ch);
            }
        } else {
            st.push(c);
        }
    }

    StringBuilder res = new StringBuilder();
    while (!st.isEmpty()) res.append(st.pop());
    return res.reverse().toString();
}
```

**💭 Intuition Behind the Approach:**
- Stack naturally handles nested parentheses
- Characters inside parentheses are reversed using pop order

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N) — stack

### Approach 3: Stack of StringBuilder (Optimal & Clean)

**Idea:**
- Use a stack of StringBuilder
- Each ( starts a new context
- Each ) ends a context and reverses it

**Steps:**
- Maintain a StringBuilder curr
- On '(':
  * push curr onto stack
  * start new curr
- On ')':
  * reverse curr
  * pop previous string and append
- On letter:
  * append to curr

**Java Code:**
```java
public String reverseParentheses(String s) {
    Stack<StringBuilder> st = new Stack<>();
    StringBuilder curr = new StringBuilder();

    for (char ch : s.toCharArray()) {
        if (ch == '(') {
            st.push(curr);
            curr = new StringBuilder();
        } else if (ch == ')') {
            curr.reverse();
            StringBuilder prev = st.pop();
            prev.append(curr);
            curr = prev;
        } else {
            curr.append(ch);
        }
    }
    return curr.toString();
}
```

**💭 Intuition Behind the Approach:**
- Each parentheses level builds its own substring
- Reversal happens exactly once per pair
- Avoids repeated character-level operations

**Complexity (Time & Space):**
- Time: O(N) — each character processed once
- Space: O(N) — stack + builders

---

## 7. Justification / Proof of Optimality

- Nested structure demands stack usage
- StringBuilder avoids unnecessary string copying
- Clean, readable, and optimal
---

## 8. Variants / Follow-Ups

- Decode string problems
- Remove outer parentheses
- Reverse words inside brackets
- Expression parsing
---

## 9. Tips & Observations

- Always process innermost parentheses first
- Stack naturally enforces nesting order
- StringBuilder is faster than String concatenation
---

## 10. Pitfalls

- Forgetting to remove parentheses
- Reversing too early
- Using recursion unnecessarily
- Mishandling nested brackets
---

<!-- #endregion -->
