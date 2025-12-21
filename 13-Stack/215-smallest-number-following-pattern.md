<!-- #region 211-Smallest Number Following Pattern -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q215: Smallest Number Following Pattern</h1>

## 1. Problem Statement

- You are given a pattern string str consisting of characters 'i' and 'd'.
  * 'i' means increasing
  * 'd' means decreasing
- You must construct the smallest possible number using digits from 1 to 9 without repetition, such that:
  * digits increase where pattern has 'i'
  * digits decrease where pattern has 'd'
- The length of the resulting number will be str.length + 1.
---

## 2. Problem Understanding

- Pattern length = n
- Result length = n + 1
- Digits must be:
  * unique
  * chosen from 1 to 9
- “Smallest number” means:
  * smallest lexicographically
  * smallest digits as early as possible
- Key observation:
  * A continuous sequence of 'd' forces a reverse ordering of digits.
---

## 3. Constraints

- 1 <= str.length <= 8
- Digits allowed: 1 to 9 (no repetition)
---

## 4. Edge Cases

- All 'i' → straight increasing
- All 'd' → fully decreasing
- Single character pattern
- Pattern ending with 'd'
- Mixed patterns like "iiddd"
---

## 5. Examples

```text
Input:  d
Output: 21

Input:  i
Output: 12

Input:  ddd
Output: 4321

Input:  iiddd
Output: 126543
```

---

## 6. Approaches

### Approach 1: Brute Force (Generate Permutations)

**Idea:**
- Generate all permutations of digits 1–9
- Pick the smallest number that satisfies the pattern

**Steps:**
- Generate permutations of length n+1
- Check pattern validity
- Track smallest valid number

**Java Code:**
```java
public String smallestNumberBrute(String pattern) {
    List<String> res = new ArrayList<>();
    permute("", "123456789", pattern.length() + 1, res);

    String ans = null;
    for (String s : res) {
        if (matches(s, pattern)) {
            if (ans == null || s.compareTo(ans) < 0) ans = s;
        }
    }
    return ans;
}

private void permute(String curr, String rem, int len, List<String> res) {
    if (curr.length() == len) {
        res.add(curr);
        return;
    }
    for (int i = 0; i < rem.length(); i++) {
        permute(curr + rem.charAt(i),
                rem.substring(0, i) + rem.substring(i + 1),
                len, res);
    }
}

private boolean matches(String num, String p) {
    for (int i = 0; i < p.length(); i++) {
        if (p.charAt(i) == 'i' && num.charAt(i) > num.charAt(i + 1)) return false;
        if (p.charAt(i) == 'd' && num.charAt(i) < num.charAt(i + 1)) return false;
    }
    return true;
}
```

**💭 Intuition Behind the Approach:**
- Try all possibilities and pick the smallest valid one

**Complexity (Time & Space):**
- Time: O(9!) — factorial permutations
- Space: O(9!) — storing permutations

### Approach 2: Greedy with Reversal (Better)

**Idea:**
- Start with increasing sequence 1 2 3 ...
- Reverse segments corresponding to 'd'

**Steps:**
- Initialize result array with 1..n+1
- For each continuous 'd' segment:
  * reverse that segment

**Java Code:**
```java
public String smallestNumberGreedy(String str) {
    int n = str.length();
    int[] arr = new int[n + 1];
    for (int i = 0; i <= n; i++) arr[i] = i + 1;

    int i = 0;
    while (i < n) {
        if (str.charAt(i) == 'd') {
            int j = i;
            while (j < n && str.charAt(j) == 'd') j++;
            reverse(arr, i, j);
            i = j;
        } else {
            i++;
        }
    }

    StringBuilder sb = new StringBuilder();
    for (int x : arr) sb.append(x);
    return sb.toString();
}

private void reverse(int[] arr, int l, int r) {
    while (l < r) {
        int t = arr[l];
        arr[l] = arr[r];
        arr[r] = t;
        l++; r--;
    }
}
```

**💭 Intuition Behind the Approach:**
- 'd' means local reversal
- Using smallest digits early ensures minimal number

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

### Approach 3: Stack-Based (Optimal & Clean)

**Idea:**
- Use stack to delay output
- Push increasing digits
- On 'i', flush stack
- Stack naturally reverses 'd' sequences

**Steps:**
- Initialize number counter from 1
- Traverse pattern:
  * Push number to stack
  * If 'i', pop all from stack
- After loop, push last number and pop all

**Java Code:**
```java
public String smallestNumber(String str) {
    StringBuilder ans = new StringBuilder();
    Stack<Integer> st = new Stack<>();

    int num = 1;
    for (int i = 0; i < str.length(); i++) {
        st.push(num++);

        if (str.charAt(i) == 'i') {
            while (!st.isEmpty()) {
                ans.append(st.pop());
            }
        }
    }

    st.push(num);
    while (!st.isEmpty()) {
        ans.append(st.pop());
    }

    return ans.toString();
}
```

**💭 Intuition Behind the Approach:**
- Stack reverses digits for 'd'
- 'i' forces output
- Uses smallest available digits first

**Complexity (Time & Space):**
- Time: O(N)
- Space: O(N)

---

## 7. Justification / Proof of Optimality

- Stack approach is simplest and most intuitive
- Greedy reversal is correct but harder to reason
- Brute force is only for conceptual understanding
---

## 8. Variants / Follow-Ups

- Largest number following pattern
- Pattern with 'i', 'd', '='
- Binary pattern to number mapping
---

## 9. Tips & Observations

- Result length is always pattern.length + 1
- Continuous 'd' means reverse
- Stack is ideal for delayed output problems
---

## 10. Pitfalls

- Forgetting the last digit
- Using digits beyond 9
- Outputting too early on 'd'
- Repeating digits
---

<!-- #endregion -->
