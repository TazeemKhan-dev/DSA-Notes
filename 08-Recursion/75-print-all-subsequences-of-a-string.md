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
## 💭 Intuition Behind the Approach

The problem of generating all subsequences follows a natural **binary decision pattern** — for every character in the string, we have two choices:
1. **Include** the current character in the current subsequence.
2. **Exclude** it and move on.

This branching process forms a recursion tree with `2^N` leaves, where each path from root to leaf represents one unique subsequence.  
Using recursion allows us to systematically explore every possible combination in a clean and structured way, ensuring no subsequence is missed or repeated.

---

## ⚔️ Approach Comparison — Index vs Substring

| Criteria | **Approach 1 (With Index Parameter)** | **Approach 2 (With Substring)** |
|-----------|--------------------------------------|----------------------------------|
| **Idea** | Pass an index `i` to track the current position in the original string. | Slice the string into smaller parts using `substring(1)` at each recursive step. |
| **String Handling** | Original string remains unchanged; recursion moves using index only. | A new string is created every call (`substring()` copies remaining characters). |
| **Memory Usage** | ✅ More efficient — no extra string copies. | ❌ Less efficient — new string allocated in every call. |
| **Execution Speed** | ✅ Faster (O(2ⁿ) with smaller constant factor). | ⚠️ Slower (O(2ⁿ) but with higher memory and time overhead). |
| **Ease of Understanding** | Slightly more technical (needs index handling). | Simpler to grasp conceptually. |
| **Scalability** | ✅ Handles larger strings efficiently (n ≤ 20+). | ⚠️ Can cause memory strain for n > 12–15. |
| **Recursion Depth** | `O(n)` | `O(n)` |
| **Practical Use** | ✅ Preferred in interviews and large test cases. | Okay for learning and short examples. |

---

### ✅ **Verdict**

- Both generate all `2^N` subsequences correctly.  
- However, **Approach 1 (with index parameter)** is **more optimal** in memory and speed.  
- Always choose **index-based recursion** for competitive programming or real-world problems.

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
