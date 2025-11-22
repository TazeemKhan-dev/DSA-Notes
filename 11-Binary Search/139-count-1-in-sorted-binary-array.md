<!-- #region 139-Count 1 in sorted binary array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q139: Count 1 in sorted binary array</h1>

## 1. Problem Understanding

- You are given a binary, sorted in non-increasing order array:
  * 1 1 1 ... 1 0 0 0
- You must find the count of 1s.
- Because all 1s come first and all 0s later, the problem becomes finding the boundary between 1 → 0.
- Binary search helps find this boundary in O(log n).
---

## 2. Constraints

- 1 <= N <= 10^6
- Array contains only 0 and 1
- Array is sorted in non-increasing order
- Must be efficient (binary search recommended)
---

## 3. Edge Cases

- All 1s → count = N
- All 0s → count = 0
- Only one element
- Transition happens at index 0
- Transition happens at index N-1
---

## 4. Examples

```text
Example 1

Input

8
1 1 1 1 1 0 0 0


Output

5

Example 2

Input

4
1 1 1 1


Output

4
```

---

## 5. Approaches

### Approach 1: Linear Scan (Brute Force)

**Idea:**
- Traverse from the left and count until you see the first 0.

**Steps:**
- Initialize count = 0
- Loop from left to right
- If element is 1, increment
- If element is 0, break immediately
- Return count

**Java Code:**
```java
public int countOnes(int[] arr) {
    int count = 0;
    for (int x : arr) {
        if (x == 1) count++;
        else break;
    }
    return count;
}
```

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(k) where k = number of initial 1s
  * Worst case → O(n)
  * Why?
    * You may traverse the entire array if all values are 1.
- 💾 Space Complexity
  * O(1)
  * Why?
    * No extra memory used.

### Approach 2: Binary Search for First 0 (Better)

**Idea:**
- Count of 1s = index of the first 0.
- Example:
- [1 1 1 0 0] → first 0 at index 3 → count = 3.

**Steps:**
- Binary search to find the first index where arr[mid] == 0.
- If no 0 exists → return N.
- Else return that index.

**Java Code:**
```java
public int countOnes(int[] arr) {
    int low = 0, high = arr.length - 1;
    int firstZero = arr.length;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == 0) {
            firstZero = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }

    return firstZero;
}
```

**💭 Intuition Behind the Approach:**
- The first 0 divides the array into:
- [1s] [0s]
- Finding that boundary gives us the count of 1s.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Because each step halves the search space.
- 💾 Space Complexity
  * O(1)
  * Only a few variables used.

### Approach 3: Binary Search for Last 1 (Optimal)

**Idea:**
- Find last index where arr[mid] == 1, then count = mid + 1

**Steps:**
- Steps
- Binary search over array.
- If arr[mid] == 1, record index and move right to find last 1.
- If arr[mid] == 0, move left.
- At end →
  * if last1 = -1, return 0
  * else return last1 + 1

**Java Code:**
```java
public int countOnes(int[] arr) {
    int low = 0, high = arr.length - 1;
    int lastOne = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == 1) {
            lastOne = mid;
            low = mid + 1; // search right
        } else {
            high = mid - 1;
        }
    }

    return lastOne + 1;
}
```

**💭 Intuition Behind the Approach:**
- The array looks like this:
  * 1 1 1 … 1 | 0 0 0
- Binary search targets the rightmost 1 efficiently.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(log n)
  * Boundary search using binary search.
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Binary search works perfectly because the array has a monotonic pattern: all 1s first, then all 0s.
- Finding the transition boundary is the fastest way to determine count.
---

## 7. Variants / Follow-Ups

- Find first 1
- Find first 0 in increasing binary array
- Count zeros
- First and last occurrence of any target
- Works in monotonic boolean functions
- Find first element greater than X
---

## 8. Tips & Observations

- Always look for monotonicity to apply binary search.
- For non-increasing binary arrays:
  * First 0 → count of 1s
  * Last 1 → count of 1s
- Binary search boundary problems always need careful condition handling.
---

<!-- #endregion -->
