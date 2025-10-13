<!-- #region 62-Autori: Shorten Author Names -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q62: Autori: Shorten Author Names</h1>

## 1. Problem Understanding

- Input: A "long variation" of authors, e.g., Knuth-Morris-Pratt.
- Task: Convert to "short variation" using only the first letters of each last name, e.g., KMP.

- **Rules:**
    - Names are separated by hyphens -.
    - Each name starts with an uppercase letter.
    - Hyphens are always followed by uppercase letters.
---

## 2. Constraints

- 1 ≤ |s| ≤ 100
- Only letters (a-z, A-Z) and hyphens -
---

## 3. Edge Cases

- Single author → return the first letter
- Maximum length string → |s| = 100
---

## 4. Examples

```text
Example 1
Input: Knuth-Morris-Pratt
Output: KMP

Example 2
Input: Mirko-Slavko
Output: MS
```

---

## 5. Approaches

### Approach 1: Split and concatenate (your current approach)

**Idea:**
- Split string by -
- Take first character of each substring
- Concatenate

**Java Code:**
```java
static String autori(String str) {
    String[] parts = str.split("-");
    String result = "";
    for (String part : parts) {
        result += part.charAt(0);
    }
    return result;
}
```

**Complexity (Time & Space):**
- Time: O(n) → split + iterate through characters
- Space: O(n) → array of substrings + result string

### Approach 2: Single pass without split (more optimal)

**Idea:**
- Iterate over the string once
- Append a character to result if it is uppercase (first character of each name)

**Java Code:**
```java
static String autori(String str) {
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < str.length(); i++) {
        char ch = str.charAt(i);
        if (Character.isUpperCase(ch)) {
            sb.append(ch);
        }
    }
    return sb.toString();
}
```

**Complexity (Time & Space):**
- Time: O(n) → single pass through string
- Space: O(n) → StringBuilder
- ✅ No need to create extra array for splitting

---

## 6. Justification / Proof of Optimality

- Approach 2 is slightly more memory-efficient and avoids creating substrings
- Both approaches are linear in time, which is optimal for |s| ≤ 100
---

## 7. Variants / Follow-Ups

- Handle names with middle initials
- Ignore non-alphabet characters
- Convert all output letters to uppercase (if input format is inconsistent)
---

## 8. Tips & Observations

- Using StringBuilder avoids repeated string concatenation overhead
- Iterating and checking Character.isUpperCase() is a neat trick when first letters are guaranteed to be uppercase
---

<!-- #endregion -->
