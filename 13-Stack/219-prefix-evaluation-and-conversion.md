<!-- #region 215-  Prefix Evaluation and Conversion -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q219: Prefix Evaluation and Conversion</h1>

## 1. Problem Statement

- You are given a prefix expression consisting of:
  * single-digit operands (0–9)
  * operators: +, -, *, /
- You must:
  * Evaluate the prefix expression
  * Convert it to an infix expression (with brackets)
  * Convert it to a postfix expression
---

## 2. Problem Understanding

- In prefix notation, operators come before operands
- Each operator always applies to the next two operands
- Operator precedence is already encoded in prefix
- Expression is guaranteed to be valid
- All operands are single-digit, simplifying parsing
- Key insight:
  * Prefix expressions are best processed from right to left using a stack.
---

## 3. Constraints

- Expression is valid
- Operands are single-digit numbers
- Operators: + - * /
---

## 4. Edge Cases

- Single operand only
- Nested prefix expressions
- Order-sensitive operators (-, /)
- Division results are integer-based
---

## 5. Examples

```text
Input:

-+2/*6483


Output:

2
((2+((6*4)/8))-3)
264*8/+3-


Input:

++123


Output:

6
((1+2)+3)
12+3+
```

---

## 6. Approaches

### Approach 1: Brute Force (Expression Tree)

**Idea:**
- Build an expression tree from prefix expression
- Traverse the tree to:
  * evaluate the expression
  * generate infix
  * generate postfix

**Steps:**
- Read prefix expression from right to left
- On operand → create node and push
- On operator:
  * pop two nodes
  * make them left and right children
- Traverse the tree

**Java Code:**
```java
class Node {
    char val;
    Node left, right;
}

public void prefixBrute(String exp) {
    Stack<Node> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char c = exp.charAt(i);
        Node node = new Node();
        node.val = c;

        if (Character.isDigit(c)) {
            st.push(node);
        } else {
            node.left = st.pop();
            node.right = st.pop();
            st.push(node);
        }
    }

    Node root = st.pop();
    System.out.println(eval(root));
    System.out.println(infix(root));
    System.out.println(postfix(root));
}
```

**💭 Intuition Behind the Approach:**
- Prefix naturally maps to an expression tree
- Tree traversal gives all forms

**Complexity (Time & Space):**
- Time: O(N)  Space: O(N)

### Approach 2: Stack-Based (Standard & Optimal)

**Idea:**
- Use stacks to evaluate and convert simultaneously
- Process prefix from right to left
- On operator, combine the last two operands

**Steps:**
- Traverse expression from right to left
- On operand:
  * push into value, infix, postfix stacks
- On operator:
  * pop two operands
  * evaluate value
  * build infix and postfix strings
- Final stack top contains result

**Java Code:**
```java
public void evaluatePrefix(String exp) {

    Stack<Integer> values = new Stack<>();
    Stack<String> infix = new Stack<>();
    Stack<String> postfix = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);

        if (Character.isDigit(ch)) {
            values.push(ch - '0');
            infix.push(ch + "");
            postfix.push(ch + "");
        } else {
            int a = values.pop();
            int b = values.pop();
            values.push(apply(a, b, ch));

            String ia = infix.pop();
            String ib = infix.pop();
            infix.push("(" + ia + ch + ib + ")");

            String pa = postfix.pop();
            String pb = postfix.pop();
            postfix.push(pa + pb + ch);
        }
    }

    System.out.println(values.pop());
    System.out.println(infix.pop());
    System.out.println(postfix.pop());
}
```

**💭 Intuition Behind the Approach:**
- Prefix evaluation requires right-to-left traversal
- Stack ensures correct operand pairing
- The same pop order builds:
  * evaluated value
  * infix expression
  * postfix expression
- Evaluation and conversions stay synchronized

**Complexity (Time & Space):**
- Time: O(N) — each character processed once
- Space: O(N) — stacks store intermediate results

---

## 7. Justification / Proof of Optimality

- Prefix expressions remove precedence ambiguity
- Stack guarantees correct operand order
- No better asymptotic solution exists
---

## 8. Variants / Follow-Ups

- Prefix evaluation only
- Prefix to infix conversion
- Prefix to postfix conversion
- Expression evaluation with variables
---

## 9. Tips & Observations

- Always process prefix from right to left
- Operand order matters for - and /
- Wrap infix expressions in brackets
- Stack size reduces by one on every operator
---

## 10. Pitfalls

- Traversing left to right (wrong)
- Reversing operand order
- Forgetting parentheses in infix
- Treating prefix like postfix
---

<!-- #endregion -->
