<!-- #region 196-Distinct Numbers in Window -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q196: Distinct Numbers in Window</h1>

## 1. Problem Statement

- You are given an array of N integers and a window size k.
- For every window A[i .. i+k-1], you must print the count of distinct elements.
- Return an array of length N - k + 1.
- If k > N, return an empty array.
---

## 2. Problem Understanding

- This is a classic sliding window problem where we must efficiently maintain:
  * The number of distinct elements in a window of size k
  * Sliding the window right by 1 means:
    * Remove the element leaving the window
    * Add the element entering the window
    * Update distinct count accordingly
  * A HashMap (or HashMap-like frequency map) is ideal:
    * Increase count when adding new element
    * Decrease count when removing element
    * When frequency becomes 0 → element is no longer in window → distinct--
- This gives O(N) performance.
---

## 3. Constraints

- 1 ≤ N, k ≤ 1e5
- Array values up to 1e9
- Must be O(N)
---

## 4. Edge Cases

- k > N → empty result
- All elements same → every window has distinct = 1
- All elements different → every window has distinct = k
- Large N → must avoid O(N*k)
---

## 5. Examples

```text
Example 1:

A = [1,2,1,3,4,3], k = 3
Output = [2,3,3,2]


Example 2:

A = [1,2,3,2], k = 1
Output = [1,1,1,1]
```

---

## 6. Approaches

### Approach 1: Sliding Window + HashMap (Optimal)

**Idea:**
- Maintain a frequency map for the current window:
  * Insert elements until the window reaches size k
  * Distinct count = number of keys with freq > 0
  * Slide window:
    * Remove outgoing element (reduce frequency or delete)
    * Add incoming element (increase frequency)
  * Append distinct count at each step

**Steps:**
- Use a HashMap<Integer, Integer> as a frequency map.
- Process first k elements → fill map → record distinct count.
- For each next position:
  * Remove old element: decrement freq; if freq = 0 → remove key.
  * Add new element: increment freq.
  * Append map.size() to result.
- Return result list/array.

**Java Code:**
```java
public List<Integer> distinctInWindow(int[] arr, int k) {
    int n = arr.length;
    List<Integer> res = new ArrayList<>();
    if (k > n) return res;

    Map<Integer, Integer> freq = new HashMap<>();

    // First window
    for (int i = 0; i < k; i++) {
        freq.put(arr[i], freq.getOrDefault(arr[i], 0) + 1);
    }
    res.add(freq.size());

    // Slide the window
    for (int i = k; i < n; i++) {

        // Remove outgoing element
        int out = arr[i - k];
        freq.put(out, freq.get(out) - 1);
        if (freq.get(out) == 0) freq.remove(out);

        // Add incoming element
        int in = arr[i];
        freq.put(in, freq.getOrDefault(in, 0) + 1);

        res.add(freq.size());
    }

    return res;
}
```

**💭 Intuition Behind the Approach:**
- The window changes by only one element at a time.
- So instead of recalculating distinct values for every window, maintain frequencies incrementally:
  * Add the entering element
  * Remove the leaving element
- HashMap size naturally tracks the number of distinct elements.
- This converts an O(N*k) brute force into O(N).

**Complexity (Time & Space):**
- Time: O(N) — each element is inserted and removed at most once from the map
- Space: O(k) — frequency map stores at most k distinct elements

### Approach 2: Brute Force (Too Slow)

**Idea:**
- For each window, use a Set to count distinct elements

**Java Code:**
```java
public List<Integer> distinctInWindow(int[] arr, int k) {
    int n = arr.length;
    List<Integer> res = new ArrayList<>();
    if (k > n) return res;

    for (int i = 0; i <= n - k; i++) {
        Set<Integer> set = new HashSet<>();
        for (int j = i; j < i + k; j++) {
            set.add(arr[j]);
        }
        res.add(set.size());
    }

    return res;
}
```

**💭 Intuition Behind the Approach:**
- Directly count distinct values in each window, but recomputing every window independently causes large repeated work.

**Complexity (Time & Space):**
- Time: O(N*k) — scanning k elements for each of the N-k+1 windows
- Space: O(k) — set stores up to k elements

---

## 7. Justification / Proof of Optimality

- Distinct count changes only when elements enter or leave the window.
- HashMap frequencies allow constant-time update of distinctness (via map size), so sliding the window with incremental updates is both necessary and sufficient to achieve O(N) time.
- Brute force repeats work; HashMap avoids recomputation.
---

## 8. Variants / Follow-Ups

- Count distinct elements in window size K for strings
- Count distinct numbers in every subarray of varying window sizes
- Sliding window max/min
- Frequency sliding window problems (e.g., longest substring with K unique characters)
---

## 9. Tips & Observations

- Removing outgoing element is crucial; forgetting to delete freq=0 breaks the distinct count.
- Works well even with very large numbers since HashMap handles arbitrary values.
- Suitable for all sliding-window frequency problems.
---

## 10. Pitfalls

- Not removing element when frequency hits zero
- Using map.size() incorrectly (should reflect distinct count)
- k > N must return empty array
---

<!-- #endregion -->
