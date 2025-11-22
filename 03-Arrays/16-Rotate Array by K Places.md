<!-- #region 16-Rotate Array by K Places -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q16: Rotate Array by K Places</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** For left rotation: [1,2,3,4,5], k=2 → [3,4,5,1,2]  For right rotation: [1,2,3,4,5], k=2 → [4,5,1,2,3]

---

## 2. Input, Output, & Constraints

**Constraints:**
- 1 <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4
- 0 <= k <= 10^5


---

## 3. Examples & Edge Cases

**Example 1 (Left Rotate:):**
Input:
```text
[1,2,3,4,5], k=4
```
Output:
```text
[5,1,2,3,4]
```

**Example 2 (Right Rotate:):**
Input:
```text
[1,2,3,4,5], k=4
```
Output:
```text
[2,3,4,5,1]
```


---

## 4. Approaches

### Approach 1: Using Extra Array (Simple)

- **Idea:**
  - For left rotate: Create a new array, place nums[i] at (i - k + n) % n
  - For right rotate: Place nums[i] at (i + k) % n

**Java Code:**
```java
// Left Rotate
public static void leftRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    int[] res = new int[n];
    for (int i = 0; i < n; i++) {
        res[(i - k + n) % n] = nums[i];
    }
    for (int i = 0; i < n; i++) nums[i] = res[i];
}

// Right Rotate
public static void rightRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    int[] res = new int[n];
    for (int i = 0; i < n; i++) {
        res[(i + k) % n] = nums[i];
    }
    for (int i = 0; i < n; i++) nums[i] = res[i];
}
```

**Complexity:**
- Time: O(n)
- Space: O(n) → extra array

### Approach 2: Using Reverse (Optimal, In-Place)

- **Idea:**
  - Reverse parts of the array to rotate in-place.
  - Left Rotate by k:
  - Reverse first k elements
  - Reverse remaining n-k elements
  - Reverse the whole array
  - Right Rotate by k:
  - Reverse last k elements
  - Reverse first n-k elements
  - Reverse the whole array

**Java Code:**
```java
// Helper to reverse a subarray
private static void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}

// Left Rotate
public static void leftRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    reverse(nums, 0, k - 1);
    reverse(nums, k, n - 1);
    reverse(nums, 0, n - 1);
}

// Right Rotate
public static void rightRotate(int[] nums, int k) {
    int n = nums.length;
    k = k % n;
    reverse(nums, n - k, n - 1);
    reverse(nums, 0, n - k - 1);
    reverse(nums, 0, n - 1);
}
```

**Complexity:**
- Time: O(n)
- Space: O(1) → in-place


---

## 5. Justification / Proof of Optimality

- Extra array method is intuitive but uses O(n) extra space.
- Reverse method is optimal, in-place, and widely used in interviews.
- Correctly handles k >= n using modulo.

---

## 6. Variants / Follow-Ups

- Rotate array by one position repeatedly
- Rotate multi-dimensional arrays
- Rotate linked list by k positions
- Rotate circular arrays or arrays with constraints

<!-- #endregion -->

