<!-- #region 193-Count Subarrays With Equal 0s, 1s and 2s -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q193: Count Subarrays With Equal 0s, 1s and 2s</h1>

## 1. Problem Statement

- You are given an array of size n containing only 0, 1, and 2.
- You must count how many contiguous subarrays contain equal numbers o
---

## 2. Problem Understanding

- A subarray has equal 0s, 1s, 2s if:
  * count0 = count1 = count2
- Direct counting per subarray is too slow.
- Key trick:
- Instead of tracking absolute counts, track differences:
  * d10 = count1 - count0
  * d21 = count2 - count1
- If at two different indices i and j, we have:
  * (d10_i == d10_j) AND (d21_i == d21_j)
- Then the subarray from (i+1 to j) has equal 0s, 1s, 2s.
- We will use a HashMap to count frequency of each (d10, d21) pair.
- This reduces to counting equal prefix-pattern repeats.
---

## 3. Constraints

- 1 ≤ N ≤ 1e6 → must be O(N)
- Values are only {0,1,2}
---

## 4. Edge Cases

- No valid subarray → answer = 0
- Single element → cannot form equal triplet
- All zeros or all ones or all twos → output 0
- Very large N → memory-efficient HashMap usage required
---

## 5. Examples

```text
Example 1:

2 0 1 2
Subarrays:
[2,0,1] → equal 0s,1s,2s
[0,1,2] → equal 0s,1s,2s
Answer = 2


Example 2:

1 1 2
No such subarray → 0
```

---

## 6. Approaches

### Approach 1: Brute Force (Too Slow)

**Idea:**
- Check all subarrays, count 0s, 1s, 2s

**Steps:**
- For each i, for each j ≥ i, count and check equality.

**Java Code:**
```java
public long countSubarrays(int[] arr) {
    int n = arr.length;
    long ans = 0;

    for (int i = 0; i < n; i++) {
        int c0 = 0, c1 = 0, c2 = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 0) c0++;
            else if (arr[j] == 1) c1++;
            else c2++;
            if (c0 == c1 && c1 == c2) ans++;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Brute force checks all possible subarrays — simple but too slow.

**Complexity (Time & Space):**
- Time: O(N^2) — nested loops scan all subarrays
- Space: O(1) — only counters used

### Approach 2: Prefix Difference Method + HashMap (Optimal)

**Idea:**
- Track two differences:
  * d10 = count1 - count0
  * d21 = count2 - count1
- Whenever the same (d10, d21) pair appears again at index j, a subarray ending at j has equal 0,1,2 counts.
- Use HashMap to count frequencies of (d10, d21).

**Steps:**
- Initialize d10 = 0, d21 = 0.
- Push (0,0) into map with count 1 (prefix before array starts).
- Traverse array:
  * Update counts
  * Update d10, d21
  * Add existing frequency of this pair to answer
  * Increment map frequency

**Java Code:**
```java
public long countSubarrays(int[] arr) {
    Map<String, Long> map = new HashMap<>();
    long ans = 0;

    int c0 = 0, c1 = 0, c2 = 0;

    map.put("0#0", 1L);

    for (int x : arr) {
        if (x == 0) c0++;
        else if (x == 1) c1++;
        else c2++;

        int d10 = c1 - c0;
        int d21 = c2 - c1;

        String key = d10 + "#" + d21;

        if (map.containsKey(key)) {
            ans += map.get(key);
        }

        map.put(key, map.getOrDefault(key, 0L) + 1);
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Equal 0,1,2 counts imply identical difference patterns between two prefix points.
- Matching (d10,d21) ensures balanced increments inside the subarray.

**Complexity (Time & Space):**
- Time: O(N) — single scan with O(1) map ops
- Space: O(N) — storing distinct difference pairs

---

## 7. Justification / Proof of Optimality

- Equal 0s,1s,2s means:
  * (count1 - count0) and (count2 - count1)
- must remain unchanged over a subarray.
- Prefix difference repetition guarantees this condition.
- HashMap ensures we count all such subarrays in linear time.
---

## 8. Variants / Follow-Ups

- Count subarrays with equal 0 and 1
- Count subarrays with equal 0,1,...,k
- Largest subarray with equal 0,1,2
---

## 9. Tips & Observations

- Always store prefix (0,0) initially
- Use string key or a custom pair class
- The difference trick generalizes to k categories
---

## 10. Pitfalls

- Forgetting prefix (0,0) entry
- Incorrectly updating differences
- Using wrong key mapping (must combine both differences)
---

<!-- #endregion -->
