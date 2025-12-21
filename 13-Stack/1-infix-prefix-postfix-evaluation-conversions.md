<!-- #region 1-Infix, Prefix & Postfix — Evaluation & Conversions -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q1: Infix, Prefix & Postfix — Evaluation & Conversions</h1>

## 1. Problem Statement

- Expressions can be written in 3 forms:
  * Infix: a + b
  * Postfix: ab+
  * Prefix: +ab
- Stacks help:
- Operands → value / string stack
- Operators → operator stack

- **Helper Rules (IMPORTANT)**
- Operator Precedence
  * +  -  → 1
  * *  /  → 2
- Digit vs Alphabet
  * Character.isDigit(ch)      // '1'...'9'
  * Character.isLetter(ch)     // 'a'...'z'
- 🧪 Operator Utility Methods (Used Everywhere)

```text
static int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

static int apply(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    return a / b;
}

```
---

## 2. Approaches

### Approach 1: Infix → Postfix

**Idea:**
- Operators wait until higher precedence operators are processed
- Parentheses control scope

**Java Code:**
```java
static String infixToPostfix(String exp) {
    Stack<Character> ops = new Stack<>();
    StringBuilder res = new StringBuilder();

    for (char ch : exp.toCharArray()) {
        if (Character.isLetterOrDigit(ch)) {
            res.append(ch);
        } 
        else if (ch == '(') {
            ops.push(ch);
        } 
        else if (ch == ')') {
            while (ops.peek() != '(')
                res.append(ops.pop());
            ops.pop();
        } 
        else {
            while (!ops.isEmpty() && precedence(ops.peek()) >= precedence(ch))
                res.append(ops.pop());
            ops.push(ch);
        }
    }

    while (!ops.isEmpty())
        res.append(ops.pop());

    return res.toString();
}
```

### Approach 2: Infix → Prefix

**Idea:**
- Reverse expression
- Swap ( and )
- Convert to postfix
- Reverse result

**Java Code:**
```java
static String infixToPrefix(String exp) {
    StringBuilder rev = new StringBuilder(exp).reverse();
    for (int i = 0; i < rev.length(); i++) {
        if (rev.charAt(i) == '(') rev.setCharAt(i, ')');
        else if (rev.charAt(i) == ')') rev.setCharAt(i, '(');
    }

    String postfix = infixToPostfix(rev.toString());
    return new StringBuilder(postfix).reverse().toString();
}
```

### Approach 3: Infix Evaluation (Digits Only)

**Java Code:**
```java
static int evaluateInfix(String exp) {
    Stack<Integer> val = new Stack<>();
    Stack<Character> ops = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isDigit(ch)) {
            val.push(ch - '0');
        } 
        else if (ch == '(') {
            ops.push(ch);
        } 
        else if (ch == ')') {
            while (ops.peek() != '(') {
                int b = val.pop(), a = val.pop();
                val.push(apply(a, b, ops.pop()));
            }
            ops.pop();
        } 
        else {
            while (!ops.isEmpty() && precedence(ops.peek()) >= precedence(ch)) {
                int b = val.pop(), a = val.pop();
                val.push(apply(a, b, ops.pop()));
            }
            ops.push(ch);
        }
    }

    while (!ops.isEmpty()) {
        int b = val.pop(), a = val.pop();
        val.push(apply(a, b, ops.pop()));
    }

    return val.peek();
}
```

### Approach 4: Postfix → Infix

**Java Code:**
```java
static String postfixToInfix(String exp) {
    Stack<String> st = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String b = st.pop();
            String a = st.pop();
            st.push("(" + a + ch + b + ")");
        }
    }
    return st.peek();
}
```

### Approach 5: Postfix → Prefix

**Java Code:**
```java
static String postfixToPrefix(String exp) {
    Stack<String> st = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String b = st.pop();
            String a = st.pop();
            st.push(ch + a + b);
        }
    }
    return st.peek();
}
```

### Approach 6: Postfix Evaluation (Digits)

**Java Code:**
```java
static int evaluatePostfix(String exp) {
    Stack<Integer> st = new Stack<>();

    for (char ch : exp.toCharArray()) {
        if (Character.isDigit(ch)) {
            st.push(ch - '0');
        } else {
            int b = st.pop();
            int a = st.pop();
            st.push(apply(a, b, ch));
        }
    }
    return st.peek();
}
```

### Approach 7: Prefix → Infix

**Java Code:**
```java
static String prefixToInfix(String exp) {
    Stack<String> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String a = st.pop();
            String b = st.pop();
            st.push("(" + a + ch + b + ")");
        }
    }
    return st.peek();
}
```

### Approach 8: Prefix → Postfix

**Java Code:**
```java
static String prefixToPostfix(String exp) {
    Stack<String> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);
        if (Character.isLetterOrDigit(ch)) {
            st.push(ch + "");
        } else {
            String a = st.pop();
            String b = st.pop();
            st.push(a + b + ch);
        }
    }
    return st.peek();
}
```

### Approach 9: Prefix Evaluation (Digits)

**Java Code:**
```java
static int evaluatePrefix(String exp) {
    Stack<Integer> st = new Stack<>();

    for (int i = exp.length() - 1; i >= 0; i--) {
        char ch = exp.charAt(i);
        if (Character.isDigit(ch)) {
            st.push(ch - '0');
        } else {
            int a = st.pop();
            int b = st.pop();
            st.push(apply(a, b, ch));
        }
    }
    return st.peek();
}
```

---

## 3. Tips & Observations

- Conversion = stack of strings
- Evaluation = stack of integers
- Postfix → left to right
- Prefix → right to left
- Infix → operator precedence matters
- Alphabet → conversion only
- Digits → evaluation only
---

<!-- #endregion -->
