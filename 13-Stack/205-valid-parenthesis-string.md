<!-- #region 205-Valid Parenthesis String -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q205: Valid Parenthesis String</h1>

## 1. Problem Statement

- Given a string s containing only three characters '(', ')', and '*', determine whether the string is valid.
- Rules for validity:
  * Every '(' must have a matching ')'
  * Every ')' must have a matching '('
  * '(' must come before its matching ')'
  * '*' can act as:
    * '('
    * ')'
    * empty string ""
- Print true if the string is valid, otherwise print false.
---

## 2. Problem Understanding

- This is an extension of the Balanced Parentheses problem
- The wildcard '*' introduces multiple possibilities
- We must check if at least one interpretation of '*' makes the string valid
- Brute-force substitution is not feasible
- We need a greedy / range-based approach
---

## 3. Constraints

- 1 <= s.length <= 100
- s[i] ∈ { '(', ')', '*' }
---

## 4. Edge Cases

- String with only '*'
- String starting with ')'
- More ')' than '(' at any point
- All '*' treated as empty
- Unmatched '(' at the end
---

## 5. Examples

```text
Example 1

Input:

2
()


Output:

true

Example 2

Input:

4
((*)


Output:

true


Explanation:
'*' is treated as ')'.
```

---

## 6. Approaches

### Approach 1: Greedy Range Tracking (Optimal)

**Idea:**
- Track the range of possible open parentheses
- Maintain two counters:
- minOpen → minimum possible '(' count
- maxOpen → maximum possible '(' count
- '*' increases flexibility by affecting both bounds

**Steps:**
- Initialize minOpen = 0, maxOpen = 0
- Traverse string character by character
- For '(':
  * minOpen++
  * maxOpen++
- For ')':
  * minOpen--
  * maxOpen--
- For '*':
  * minOpen-- (treat as ')')
  * maxOpen++ (treat as '(')
- If maxOpen < 0 → return false
- Clamp minOpen to 0 if it becomes negative
- After traversal:
  * If minOpen == 0 → valid
  * Else → invalid

**Java Code:**
```java
class Solution {
    public static boolean checkValidString(String s) {
        int minOpen = 0;
        int maxOpen = 0;

        for (char ch : s.toCharArray()) {
            if (ch == '(') {
                minOpen++;
                maxOpen++;
            } else if (ch == ')') {
                minOpen--;
                maxOpen--;
            } else { // '*'
                minOpen--;   // treat as ')'
                maxOpen++;   // treat as '('
            }

            if (maxOpen < 0) return false;
            if (minOpen < 0) minOpen = 0;
        }

        return minOpen == 0;
    }
}
```

**💭 Intuition Behind the Approach:**
- maxOpen assumes '*' acts as '('
- minOpen assumes '*' acts as ')'
- If even the maximum possibility fails → invalid
- If the minimum open count can reach 0 → valid
- This ensures at least one valid configuration exists

**Complexity (Time & Space):**
- Time: O(N) — single pass through the string
- Space: O(1) — constant extra space

---

## 7. Justification / Proof of Optimality

- The greedy range-based approach efficiently checks all possible interpretations of '*' without explicitly generating them, ensuring correctness within optimal time and space constraints.
---

## 8. Variants / Follow-Ups

- Balanced Parentheses
- Extra Brackets
- Remove Invalid Parentheses
- Longest Valid Parentheses
- Parentheses with wildcard constraints
---

## 9. Tips & Observations

- Never let maxOpen go negative
- Clamp minOpen to zero
- '*' increases ambiguity, not complexity
- This is a range feasibility problem, not a pure stack problem
---

## 10. Pitfalls

- Treating '*' as a fixed character
- Using stack-based brute force
- Forgetting to clamp minOpen
- Assuming '*' must always be used
---

<!-- #endregion -->
