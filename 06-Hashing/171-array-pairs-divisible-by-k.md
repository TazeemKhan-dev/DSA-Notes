<!-- #region 171-Array Pairs Divisible By K -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q171: Array Pairs Divisible By K</h1>

## 1. Problem Statement

- You are given an integer array arr of even length n and an integer k.
- You must split the entire array into n/2 disjoint pairs such that each pair’s sum is divisible by k.
- Return true if such a pairing is possible, otherwise return false.
---

## 2. Problem Understanding

- We must pair up all elements. No element can be left unused.
- Each pair (a, b) must satisfy:
- (a + b) % k == 0
- This means:
  * If a % k = r, then b % k must be k - r, except:
    * If r == 0, then both elements in the pair must also have remainder 0.
    * If r == k/2 (only when k is even), then both elements must have the same remainder k/2.
- We're essentially pairing based on remainders modulo k.
- This is a frequency-matching problem, not a sorting or two-pointer problem.
---

## 3. Constraints

- 1 <= n, k <= 100000
- n is always even
- 1 <= arr[i] <= 1000000
---

## 4. Edge Cases

- Elements divisible by k must appear in even count.
- If k is even, elements with remainder k/2 must also appear in even count.
- Remainder frequencies must match: freq[r] == freq[k-r].
- If any remainders mismatch → pairing impossible.
---

## 5. Examples

```text
Given:
arr = [1,2,3,4,5,10,6,7,8,9], k = 5

Pairs:
(1,9), (2,8), (3,7), (4,6), (5,10)
All sums divisible by 5.
```

---

## 6. Approaches

### Approach 1: Frequency of Remainders (Optimal & Standard)

**Idea:**
- Two numbers a and b form a valid pair if:
- (a + b) % k == 0
- → remainders must satisfy:
- (a % k) + (b % k) == k (or 0 for exact matches).
- So:
  * Count remainders.
  * For every remainder r, ensure freq[r] == freq[k-r].
  * Handle special remainders: 0 and k/2 separately.

**Steps:**
- Create an array freq[k] to store counts of remainders.
- Loop over array, compute arr[i] % k, increment frequency.
- Check rules:
  * If freq[0] is odd → return false.
  * If k % 2 == 0 and freq[k/2] is odd → return false.
  * For each r from 1 to k-1:
    * Check if freq[r] == freq[k-r]. If not → return false.
- Otherwise return true.

**Java Code:**
```java
public boolean canArrange(int[] arr, int k) {
    int[] freq = new int[k];

    for (int x : arr) {
        int r = x % k;
        if (r < 0) r += k; // handle negatives
        freq[r]++;
    }

    if (freq[0] % 2 != 0) return false;

    if (k % 2 == 0 && freq[k / 2] % 2 != 0) return false;

    for (int r = 1; r < k; r++) {
        if (r == k - r) continue;
        if (freq[r] != freq[k - r]) return false;
    }

    return true;
}
```

**💭 Intuition Behind the Approach:**
- The only thing that matters is remainders.
- If two numbers must sum to a multiple of k, their remainders must complement each other.
- So the whole problem reduces to validating if remainder frequencies match up properly.

**Complexity (Time & Space):**
- Time: O(n + k)
  * O(n) to build frequencies
  * O(k) to validate
- Space: O(k)
  * Frequency array

### Approach 2: HashMap instead of Array (Same Logic, Slower)

**Idea:**
- Same as above but using HashMap instead of fixed array.

**Java Code:**
```java
public boolean canArrange(int[] arr, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int x : arr) {
        int r = x % k;
        if (r < 0) r += k;
        map.put(r, map.getOrDefault(r, 0) + 1);
    }

    if (map.getOrDefault(0, 0) % 2 != 0) return false;

    for (int r : map.keySet()) {
        if (r == 0) continue;
        int complement = (k - r) % k;

        if (!map.get(r).equals(map.getOrDefault(complement, 0)))
            return false;
    }

    return true;
}
```

**💭 Intuition Behind the Approach:**
- Same remainder-complement rule, different storage.

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(k)

---

## 7. Justification / Proof of Optimality

- The remainder-pairing method is mathematically guaranteed to be correct because it's derived directly from modular arithmetic properties.
- If remainder frequencies satisfy all pairing rules, valid pairs must exist.
- No other approach is simpler or faster.
---

## 8. Variants / Follow-Ups

- Find actual pairs instead of boolean result.
- Count how many valid pairings exist.
- Return pairs with minimum difference but still divisible by k.
- Multi-set pairing with constraints on number of pairs.
---

## 9. Tips & Observations

- Always normalize negative remainders (r < 0 ? r + k).
- Remainder-based problems almost always use frequency buckets.
- Special remainders (0, k/2) must be even in count because they can pair only with themselves.
---

## 10. Pitfalls

- Forgetting to normalize negative numbers (-3 % 5 = -3, not valid as a remainder).
- Forgetting special cases: 0 and k/2.
- Directly trying to sort + two-pointer — this problem is not solvable with two pointers reliably.
---

<!-- #endregion -->
