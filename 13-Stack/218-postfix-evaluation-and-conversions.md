<!-- #region 214-Postfix Evaluation And Conversions   -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q218: Postfix Evaluation And Conversions</h1>

## 1. Problem Statement

- You are given a postfix expression consisting of:
  * single-digit operands (0–9)
  * operators: +, -, *, /
- You must:
  * Evaluate the postfix expression
  * Convert it to an infix expression (with brackets)
  * Convert it to a prefix expression
---

## 2. Problem Understanding

- In postfix notation, operators come after operands
- Operator precedence is already encoded in postfix
- No parentheses are present in input
- Every operator always applies to the two most recent operands
- Key insight:
  * Postfix expressions are evaluated naturally using a stack.
---

## 3. Constraints

- Expression is valid
- Operands are single-digit numbers
- Operators: + - * /
---

## 4. Edge Cases

- Single operand only
- Nested operations
- Division producing integer result
- Order-sensitive operators (-, /)
---

## 5. Examples

```text
Input:  264*8/+3-

Output:
2
((2+((6*4)/8))-3)
-+2/*6483


Input:  12+34*5/6*7/+

Output:
4
((1+2)+((((3*4)/5)*6)/7))
++12/*/*34567
```

---

## 6. Approaches

### Approach 1: Brute Force (Expression Tree)

**Idea:**
- Build an expression tree from postfix
- Traverse tree to:
  * evaluate
  * generate infix
  * generate prefix

**Steps:**
- Create tree nodes for operands
- On operator:
  * pop two nodes
  * make them children
- Traverse tree

**Java Code:**
```java
class Node {
    char val;
    Node left, right;
}

public void postfixBrute(String exp) {
    Stack<Node> st = new Stack<>();

    for (char c : exp.toCharArray()) {
        Node node = new Node();
        node.val = c;

        if (Character.isDigit(c)) {
            st.push(node);
        } else {
            node.right = st.pop();
            node.left = st.pop();
            st.push(node);
        }
    }

    Node root = st.pop();
    System.out.println(eval(root));
    System.out.println(infix(root));
    System.out.println(prefix(root));
}
```

**💭 Intuition Behind the Approach:**
- Postfix naturally represents an expression tree
- Tree traversal gives all notations

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

### Approach 2: Stack-Based (Standard)

**Idea:**
- Use three stacks:
  * values
  * infix strings
  * prefix strings
- Process postfix left to right

**Steps:**
- Traverse postfix expression
- On operand:
  * push into all stacks
- On operator:
  * pop two operands
  * evaluate
  * build infix & prefix

**Java Code:**
```java
import java.util.*;

class Node {
    char val;
    Node left, right;

    Node(char v) {
        val = v;
        left = right = null;
    }
}

public class ExpressionTreeFromPostfix {

    // Main brute method
    public void postfixBrute(String exp) {
        Stack<Node> st = new Stack<>();

        for (char c : exp.toCharArray()) {
            Node node = new Node(c);

            if (Character.isDigit(c)) {
                st.push(node);
            } else {
                node.right = st.pop();
                node.left = st.pop();
                st.push(node);
            }
        }

        Node root = st.pop();

        System.out.println(eval(root));
        System.out.println(infix(root));
        System.out.println(prefix(root));
    }

    // Evaluate expression tree
    private int eval(Node root) {
        if (root.left == null && root.right == null) {
            return root.val - '0';
        }

        int left = eval(root.left);
        int right = eval(root.right);

        if (root.val == '+') return left + right;
        if (root.val == '-') return left - right;
        if (root.val == '*') return left * right;
        if (root.val == '/') return left / right;

        return 0;
    }

    // Infix traversal
    private String infix(Node root) {
        if (root.left == null && root.right == null) {
            return String.valueOf(root.val);
        }

        String left = infix(root.left);
        String right = infix(root.right);

        return "(" + left + root.val + right + ")";
    }

    // Prefix traversal
    private String prefix(Node root) {
        if (root.left == null && root.right == null) {
            return String.valueOf(root.val);
        }

        String left = prefix(root.left);
        String right = prefix(root.right);

        return root.val + left + right;
    }

    // Driver
    public static void main(String[] args) {
        ExpressionTreeFromPostfix obj = new ExpressionTreeFromPostfix();

        // Example: postfix of 1+2+(3*4) → 12+34*+
        obj.postfixBrute("12+34*+");
    }
}

```
### 💭 Intuition Behind the Approach (Approach 2: Stack-Based)

- In postfix expressions, **operator precedence is already resolved**, so no operator stack is needed.
- The expression is processed **left to right**:
  - Operands are pushed onto stacks.
  - When an operator appears, it always applies to the **two most recent operands**.
- The same pop order (`left operand`, `right operand`) can be reused to:
  - compute the numeric value
  - construct the infix expression (with brackets)
  - construct the prefix expression
- Using **three parallel stacks** keeps all representations synchronized:
  - `values` stack → evaluates the expression
  - `infix` stack → builds bracketed infix form
  - `prefix` stack → builds prefix form

> **Key idea:**  
> Every operator resolution combines exactly two operands, and that single combination updates
> evaluation, infix, and prefix simultaneously.

---

### ⏱️ Complexity

- **Time:** `O(N)`  
  — each character of the postfix expression is pushed and popped at most once

- **Space:** `O(N)`  
  — stacks store operands and intermediate infix/prefix strings

---

### 🔒 Note

> This approach is both **standard and optimal** for postfix evaluation and conversions.  
> Any further “optimal” approach is the same algorithm, just labeled differently.

---
### Approach 3: Optimal (Single-Pass Multi-Stack)

**Idea:**
- Same as Approach 2, but recognized as optimal
- No precedence handling needed

**Java Code:**
```java
private int apply(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    return a / b;
}
```

**💭 Intuition Behind the Approach:**
- private int apply(int a, int b, char op) {
    * if (op == '+') return a + b;
    * if (op == '-') return a - b;
    * if (op == '*') return a * b;
    * return a / b;
- }

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

---

## 7. Justification / Proof of Optimality

- Postfix evaluation is naturally stack-based
- No need for operator precedence handling
- Clean, linear-time solution
---

## 8. Variants / Follow-Ups

- Postfix evaluation is naturally stack-based
- No need for operator precedence handling
- Clean, linear-time solution
---

## 9. Tips & Observations

- Always pop right operand first
- Wrap infix expressions in brackets
- Order matters for - and /
---

## 10. Pitfalls

- Reversing operand order
- Forgetting parentheses in infix
- Treating postfix like infix
- Using recursion unnecessarily
---

<!-- #endregion -->
