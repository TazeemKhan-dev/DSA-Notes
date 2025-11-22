<!-- #region 140-Floor in a Sorted Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q140: Floor in a Sorted Array</h1>

## 1. Problem Understanding

- You are given a sorted array without duplicates and a value x.
- You must find the floor of x:
  * Floor of x = largest element ≤ x.
- Return its index (0-based).
- If it doesn’t exist → return -1.
---

## 2. Constraints

- 1 ≤ N ≤ 1e5
- Array sorted strictly increasing
- 0 ≤ x ≤ arr[N-1]
---

## 3. Edge Cases

- x < smallest element → return -1
- x > largest element → return index of last element
- Exact match exists → return index
- Single-element array
- Missing element but floor exists (example: x=5, array=[1,2,8…])
---

## 4. Examples

```text
Example 1

Input

7 0
1 2 8 10 11 12 19


Output

-1

Example 2

Input

7 5
1 2 8 10 11 12 19


Output

1

Example 3

Input

7 10
1 2 8 10 11 12 19


Output

3
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Scan from left until elements exceed x.

**Steps:**
- Loop through array
- Track last index with arr[i] <= x
- Return index

**Java Code:**
```java
public int floorIndex(int[] arr, int x) {
    int ans = -1;
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] <= x) ans = i;
        else break;
    }
    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Since array is sorted, once you exceed x, you won’t find a valid floor later.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n)
  * Might scan whole array.
- 💾 Space Complexity
  * O(1)

### Approach 2: Binary Search (Optimal)

**Idea:**
- Use binary search to find last element ≤ x.

**Steps:**
- Initialize low=0, high=n-1, ans=-1
- While low ≤ high:
  * Compute mid
  * If arr[mid] ≤ x:
    * Record ans = mid
    * Go right → low = mid + 1
  * Else (arr[mid] > x):
    * Go left → high = mid - 1
- Return ans

**Java Code:**
```java
public int floorIndex(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] <= x) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- This is a boundary search:
- We want the rightmost value that is still ≤ x.
- Binary search lets us jump over large sections to find this efficiently.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Binary search halves the space each step.
- 💾 Space Complexity
- O(1)
- Constant space.

---

## 6. Justification / Proof of Optimality

- Binary search is ideal because the array is sorted and we need to find a boundary position.
- This ensures the best efficiency (O(log n)).
---

## 7. Variants / Follow-Ups

- Find ceil of x (smallest ≥ x)
- Find first element greater than x
- Lower bound / upper bound problems
- Find last occurrence of a number
---

## 8. Tips & Observations

- Floor → go right when arr[mid] <= x
- Ceil → go left when arr[mid] >= x
- Boundary problems always use modified binary search
- Always store ans before moving
---

<!-- #endregion -->
