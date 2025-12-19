<!-- #region 204-Extra Brackets -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q204: Extra Brackets</h1>

## 1. Problem Statement

- You are given a string exp representing a balanced expression containing only round brackets '(' and ')' along with operands and operators.
- Some pairs of brackets may be extra / redundant (i.e., they do not affect the evaluation of the expression).
- You need to determine whether the given expression contains extra brackets.
- Return:
  * true → if extra brackets exist
  * false → otherwise
---

## 2. Problem Understanding

- The expression is already balanced
- Only round brackets ( and ) are present
- Extra brackets are those which:
  * Enclose no operator
  * Or enclose a single element unnecessarily
- We must detect redundant parentheses
---

## 3. Constraints

- 1 <= exp.length <= 10000
- Expression contains:
  * lowercase letters
  * operators (+ - * /)
  * round brackets only
---

## 4. Edge Cases

- Single pair of brackets around one variable → extra
- Nested brackets with no operator inside
- Expression with operators inside every bracket → not extra
- Deeply nested expressions
---

## 5. Examples

```text
Example 1

Input:

((a + b) + (c + d))


Output:

false

Example 2

Input:

(a + b) + ((c + d))


Output:

true
```

---

## 6. Approaches

### Approach 1: Stack-Based Check (Optimal)

**Idea:**
- Use a stack
- Push all characters except )
- When ) is found:
  * Pop elements until matching ( is found
  * Check if there was any operator between them
- If no operator is found → extra brackets exist

**Steps:**
- Initialize an empty stack
- Traverse the expression character by character
- If character is not ) → push to stack
- If character is ):
  * Pop elements until ( is found
  * Track if any operator (+ - * /) is found
  * If no operator found → return true
- After traversal → return false

**Java Code:**
```java
class Solution {
    public static boolean ExtraBrackets(String exp) {
        Stack<Character> st = new Stack<>();

        for (char ch : exp.toCharArray()) {

            if (ch != ')') {
                st.push(ch);
            } else {
                boolean hasOperator = false;

                while (!st.isEmpty() && st.peek() != '(') {
                    char top = st.pop();
                    if (top == '+' || top == '-' || top == '*' || top == '/') {
                        hasOperator = true;
                    }
                }

                st.pop(); // pop '('

                if (!hasOperator) {
                    return true; // extra brackets found
                }
            }
        }
        return false;
    }
}

// below is in-class solution 
// Structural Check for Extra Brackets
// ------------------------------------------------
// This solution detects extra brackets by checking for empty brackets "()".
// When a closing ')' is encountered:
// - If the stack top is immediately '(', it means there was nothing inside → extra brackets.
// - Otherwise, elements are popped until '(' is found.
//
// Note:
// This approach works because the expression is assumed to be valid.
// It checks only the presence of content inside brackets,
// not whether that content contains an operator.
// For semantic correctness (checking actual usefulness of brackets),
// refer to the operator-based solution below.

  public boolean ExtraBrackets(String exp) {
        // Write your code here
        Stack<Character> st=new Stack<>();
        for(int i=0;i<exp.length();i++){
            char c=exp.charAt(i);
            if(c==')'){
            if(!st.isEmpty() && st.peek()=='(' ) return true;
            while(!st.isEmpty() && st.peek()!='(' ) st.pop();
            st.pop();
            }else st.push(c);
        }
        return false;
       
    }
```

**💭 Intuition Behind the Approach:**
- Brackets are useful only if they group an operation
- If a pair of brackets contains no operator, they are redundant
- Stack helps process the innermost brackets first
- Early detection avoids unnecessary traversal

**Complexity (Time & Space):**
- Time: O(N) — single traversal of expression
- Space: O(N) — stack storage

---

## 7. Justification / Proof of Optimality

- The stack-based approach correctly identifies redundant brackets by ensuring that every bracket pair encloses at least one operator, fulfilling the problem’s requirement efficiently.
---

## 8. Variants / Follow-Ups

- Remove redundant parentheses
- Valid Parentheses
- Balanced Brackets
- Minimum brackets removal
- Expression evaluation
---

## 9. Tips & Observations

- Balanced expression does NOT mean non-redundant
- Operators are the key to identifying useful brackets
- Always process from innermost to outermost
- Only ) triggers evaluation
---

## 10. Pitfalls

- Forgetting to check operator presence
- Treating nested valid brackets as redundant
- Missing operator characters in check
- Assuming balanced means correct
---

<!-- #endregion -->
