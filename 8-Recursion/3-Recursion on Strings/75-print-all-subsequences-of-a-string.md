<!-- #region 75-Print all subsequences of a string -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q75: Print all subsequences of a string</h1>

## 1. Problem Understanding

- We are given a string str, and we need to print all possible subsequences (not substrings).
- A subsequence is formed by including or excluding each character while maintaining the order.
---

## 2. Constraints

- 1 ≤ str.length() ≤ 15
- Total possible subsequences = 2ⁿ
---

## 3. Edge Cases

- Empty string → only one subsequence: ""
- Repeated characters → subsequences may not be unique.
- Long string (n > 20) → exponential output, recursion may overflow.
---

## 4. Examples

```text
Input:
abc
Output:
abc ab ac a bc b c 

Justification
Every subsequence is a unique combination of including/excluding characters in order.
The recursion tree for "abc" looks like:

              ""
          /         \
        "a"          ""
       /   \        /   \
    "ab"   "a"   "b"    ""
    / \     / \   / \    / \
"abc" "ab" "ac" "a" "bc" "b" "c" ""
```

---

## 5. Approaches

### Approach 1: With Index Parameter

**Idea:**
- At each index, we have two choices:
  * Include the current character.
  * Exclude the current character.
- This branching generates all possible subsequences.

**Steps:**
- Base Case: if i == str.length(), print the accumulated string ans.
- Recursive Case:
- Include str.charAt(i) in ans → call f(str, i + 1, ans + str.charAt(i))
- Exclude str.charAt(i) → call f(str, i + 1, ans)

**Java Code:**
```java
void printSubsequences(String str, int i, String ans) {
    if (i == str.length()) {
        System.out.print(ans + " ");
        return;
    }

    // include
    printSubsequences(str, i + 1, ans + str.charAt(i));
    // exclude
    printSubsequences(str, i + 1, ans);
}
Call: printSubsequences(str, 0, "")
```

**Complexity (Time & Space):**
- Time Complexity: O(2ⁿ) → each character has 2 choices
- Space Complexity: O(n) → recursion stack

### Approach 2: Without Using Index Parameter

**Idea:**
- Instead of tracking index, break the string from the front:
- At each step, take the first character and work on the remaining substring.

**Java Code:**
```java
void printSubsequences(String str, String ans) {
    if (str.isEmpty()) {
        System.out.print(ans + " ");
        return;
    }

    char ch = str.charAt(0);
    String rest = str.substring(1);

    // include
    printSubsequences(rest, ans + ch);
    // exclude
    printSubsequences(rest, ans);
}
🟢 Call: printSubsequences(str, "")
```

**Complexity (Time & Space):**
- Time Complexity: O(2ⁿ)
- Space Complexity: O(n)
- Both versions have identical performance.
- Difference: this one uses substring decomposition instead of index tracking.

---

## 6. Variants / Follow-Ups

- Return subsequences as a List: return ArrayList<String> instead of printing.
- Count total subsequences: return int instead of printing.
- Generate subsequences with ASCII values: also include ASCII codes of characters (useful in recursion practice).
---

## 7. Tips & Observations

- This is a 2-branch recursion pattern: include/exclude.
- Number of subsequences = 2ⁿ.
- Often used as a base template for subset, power set, or combination problems.
- In backtracking, this is one of the core “choice-making” structures.
---

<!-- #endregion -->
