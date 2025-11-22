<!-- #region 82-Count String Encodings -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q82: Count String Encodings</h1>

## 1. Problem Understanding

- Given a string of digits, find all possible ways to encode it as letters a-z corresponding to 1-26.
- Return the total number of valid encodings.
- Examples:
  * "123" → "abc", "aw", "lc" → total 3.
  * "013" → invalid → total 0.
---

## 2. Constraints

- 0 <= str.length <= 10
- Digits are 0-9
---

## 3. Edge Cases

- Leading 0 → invalid.
- Any substring forming 0 or >26 → invalid.
- Single digit 1-9 → valid encoding.
---

## 4. Examples

```text
Input: "123" → Output: 3

Input: "013" → Output: 0

Input: "303" → Output: 0
```

---

## 5. Approaches

### Approach 1: Recursion

**Idea:**
- Try encoding 1-digit and 2-digit numbers recursively.
- Base case: empty string → return 1 (valid encoding).
- If first digit is '0' → return 0 (invalid).
- Count total encodings by summing results from 1-digit and 2-digit recursive calls.

**Steps:**
- If string empty → return 1.
- If first digit is '0' → return 0.
- Take first digit → recurse on rest of string.
- If first two digits ≤26 → recurse on remaining substring.
- Return sum of counts.

**Java Code:**
```java
static int countEncodings(String str) {
    if (str.length() == 0) return 1;
    if (str.charAt(0) == '0') return 0;

    // 1-digit encoding
    int count = countEncodings(str.substring(1));

    // 2-digit encoding
    if (str.length() >= 2) {
        int num = Integer.parseInt(str.substring(0, 2));
        if (num <= 26) count += countEncodings(str.substring(2));
    }

    return count;
}
countEncodings("123")
        /             \
  "23" (1-digit)     "3" (2-digit: 12)
   /      \               \
"3"        ""              ""
 |          |               |
""         ""               ""
Paths:

"1"-"2"-"3" → "abc"

"1"-"23" → "aw"

"12"-"3" → "lc"

Total = 3
```

**Complexity (Time & Space):**
- Time: O(2^n) → each step can branch into 1-digit or 2-digit encoding.
- Space: O(n) → recursion stack.

---

## 6. Justification / Proof of Optimality

- Correct because it recursively checks all valid splits of the string.
- Handles invalid cases with leading zeros and numbers >26.
---

## 7. Variants / Follow-Ups

- Return all possible encoded strings (not just count).
- Dynamic Programming / memoization to optimize for larger strings.
---

## 8. Tips & Observations

- Always check for leading zeros before considering a substring.
- If substring >26 → skip that branch.
- Maximum recursion depth = length of string.
- For counting, you don’t need to generate strings; just sum valid paths.
---

<!-- #endregion -->
