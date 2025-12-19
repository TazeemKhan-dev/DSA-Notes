<!-- #region 205-Valid Parenthesis String -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q205: Valid Parenthesis String</h1>

## 1. Problem Statement

- Given a string s containing only three characters '(', ')', and '*', determine whether the string is valid.
- Rules for validity:
- Every '(' must have a matching ')'
- Every ')' must have a matching '('
- '(' must come before its matching ')'
- '*' can act as:
  * '('
  * ')'
  * empty string ""
- Return true if the string is valid, otherwise return false.
---

## 2. Problem Understanding

- We must validate balanced parentheses, but with a twist:
  * '*' is flexible
- Order matters → '(' must appear before ')'
- We must check existence of at least one valid interpretation
- This is not about counting exact matches, but about possibility
---

## 3. Constraints

- 1 <= s.length <= 100
- s[i] ∈ { '(', ')', '*' }
---

## 4. Edge Cases

- String with only '*' → valid
- More ')' than possible '(' at any point → invalid
- Unmatched '(' at end → invalid
- '*' before '(' cannot act as its closing ')'
---

## 5. Examples

```text
"()" → true

"(*)" → true

"(*))" → true

"())" → false

"*)(" → false
```

---

## 6. Approaches

### Approach 1: Stack-Based (Two Stacks)

**Idea:**
- Track positions of:
  * '(' in one stack
  * '*' in another stack
- When ')' appears  :
  * Prefer matching '('
  * Else use '*' as '('
- After traversal:
  * Match remaining '(' with '*' acting as ')'
  * Ensure '*' comes after '(' (index order)

**Steps:**
- Traverse the string
- Push indices of '(' and '*'
- On ')':
  * Pop '(' if possible
  * Else pop '*'
  * Else return false
- After traversal:
  * Match leftover '(' with '*' (order check)
- If unmatched '(' remains → invalid

**Java Code:**
```java
public static boolean checkValidString(String s) {
    Stack<Integer> open = new Stack<>();
    Stack<Integer> star = new Stack<>();

    for (int i = 0; i < s.length(); i++) {
        char ch = s.charAt(i);

        if (ch == '(') {
            open.push(i);
        } else if (ch == '*') {
            star.push(i);
        } else { // ')'
            if (!open.isEmpty()) {
                open.pop();
            } else if (!star.isEmpty()) {
                star.pop();
            } else {
                return false;
            }
        }
    }

    while (!open.isEmpty() && !star.isEmpty()) {
        if (open.peek() > star.peek()) return false;
        open.pop();
        star.pop();
    }

    return open.isEmpty();
}
```

**💭 Intuition Behind the Approach:**
- '(' and ')' need order-aware matching
- '*' acts as a buffer
- Index comparison enforces correct bracket order

**Complexity (Time & Space):**
- Time: O(N) — each character is pushed/popped once
- Space: O(N) — stacks store indices

### Approach 2: Greedy Range Method (Optimal)

**Idea:**
- Track a range of possible open brackets:
  * minOpen → minimum possible '('
  * maxOpen → maximum possible '('
- '*' expands the range
- If range becomes invalid → return false

**Steps:**
- Initialize minOpen = 0, maxOpen = 0
- Traverse characters:
  * '(' → both increase
  * ')' → both decrease
  * '*' → minOpen--, maxOpen++
- If maxOpen < 0 → invalid
- Clamp minOpen to 0
- At end, if minOpen == 0 → valid

**Java Code:**
```java
public static boolean checkValidString(String s) {
    int minOpen = 0, maxOpen = 0;

    for (char ch : s.toCharArray()) {
        if (ch == '(') {
            minOpen++;
            maxOpen++;
        } else if (ch == ')') {
            minOpen--;
            maxOpen--;
        } else { // '*'
            minOpen--;
            maxOpen++;
        }

        if (maxOpen < 0) return false;
        if (minOpen < 0) minOpen = 0;
    }

    return minOpen == 0;
}
```

**💭 Intuition Behind the Approach:**
- We don’t care about exact matching — only possibility
- Track best and worst cases simultaneously
- If even the best case fails → impossible

**Complexity (Time & Space):**
- Time: O(N) — single traversal
- Space: O(1) — no extra data structures

---

## 7. Justification / Proof of Optimality

- Stack approach guarantees correctness via explicit matching
- Greedy approach compresses all valid stack states into a range
- Both correctly validate existence of at least one valid interpretation
---

## 8. Variants / Follow-Ups

- Valid Parentheses ((, ))
- Balanced Brackets (()[]{})
- Remove Invalid Parentheses
- Minimum Add to Make Parentheses Valid
---

## 9. Tips & Observations

- If order matters, stacks are natural
- If existence of solution matters, greedy often works
- '*' is not fixed, treat it as a range contributor
---

## 10. Pitfalls

- Treating this as a normal parentheses problem
- Forgetting '*' can be empty
- Not enforcing index order in stack approach
- Assuming greedy means guessing — it’s bounded logic
---

<!-- #endregion -->
