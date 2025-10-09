<!-- #region 56-Camel Case Conversion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q56: Camel Case Conversion</h1>

## 1. Problem Understanding

- Convert a string with words separated by underscores (_) to camel case.
- Rules:
  * Remove underscores _.
  * First letter of the entire string → lowercase.
  * First letter of each subsequent word → uppercase.
---

## 2. Constraints

- 1 <= T <= 10 (number of test cases)
- 1 <= |S| <= 100000 (length of each string)
- String contains lowercase letters and underscores only.
---

## 3. Edge Cases

- String has consecutive underscores: "a__b" → "aB"
- String starts or ends with underscores: "_abc_" → "Abc"
- String with only one word → stays lowercase
- String with maximum length → performance matters (regex vs iterative)
---

## 4. Examples

```text
Example 1
Input: abb_b_cc_d
Output: abbBCcD

Example 2
Input:how_are_you
Output:howAreYou

Example 3 (Edge Case)
Input: __start_middle__end__
Output: StartMiddleEnd
```

---

## 5. Approaches

### Approach 1: Iterative (Character Traversal)

**Idea:**
- Traverse string character by character. Track previous character to detect underscores. Convert next character to uppercase if previous was _.

**Steps:**
- Initialize prev = ' ' (space).
- For each character c in string:
  * If c is not _ and prev == '_' → uppercase c.
  * Else if c is not _ → keep as is.
  * Skip underscores.
  * Update prev = c.

**Java Code:**
```java
static void camelCaseIterative(String s) {
    char prev = ' ';
    for(int i = 0; i < s.length(); i++){
        char c = s.charAt(i);
        if(c != '_' && prev == '_') System.out.print(Character.toUpperCase(c));
        else if(c != '_') System.out.print(c);
        prev = c;
    }
}
```

**Complexity (Time & Space):**
- Time → O(n)
- Space → O(1) (in-place printing)

### Approach 2: Using StringBuilder

**Idea:**
- Similar to iterative, but store result in StringBuilder to return string instead of printing.

**Steps:**
- Initialize StringBuilder sb.
- Traverse string, skip underscores.
- Capitalize first letter after _.
- Append to sb

**Java Code:**
```java
static String camelCaseSB(String s) {
    StringBuilder sb = new StringBuilder();
    boolean upperNext = false;
    for(char c : s.toCharArray()){
        if(c == '_') upperNext = true;
        else {
            sb.append(upperNext ? Character.toUpperCase(c) : c);
            upperNext = false;
        }
    }
    return sb.toString();
}
```

**Complexity (Time & Space):**
- Time → O(n)
- Space → O(n) (for StringBuilder)

### Approach 3: Regex Approach

**Idea:**
- Use regular expression to find underscores followed by a character and replace them with the uppercase character.

**Steps:**
- Use regex "_([a-z])" to capture the character after _.
- Replace match with its uppercase using replaceAll() and lambda.
- Optional: convert first character to lowercase if needed.

**Java Code:**
```java
static String camelCaseRegex(String s) {
    // Replace '_' followed by a letter with uppercase letter
    String result = s.replaceAll("_(.)", m -> m.group(1).toUpperCase());
    return result;
}

Method Calls Explained:
replaceAll(regex, lambda) → finds all matches of regex and replaces them.
_(.) → matches underscore + single character; () captures the character.
m.group(1) → retrieves captured character.
toUpperCase() → converts it to uppercase.
```

**Complexity (Time & Space):**
- Time → O(n) (Java regex engine scans the string)
- Space → O(n) (new string created)

---

## 6. Justification / Proof of Optimality

- Iterative → fastest, minimal memory.
- StringBuilder → easier to return string, avoids repeated print.
- Regex → very concise, ideal for pattern-based transformations, slightly slower for huge strings.
---

## 7. Variants / Follow-Ups

- PascalCase (first letter uppercase)
- SnakeCase ↔ CamelCase conversions
- Remove digits, punctuation before camel case (replaceAll("[^a-zA-Z_]", ""))
- Kebab-case (-) to CamelCase
---

## 8. Tips & Observations

- Use Character.toUpperCase() / Character.toLowerCase() instead of ASCII math.
- Regex "_(.)" is versatile and works for any character after underscore.
- For strings of length > 10^5, iterative approach is slightly faster than regex.
- Combining replaceAll("[^a-zA-Z_]", "") first can clean string before camel case.
---

<!-- #endregion -->
