<!-- #region 79-String Encodings -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q79: String Encodings</h1>

## 1. Problem Understanding

- You are given a numeric string str.
- Each number (1–26) represents a lowercase alphabet letter:
  * 1 → a, 2 → b, 3 → c, …, 26 → z.
- You must print all possible valid encodings of the string.
- Invalid cases:
  * Substrings starting with '0'.
  * Numbers > 26 (like 30, 40).
  * Empty string should result in a valid (printed) path.
---

## 2. Constraints

- 0 ≤ str.length ≤ 10
- String consists only of digits '0'–'9'.
---

## 3. Edge Cases

- String starts with '0' → invalid → print nothing.
- Substrings containing '0' (like "303", "06") → invalid paths.
- Empty string → print accumulated encoding.
---

## 4. Examples

```text
Input:
123
Output:
abc
aw
lc
Explanation:
{1,2,3} → "abc"
{1,23} → "aw"
{12,3} → "lc"

Input:
013
Output:
(no output)
Invalid because it starts with 0.

Input:
303
Output:
(no output)
Invalid because "30" and "03" are not encodable.
```

---

## 5. Approaches

### Approach 1: Recursive Encoding

**Idea:**
- At each step:
  * Take one digit → if between 1–9, convert and recurse.
  * Take two digits → if between 10–26, convert and recurse.
- Base case: when string is empty, print accumulated encoding.

**Steps:**
- If string starts with '0', return (invalid).
- Take one digit → map to a character → recurse on remaining string.
- If two digits form a number ≤ 26 → map and recurse.
- Continue until string becomes empty (print valid encoding).

**Java Code:**
```java
public static void printEncodings(String str, String ans) {
    if (str.length() == 0) {
        System.out.println(ans);
        return;
    }

    if (str.charAt(0) == '0') return; // invalid start

    // Take one digit
    int oneDigit = str.charAt(0) - '0';
    char oneChar = (char)('a' + oneDigit - 1);
    printEncodings(str.substring(1), ans + oneChar);

    // Take two digits if possible
    if (str.length() >= 2) {
        int twoDigit = Integer.parseInt(str.substring(0, 2));
        if (twoDigit <= 26) {
            char twoChar = (char)('a' + twoDigit - 1);
            printEncodings(str.substring(2), ans + twoChar);
        }
    }
}
Initial call:
printEncodings("123", "")

                         ("123","")
                        /           \
              take '1' (a)          take '12' (l)
                  /                      \
         ("23","a")                      ("3","l")
           /        \                         |
 take '2'(b)     take '23'(w)           take '3'(c)
     /                \                     ↓
("3","ab")        ("","aw")             ("","lc")
   |
take '3'(c)
 ↓
("","abc")
```
### 💭 Intuition Behind the Approach

At each step, we either decode **one digit** (`'1' → 'a'`) or **two digits** (`'12' → 'l'`) if valid (`≤ 26`).  
The recursion explores both choices, building all possible letter combinations.  
When the string becomes empty (`str.length() == 0`), it means a full decoding is formed, so we print it.  
We skip `'0'` since it cannot map to any letter.


**Complexity (Time & Space):**
- Time Complexity: O(2ⁿ) — each digit can branch into 1 or 2 calls.
- Space Complexity: O(n) — recursion depth equals string length.

---

## 6. Justification / Proof of Optimality

- Each recursive call represents a choice point:
- Take a 1-digit encoding.
- Take a 2-digit encoding.
- Invalid branches (starting with 0 or >26) terminate early.
- The recursion tree ensures all valid combinations are explored.
- Base case ensures printing only when a full valid decoding is achieved.
---

## 7. Variants / Follow-Ups

- Count Encodings: Return the number of valid encodings instead of printing.
- Return List: Collect and return all encodings in a list.
- Memoized Version: Cache subproblems for optimization when string is large.
- Dynamic Programming: Convert recursion to bottom-up DP for performance.
---

## 8. Tips & Observations

- Always check for '0' at the start of a string/substring — it’s invalid, because no letter maps to 0.
- Branching occurs at each character:
  * 1-digit number (1–9) → valid encoding.
  * 2-digit number (10–26) → valid encoding.
- Prune invalid branches early:
  * Substrings like "03", "30" are not valid → return immediately.
- Use recursion to build the encoded string incrementally:
  * Pass the accumulated string (ans) in each recursive call.
- Lexicographical order is automatically maintained:
  * Always process the 1-digit branch before the 2-digit branch.
- Base case is reached when the string is empty → print the accumulated encoding.
- Recursive depth = length of string → space complexity O(n).
- Number of total branches = 2ⁿ in worst case (each character may split into 1-digit or 2-digit encoding).
- Observation:
  * Some digits may lead to only 1 choice (like '0' cannot start a branch).
  * Strings with repeated digits or multiple valid splits will create multiple valid encodings.
- For optimization:
  * Memoization is useful if you need to count encodings instead of printing them.
  * You can cache the number of encodings for a substring starting at each index.
---

<!-- #endregion -->
