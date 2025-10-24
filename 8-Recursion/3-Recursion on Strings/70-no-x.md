<!-- #region 70-No X -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q70: No X</h1>

## 1. Problem Understanding

- You are given a string s.
- You must remove all characters 'x' recursively.
- Return or print the new string after all 'x' are removed.
---

## 2. Constraints

- 1 <= s.length() <= 10^4
- String may contain lowercase alphabets including 'x'.
---

## 3. Edge Cases

- String has no 'x' → return same string.
- String is all 'x' → return empty string.
- 'x' appears at start, middle, or end — handle all positions.
---

## 4. Examples

```text
Input → "xaaax"
Output → "aaa"
```

---

## 5. Approaches

### Approach 1: Expanded Recursive Template (for clarity)

**Idea:**
- Work on the first character and solve the rest of the string recursively.

**Steps:**
- Base case: if string is empty → return "".
- Extract first char.
- Recurse for rest of string.
- Combine results:
- If char == 'x' → skip it.
- Else → append it to result.

**Java Code:**
```java
static String removeX(String s) {
    if (s.length() == 0) return "";
    char ch = s.charAt(0);
    String smallAns = removeX(s.substring(1));
    if (ch == 'x') return smallAns;
    else return ch + smallAns;
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n²) (due to substring copying in each call)
- Space Complexity: O(n) (recursion stack)

### Approach 2: Concise Recursive Template (for contests/interviews)

**Idea:**
- Same as above but written compactly in one return chain.

**Java Code:**
```java
static String noX(String s) {
    if (s.length() == 0) return "";
    if (s.charAt(0) == 'x') return noX(s.substring(1));
    return s.charAt(0) + noX(s.substring(1));
}
```

**Complexity (Time & Space):**
- Time Complexity: O(n²)
- Space Complexity: O(n)

---

## 6. Justification / Proof of Optimality

- Both methods are equivalent — Approach 1 is great for concept building,
- Approach 2 is ideal for fast coding once the pattern is familiar.
---

## 7. Tips & Observations

- Always check the first character, then recurse on the remaining.
- Avoid + concatenation in large strings → use StringBuilder for optimization.
- Similar pattern used in:
- “Replace Pi with 3.14”
- “Remove duplicates”
- “Replace character in string”
---

<!-- #endregion -->
