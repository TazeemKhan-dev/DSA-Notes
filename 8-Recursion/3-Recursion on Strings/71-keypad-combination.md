<!-- #region 71-Keypad Combination -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q71: Keypad Combination</h1>

## 1. Problem Understanding

- Given a string str consisting of digits (0–9), print all possible combinations of letters corresponding to each digit based on a phone keypad mapping.
- Each digit maps to a specific set of characters.
- For example:
- 0 → .;
- 1 → abc
- 2 → def
- 3 → ghi
- 4 → jkl
- 5 → mno
- 6 → pqrs
- 7 → tu
- 8 → vwx
- 9 → yz
- We must generate all possible strings that could be formed by pressing the given keys in sequence.
---

## 2. Constraints

- 0 <= str.length() <= 10
- Input string contains digits only.
---

## 3. Edge Cases

- Empty input string → should return nothing (no output).
- Digits like 0 or 1 → still must map correctly using given mapping.
- Long strings (length ≥ 10) → handle via recursion efficiently.
---

## 4. Examples

```text
Input:
78
Output:
tv
tw
tx
uv
uw
ux
```

---

## 5. Approaches

### Approach 1: Recursive Expansion (Character Mapping + Combination)

**Idea:**
- For each digit, get all possible characters it can represent.
- Recursively solve for the remaining string and append each possible combination.

**Steps:**
- Base Case → If str is empty, print "" (an empty string as one valid combination).
- Get first digit (e.g., '7' → "tu").
- Recursively call for the remaining substring.
- Combine each character of the current digit with all strings returned from smaller problem.

**Java Code:**
```java
import java.util.*;

public class Main {
    static String[] codes = {".;", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tu", "vwx", "yz"};

    public static void printKPC(String str, String ans) {
        // Base case
        if (str.length() == 0) {
            System.out.println(ans);
            return;
        }

        // Current digit
        char ch = str.charAt(0);
        String code = codes[ch - '0'];

        // Smaller problem
        String ros = str.substring(1);

        // Combine current digit's characters with recursive results
        for (int i = 0; i < code.length(); i++) {
            printKPC(ros, ans + code.charAt(i));
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        printKPC(str, "");
    }
}
```

**Complexity (Time & Space):**
- Time Complexity:
- O(kⁿ) — where k is average number of characters per key (max 4), and n is length of input string.
- Each character can branch into multiple recursive calls.
- Space Complexity:
- O(n) — recursion depth (stack space).

### Approach 2: Concise Recursive Template (Contest Friendly)

**Java Code:**
```java
static String[] codes = {".;", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tu", "vwx", "yz"};

static void printKPC(String str, String ans) {
    if (str.isEmpty()) {
        System.out.println(ans);
        return;
    }
    for (char ch : codes[str.charAt(0) - '0'].toCharArray())
        printKPC(str.substring(1), ans + ch);
}
```

**Complexity (Time & Space):**
- Time Complexity:
- O(kⁿ) — where k is average number of characters per key (max 4), and n is length of input string.
- Each character can branch into multiple recursive calls.
- Space Complexity:
- O(n) — recursion depth (stack space).

---

## 6. Justification / Proof of Optimality

- Base case handles termination cleanly.
- Each recursion level branches according to key mapping — pure combinatorial recursion.
- Common recursion pattern: “one choice per character → recurse on the rest.”
---

## 7. Variants / Follow-Ups

- Return list of all combinations instead of printing them.
- Add support for invalid digits.
- Lexicographical sorting of results.
---

## 8. Tips & Observations

- Whenever the problem says “print all combinations” or “all possible outputs”, think recursion/backtracking.
- The number of recursive calls = product of number of mappings per digit.
- The base case (empty string) is crucial — it marks one complete combination.
- Use String[] codes array globally to avoid repeated creation in each call.
- The pattern here is similar to subsequence generation → choose one char per level and recurse.
- For interviews: this problem tests your recursive tree visualization — one node per digit, each branch per possible letter.
---

<!-- #endregion -->
