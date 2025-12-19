<!-- #region 213-Infix Evaluation and Conversions -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q213: Infix Evaluation and Conversions</h1>

## 1. Problem Statement

- You are given an infix expression consisting of:
  * single-digit operands (0–9)
  * operators: +, -, *, /
  * parentheses ( and )
- You must:
  * Evaluate the infix expression
  * Convert it to postfix expression
  * Convert it to prefix expression
---

## 2. Problem Understanding

- Infix notation places operators between operands
- Operator precedence matters:
  * * and / > + and -
- Parentheses override precedence
- Expression is guaranteed to be valid
- All operands are single-digit, so parsing is simpler
- Key challenge:
  * Handle evaluation + postfix + prefix conversion together while respecting precedence and parentheses.
---

## 3. Constraints

- Expression length ≤ reasonable limits
- Operands: 0–9
- Operators: + - * /
- Expression is valid
---

## 4. Edge Cases

- Nested parentheses
- Expression with only one operand
- Multiple operators with same precedence
- Division producing integer result
---

## 5. Examples

```text
Input:  ((2+((6*4)/8))-3)

Output:
2
264*8/+3-
-+2/*6483



Input:  ((1+2)+((((3*4)/5)*6)/7))

Output:
4
12+34*5/6*7/+
++12/*/*34567
```

---

## 6. Approaches

### Approach 1: Brute Force (Separate Conversions)

**Idea:**
- Convert infix → postfix
- Evaluate postfix
- Convert postfix → prefix

**Steps:**
- Convert infix to postfix using stack
- Evaluate postfix expression
- Convert postfix to prefix

**Java Code:**
```java
public void bruteInfix(String exp) {
    String postfix = infixToPostfix(exp);
    int val = evalPostfix(postfix);
    String prefix = postfixToPrefix(postfix);

    System.out.println(val);
    System.out.println(postfix);
    System.out.println(prefix);
}
(Helper methods omitted for brevity; logic repeats across problems)
```

**💭 Intuition Behind the Approach:**
- Break problem into known sub-problems
- Easy to reason but inefficient and repetitive

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

### Approach 2: Stack-Based (Better, Separate Stacks)

**Idea:**
- Use stacks for:
  * values
  * postfix strings
  * prefix strings
  * operators
- Process operators when precedence rules apply

**Steps:**
- Traverse expression
- Push operands into value/postfix/prefix stacks
- Push operators into operator stack
- On closing parenthesis or precedence violation:
  * pop operator
  * evaluate + build postfix & prefix

**Java Code:**
```java
public void evaluateInfix(String exp) {

    Stack<Integer> values = new Stack<>();
    Stack<String> postfix = new Stack<>();
    Stack<String> prefix = new Stack<>();
    Stack<Character> ops = new Stack<>();

    for (char ch : exp.toCharArray()) {

        if (ch == '(') {
            ops.push(ch);
        }
        else if (Character.isDigit(ch)) {
            values.push(ch - '0');
            postfix.push(ch + "");
            prefix.push(ch + "");
        }
        else if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
            while (!ops.isEmpty() && ops.peek() != '(' &&
                   precedence(ops.peek()) >= precedence(ch)) {
                process(values, postfix, prefix, ops);
            }
            ops.push(ch);
        }
        else if (ch == ')') {
            while (ops.peek() != '(') {
                process(values, postfix, prefix, ops);
            }
            ops.pop();
        }
    }

    while (!ops.isEmpty()) {
        process(values, postfix, prefix, ops);
    }

    System.out.println(values.pop());
    System.out.println(postfix.pop());
    System.out.println(prefix.pop());
}
```

### 💭 Intuition Behind the Approach

- Infix expressions cannot be evaluated directly from left to right because:
  - operator precedence matters
  - parentheses override precedence
- Therefore, an **operator stack** is used to delay operations until they are safe to apply.

- At the same time, whenever an operator is applied:
  - it combines the **last two operands**
  - the same combination logic can be reused to:
    - compute the numeric value
    - build the postfix expression
    - build the prefix expression

- The key idea is:

  > **Whenever an operator becomes ready to execute,  
  > resolve it once and update all three representations together.**

- Multiple stacks are used:
  - `values` → stores evaluated integers
  - `postfix` → stores postfix strings
  - `prefix` → stores prefix strings
  - `ops` → controls when an operation should be applied

- This keeps evaluation and conversions **synchronized** and avoids repeated passes.

---

### ⏱️ Complexity

- **Time:** `O(N)` — each character is pushed and popped at most once across stacks
- **Space:** `O(N)` — stacks store operands, operators, and intermediate expressions

---

### 🔒 Note

> **This approach is already optimal.**  
> Any further “optimal” approach is the same algorithm, just classified differently.
 ---
### Approach 3: Optimal (Single-Pass Multi-Stack)

**Idea:**
- Solve evaluation + postfix + prefix simultaneously
- Each operator resolution does three operations together

**Steps:**
- Maintain 4 stacks:
  * values, postfix, prefix, operators
- On operand:
  * push into all 3 stacks
- On operator:
  * resolve based on precedence
- On ):
  * resolve until (

**Java Code:**
```java
private void process(Stack<Integer> values,
                     Stack<String> postfix,
                     Stack<String> prefix,
                     Stack<Character> ops) {

    char op = ops.pop();

    int b = values.pop();
    int a = values.pop();
    values.push(apply(a, b, op));

    String pb = postfix.pop();
    String pa = postfix.pop();
    postfix.push(pa + pb + op);

    String prb = prefix.pop();
    String pra = prefix.pop();
    prefix.push(op + pra + prb);
}

private int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    return 2;
}

private int apply(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    return a / b;
}
```

**💭 Intuition Behind the Approach:**
- Every operator combines:
  * two values
  * two postfix strings
  * two prefix strings
- Synchronizing them avoids redundant passes

**Complexity (Time & Space):**
- Time: O(N) — each character processed once
- Space: O(N) — stacks

---

## 7. Justification / Proof of Optimality

- Infix expressions require stack due to precedence
- Multi-stack solution avoids repeated conversions
- Clean and scalable approach
---

## 8. Variants / Follow-Ups

- Infix → Postfix only
- Infix → Prefix only
- Expression evaluation with variables
- Multi-digit operands
---

## 9. Tips & Observations

- Always resolve higher precedence first
- Parentheses force immediate resolution
- Prefix & postfix are mirror forms of expression tree
---

## 10. Pitfalls

- Forgetting operator precedence
- Reversing operand order (a op b)
- Ignoring parentheses
- Doing evaluation after full conversion
---

<!-- #endregion -->
