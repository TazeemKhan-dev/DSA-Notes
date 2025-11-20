<!-- #region 130-Maximum width jump -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q130: Maximum width jump</h1>

## 1. Problem Understanding

- You are given an array nums.
- A ramp is a pair (i, j) such that:
  * i < j
  * nums[i] <= nums[j]
  * The width is:
    * width = j - i
- Your task is to find the maximum width among all valid ramps.
- If no ramp exists, return 0.
---

## 2. Constraints

- 2 <= n <= 50000
- Values up to 5 * 10^4
- Must use O(n) or O(n log n) approach.
---

## 3. Edge Cases

- Strictly decreasing array → no ramp → return 0
- Multiple valid ramps; pick max width
- Duplicate values help (since <= condition)
---

## 4. Examples

```text
Example 1
nums = [6, 0, 8, 2, 1, 5]
max ramp = (1, 5) because nums[1]=0 <= nums[5]=5
width = 5 - 1 = 4
Output 4
Example 2
nums = [9, 8, 1, 0, 1, 9, 4, 0, 4, 1]
max ramp = (2, 9) => 1 <= 1
width = 7
Output 7
```

---

## 5. Approaches

### Approach 1: Monotonic Decreasing Stack + Backward Scan (Optimal O(n))

**Idea:**
- Build a stack of indices where values are strictly decreasing.
- These are the best possible left-side candidates.
- Traverse from right (j = n-1 to 0):
  * While the top of the stack forms a valid ramp:
    * compute width
    * pop (we already found the best j for that index)

**Java Code:**
```java
public static int maxWidthRamp(int[] nums) {
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < nums.length; i++) {
        if (st.isEmpty() || nums[i] < nums[st.peek()]) {
            st.push(i);
        }
    }

    int ans = 0;

    for (int j = nums.length - 1; j >= 0; j--) {
        while (!st.isEmpty() && nums[st.peek()] <= nums[j]) {
            ans = Math.max(ans, j - st.peek());
            st.pop();
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- A left index with a bigger value than previous ones is useless → skip
- Stack keeps only good smallest-left candidates
- Scanning from right ensures maximum width

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n) because:
    * each index is pushed once
    * each popped at most once
- 💾 Space Complexity
  * O(n)

### Approach 2: Sorting + Two Pointers

**Idea:**
- Create pairs (value, index)
- Sort by value (if tie → index)
- Now all value[i] <= value[j] is guaranteed in sorted order.
- Use two pointers to maintain:
  * minLeftIndex
  * currentRightIndex

**Steps:**
- Create arr = [(nums[i], i)] pairs
- Sort arr by value increasing
- Maintain:
  * minIndex = +∞
  * For each pair (value, index):
    * answer = max(answer, index - minIndex)
    * update minIndex
- This is true two-pointers, because:
  * sorted array → left ≤ right automatically
  * only track minimal index so far as left pointer
  * current index acts as right pointer

**Java Code:**
```java
public static int maxWidthRamp(int[] nums) {

    int n = nums.length;

    // arr[i] = {nums[i], i} → store (value, originalIndex)
    int[][] arr = new int[n][2];

    // Fill the pair array
    for (int i = 0; i < n; i++) {
        arr[i][0] = nums[i];  // value
        arr[i][1] = i;        // index
    }

    // Sort by value (ascending).
    // After sorting, for any two pairs a and b:
    // a[0] <= b[0] ensures nums[left] <= nums[right]
    Arrays.sort(arr, (a, b) -> a[0] - b[0]);

    int minIndex = Integer.MAX_VALUE;  // track smallest index seen so far
    int ans = 0;                       // maximum width ramp

    // Iterate over sorted pairs (value, index)
    for (int[] p : arr) {

        int idx = p[1];  // original index of current element

        // If current index is smaller → becomes new left boundary
        if (idx < minIndex) {
            minIndex = idx;
        } 
        
        // idx > minIndex → we found a valid ramp: minIndex < idx AND nums[minIndex] <= nums[idx]
        else {
            // width = rightIndex - leftIndex
            ans = Math.max(ans, idx - minIndex);
        }
    }

    return ans;
}

```

**💭 Intuition Behind the Approach:**
- Sorting by value ensures nums[left] <= nums[right]
- Only check whether the index order also matches (leftIndex < rightIndex)
- Maintain the smallest left index so far (this becomes the left pointer)
- Current element acts as right pointer
- This becomes a clean, stable 2-pointer system on sorted value-index pairs.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n log n) due to sorting
- 💾 Space Complexity
  * O(n)

### Approach 3: Brute Force (O(n²), not recommended)

**Idea:**
- Try every (i, j) pair from right to left.

**Java Code:**
```java
public static int maxWidthRamp(int[] nums) {
    int ans = 0;
    int n = nums.length;

    for (int i = 0; i < n; i++) {
        for (int j = n - 1; j > i; j--) {
            if (nums[i] <= nums[j]) {
                ans = Math.max(ans, j - i);
                break;
            }
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- Simple but too slow.

**Complexity (Time & Space):**
- ⏱️ Time Complexity
  * O(n^2)
- 💾 Space Complexity
  * O(1)

---

## 6. Justification / Proof of Optimality

- Monotonic stack gives best performance: O(n)
- Sorting + two pointers is clean and intuitive: O(n log n)
- Brute force is useful only for understanding.
---

## 7. Variants / Follow-Ups

- Count number of valid ramps
- Return the pair itself
- Reverse condition: find (i, j) such that nums[i] >= nums[j]
- 2D variant in matrices
---

## 8. Tips & Observations

- Monotonic structures drastically reduce search space
- Sorting transforms ramp condition into simple index comparison
- Always check decreasing patterns — they usually suggest stacks
---

<!-- #endregion -->
