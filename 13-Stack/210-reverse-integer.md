<!-- #region 210-Reverse Integer -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q210: Reverse Integer</h1>

## 1. Problem Statement

- Given a signed 32-bit integer x, return x with its digits reversed.
- If reversing x causes the value to go outside the signed 32-bit integer range
- [-2^31, 2^31 - 1], return 0.
- Important constraints:
  * You are not allowed to use 64-bit integers (long)
  * Must handle negative numbers
  * Must detect overflow
---

## 2. Problem Understanding

- We need to reverse the digits of an integer
- Sign (+ / -) must be preserved
- Leading zeros in the reversed number should be removed automatically
- Overflow must be detected before it happens
- String conversion is allowed logically but not optimal
- Key difficulty:
  * Detecting overflow during number construction, not after
---

## 3. Constraints

- -2^31 <= x <= 2^31 - 1
- Only 32-bit integer operations allowed
---

## 4. Edge Cases

- x = 0 → 0
- x = 120 → 21
- x = -120 → -21
- x = 1534236469 → overflow → 0
- x = -2147483648 → overflow → 0
---

## 5. Examples

```text
Input:  321
Output: 123

Input:  -321
Output: -123

Input:  120
Output: 21
```

---

## 6. Approaches

### Approach 1: Brute Force (String Conversion)

**Idea:**
- Convert number to string
- Reverse characters
- Parse back to integer
- Catch overflow

**Steps:**
- Convert integer to string
- Reverse digits
- Convert back to integer
- If overflow occurs → return 0

**Java Code:**
```java
public int reverseInteger(int x) {
    boolean neg = x < 0;
    String s = Integer.toString(Math.abs(x));
    StringBuilder sb = new StringBuilder(s).reverse();

    try {
        int val = Integer.parseInt(sb.toString());
        return neg ? -val : val;
    } catch (Exception e) {
        return 0;
    }
}
```

**💭 Intuition Behind the Approach:**
- Simple and readable
- Relies on language parsing to detect overflow

**Complexity (Time & Space):**
- Time: O(N) — digits count
- Space: O(N) — string storage
- ❌ Not interview-preferred

### Approach 2: Stack-Based (Practice-Oriented)

**Idea:**
- Extract digits using % 10
- Push digits into stack
- Pop and rebuild reversed number
- Check overflow while rebuilding

**Steps:**
- Extract digits and push into stack
- Pop digits and rebuild number
- Check overflow before multiplication

**Java Code:**
```java
public int reverseInteger(int x) {
    Stack<Integer> st = new Stack<>();
    int num = Math.abs(x);

    while (num > 0) {
        st.push(num % 10);
        num /= 10;
    }

    int rev = 0;
    while (!st.isEmpty()) {
        int d = st.pop();

        if (rev > Integer.MAX_VALUE / 10 ||
           (rev == Integer.MAX_VALUE / 10 && d > 7)) return 0;

        rev = rev * 10 + d;
    }

    return x < 0 ? -rev : rev;
}
```

**💭 Intuition Behind the Approach:**
- Stack reverses order naturally (LIFO)
- Used mainly for stack practice, not optimization

**Complexity (Time & Space):**
- Time: O(N) — digits
- Space: O(N) — stack
- ⚠️ Valid but not optimal

### Approach 3: al (Arithmetic Only — Interview Preferred)

**Idea:**
- Reverse digits using arithmetic only
- Detect overflow before multiplying by 10
- No extra data structures

**Steps:**
- Extract digit using % 10
- Shrink number using / 10
- Check overflow before rev * 10
- Append digit
- Repeat until number becomes 0

**Java Code:**
```java
public int reverseInteger(int x) {
    int rev = 0;

    while (x != 0) {
        int digit = x % 10;
        x /= 10;

        if (rev > Integer.MAX_VALUE / 10 ||
           (rev == Integer.MAX_VALUE / 10 && digit > 7)) return 0;

        if (rev < Integer.MIN_VALUE / 10 ||
           (rev == Integer.MIN_VALUE / 10 && digit < -8)) return 0;

        rev = rev * 10 + digit;
    }

    return rev;
}
```

**💭 Intuition Behind the Approach:**
- % 10 gives digits in reverse order naturally
- Overflow must be predicted before it happens
- Uses constant extra space

**Complexity (Time & Space):**
- Time: O(N) — digits
- Space: O(1) — no extra space

---

## 7. Justification / Proof of Optimality

- Arithmetic solution is optimal in space and clarity
- Stack and string solutions are valid but inferior
- Overflow check is the key challenge, not reversal
---

## 8. Variants / Follow-Ups

- Reverse digits in base k
- Reverse only even digits
- Palindrome number
- Add two reversed numbers
---

## 9. Tips & Observations

- % 10 naturally reverses digit order
- Overflow must be checked before multiply
- Stack usage here is optional, not required
- Avoid long even for temporary storage
---

## 10. Pitfalls

- Checking overflow after building the number
- Using long (violates constraints)
- Forgetting negative range (-8 case)
- Using stack where arithmetic suffices
---

<!-- #endregion -->
