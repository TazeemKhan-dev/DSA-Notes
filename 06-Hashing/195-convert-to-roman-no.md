<!-- #region 195-Convert to Roman No -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q195: Convert to Roman No</h1>

## 1. Problem Statement

- You are given an integer n (1 ≤ n ≤ 3999).
- You must convert it into its Roman numeral representation using these symbols:
  * I = 1
  * V = 5
  * X = 10
  * L = 50
  * C = 100
  * D = 500
  * M = 1000
- Output the roman numeral string.
---

## 2. Problem Understanding

- Roman numeral conversion involves subtracting largest possible values first.
- Key rules:
  * Roman numbers use additive notation (e.g., III = 3, XX = 20)
  * Special subtractive pairs exist:
    * IV = 4
    * IX = 9
    * XL = 40
    * XC = 90
    * CD = 400
    * CM = 900
- Always pick the largest roman value ≤ n, append its symbol, subtract, and continue.
---

## 3. Constraints

- 1 ≤ n ≤ 3999
- Roman system supports numbers only up to 3999 by standard notation.
---

## 4. Edge Cases

- n < 10 → purely simple symbols like I, II, III…
- n containing 4 or 9 → must use subtractive notation
- Largest values like 3888 → repeated M/D/C/X etc.
---

## 5. Examples

```text
Example 1:

Input: 5  
Output: V


Example 2:

Input: 3  
Output: III


Example 3:

Input: 944  
Output: CMXLIV
```

---

## 6. Approaches

### Approach 1: Greedy + Predefined Roman Mapping (Optimal)

**Idea:**
- Use a sorted list of value–symbol pairs and repeatedly subtract the largest possible value from n while building the answer.

**Steps:**
- Create arrays of roman values and symbols in decreasing order.
- For each value:
  * While n >= value, append symbol and subtract value.
- Continue until n becomes 0

**Java Code:**
```java
public String convertToRoman(int n) {
    int[] values = 
        {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};

    String[] symbols = 
        {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    StringBuilder sb = new StringBuilder();

    for (int i = 0; i < values.length; i++) {
        while (n >= values[i]) {
            sb.append(symbols[i]);
            n -= values[i];
        }
    }

    return sb.toString();
}
```

**💭 Intuition Behind the Approach:**
- Roman numerals work in a greedy system — always choose the largest symbol that fits.
- Subtractive symbols (like 900, 400, 90, etc.) ensure correctness without special-case logic.

**Complexity (Time & Space):**
- Time: O(1) — fixed list of 13 values, constant operations
- Space: O(1) — output string at most length ~15

### Approach 2: Digit-wise Mapping (Alternative)

**Idea:**
- Handle each digit place (thousands, hundreds, tens, ones) with predefined patterns

**Java Code:**
```java
public String convertToRoman(int num) {
    String[] thousands = {"", "M", "MM", "MMM"};
    String[] hundreds  = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    String[] tens      = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    String[] ones      = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

    return thousands[num / 1000] +
           hundreds[(num % 1000) / 100] +
           tens[(num % 100) / 10] +
           ones[num % 10];
}
```

**💭 Intuition Behind the Approach:**
- Roman numerals follow a strict pattern per digit place, allowing table lookup.

**Complexity (Time & Space):**
- Time: O(1) — constant digit operations
- Space: O(1) — fixed tables

---

## 7. Justification / Proof of Optimality

- Roman numerals are constructed by repeatedly taking the largest available symbol/value that does not exceed the number; subtracting it and continuing produces a valid representation.
- Including the subtractive pairs (900, 400, 90, 40, 9, 4) in the descending value list makes the greedy choice always correct because those pairs are the only legal ways to represent those values and they prevent invalid long sequences (e.g., 4 should be IV not IIII).
- Therefore a greedy algorithm over the fixed, ordered list of value–symbol pairs is necessary and sufficient and runs in constant time (the list is fixed size).
---

## 8. Variants / Follow-Ups

- Convert Roman to Integer
- Validate Roman numeral format
- Minimal Roman vs extended Roman rules
---

## 9. Tips & Observations

- Roman numerals are inherently greedy-friendly.
- Always include subtractive cases like 900, 400, 90 to simplify logic.
- Keep a fixed value–symbol mapping sorted in descending order.
---

## 10. Pitfalls

- Missing subtractive pairs leads to incorrect output
- Forgetting that the maximum valid number is 3999
- Using inefficient string concatenation instead of StringBuilder
---

<!-- #endregion -->
