<!-- #region 209-Balanced Expression -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q207: Balanced Expression</h1>

## 1. Problem Statement

- You are given a string exp representing an expression.
- The expression may contain:
- Opening brackets: '(', '{', '['
- Closing brackets: ')', '}', ']'
- Operators: + - * /
- Lowercase English letters
- You need to determine whether the expression is balanced.
- A string is balanced if:
  * Every opening bracket is closed by the same type
  * Brackets are closed in the correct order
- Return true if the expression is balanced, otherwise return false.
---

## 2. Problem Understanding

- This is not about evaluating the expression
- We only care about bracket correctness
- Non-bracket characters must be ignored
- Order matters → last opened must be closed first (LIFO)
- This directly hints at using a stack.
---

## 3. Constraints

- 1 <= exp.length <= 10^4
- Expression contains valid ASCII characters only
---

## 4. Edge Cases

- Empty string → balanced
- Closing bracket before any opening → invalid
- Mismatched types: {], (} → invalid
- Extra opening brackets left at the end → invalid
- Expression with no brackets → balanced
---

## 5. Examples

```text
"[(a+b)+{(c+d)*(e/f)}]" → true

"[(a+b)+{(c+d)*(e/f)]}" → false

"[(a+b)+{(c+d)*(e/f)}" → false

"([(a+b)+{(c+d)*(e/f)}]" → false
```

---

## 6. Approaches

### Approach 1: Brute Force (Repeated Removal)

**Idea:**
- Repeatedly remove valid bracket pairs: (), {}, []
- If the string reduces to empty → balanced
- Else → invalid

**Steps:**
- Loop while the string changes
- Replace "()", "{}", "[]" with empty
- Stop when no more replacements are possible
- Check if string has any brackets left

**Java Code:**
```java
public static boolean expBalancedBrute(String s) {
    boolean changed = true;

    while (changed) {
        String prev = s;
        s = s.replace("()", "")
             .replace("{}", "")
             .replace("[]", "");
        changed = !s.equals(prev);
    }

    for (char c : s.toCharArray()) {
        if (c == '(' || c == '{' || c == '[' ||
            c == ')' || c == '}' || c == ']') {
            return false;
        }
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Balanced brackets always contain adjacent valid pairs
- Removing inner pairs slowly collapses the expression

**Complexity (Time & Space):**
- Time: O(N^2) — repeated string scans and replacements
- Space: O(N) — string rebuilding

### Approach 2: Stack-Based (Standard & Optimal)

**Idea:**
- Use a stack to store opening brackets
- When a closing bracket appears:
  * Stack must not be empty
  * Top must be the matching opening bracket

**Steps:**
- Traverse the expression
- Push opening brackets onto stack
- On closing bracket:
  * If stack empty → invalid
  * Pop and verify matching type
- Ignore non-bracket characters
- At end, stack must be empty

**Java Code:**
```java
public static boolean expBalanced(String str) {
    Stack<Character> s = new Stack<>();

    for (int i = 0; i < str.length(); i++) {
        char c = str.charAt(i);

        if (c == '(' || c == '{' || c == '[') {
            s.push(c);
        }
        else if (c == ')' || c == '}' || c == ']') {
            if (s.isEmpty()) return false;

            char top = s.pop();
            if (c == ')' && top != '(') return false;
            if (c == '}' && top != '{') return false;
            if (c == ']' && top != '[') return false;
        }
    }

    return s.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- Stack keeps track of pending openings
- Closing bracket must match the most recent opening
- This enforces correct order automatically

**Complexity (Time & Space):**
- Time: O(N) — single pass
- Space: O(N) — worst case all openings

### Approach 3: Stack + HashMap (Cleaner Matching)

**Idea:**
- Use a map to define valid bracket pairs
- Simplifies matching logic

**Steps:**
- Map closing → opening brackets
- Push opening brackets
- On closing:
  * Stack must not be empty
  * Top must match map value

**Java Code:**
```java
public static boolean expBalancedMap(String str) {
    Stack<Character> st = new Stack<>();
    Map<Character, Character> map = new HashMap<>();

    map.put(')', '(');
    map.put('}', '{');
    map.put(']', '[');

    for (char c : str.toCharArray()) {
        if (map.containsValue(c)) {
            st.push(c);
        }
        else if (map.containsKey(c)) {
            if (st.isEmpty() || st.pop() != map.get(c)) {
                return false;
            }
        }
    }

    return st.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- Encodes bracket rules in data instead of condition chains
- More scalable if bracket types increase

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(N) — stack + map

---

## 7. Justification / Proof of Optimality

- Brackets require order-sensitive matching
- Stack naturally enforces LIFO order
- Any solution not using stack implicitly simulates one
---

## 8. Variants / Follow-Ups

- Valid Parentheses (())
- Valid Parenthesis String (*)
- Remove Invalid Parentheses
- Minimum Add to Make Parentheses Valid
---

## 9. Tips & Observations

- Ignore non-bracket characters early
- Always check stack empty before popping
- If multiple bracket types exist → stack is mandatory
---

## 10. Pitfalls

- Popping without checking stack emptiness
- Forgetting to check leftover opening brackets
- Comparing wrong bracket types
- Treating operators/characters as brackets
---

<!-- #endregion -->
