<!-- #region 156-Kth element of 2 sorted arrays -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q156: Kth element of 2 sorted arrays</h1>

## 1. Problem Understanding

- We have two sorted arrays a and b with sizes m and n.
- We need to find the k-th smallest element of the merged sorted array without merging.
---

## 2. Constraints

- 1 ≤ m, n ≤ 10^4
- Values up to 10^9
- Arrays already sorted
- 1 ≤ k ≤ m + n
---

## 3. Edge Cases

- k = 1 (smallest of both first elements)
- k = m + n (largest of both last elements)
- One array completely smaller than the other
- One array empty
- Arrays with duplicates
- Very uneven lengths
---

## 4. Examples

```text
Input:
a = [2,3,6,7,9], b = [1,4,8,10], k = 5
Output: 6

Input:
a = [100,112,256,349,770], b = [72,86,113,119,265,445,892], k = 7
Output: 256
```

---

## 5. Approaches

### Approach 1: Brute Force Merge (Like MergeSort)

**Idea:**
- Merge both arrays and return kth element.

**Steps:**
- Use two pointers.
- Build merged list up to k-th position.
- Return k-th element.

**Java Code:**
```java
public static int kthBrute(int[] a, int[] b, int k) {
    int i=0, j=0;
    int count=0;

    while(i<a.length && j<b.length){
        int val;
        if(a[i] < b[j]) val = a[i++];
        else val = b[j++];
        count++;
        if(count == k) return val;
    }

    while(i < a.length){
        count++;
        if(count == k) return a[i];
        i++;
    }

    while(j < b.length){
        count++;
        if(count == k) return b[j];
        j++;
    }

    return -1;
}
```

**💭 Intuition Behind the Approach:**
- Straight merging until we reach the k-th element.

**Complexity (Time & Space):**
- Time: O(k)
  * We scan up to k elements.
- Space: O(1)
  * No extra array.

### Approach 2: Binary Search on K (Partition Method)

**Idea:**
- Same as median partition logic but now we want exactly k elements on the left side.
- Binary search on cut point in array a.
- Let:
  * cut1 + cut2 = k
- Then we check cross-boundary conditions:
  * left1 <= right2
  * left2 <= right1

**Java Code:**
```java
public static int kthOptimal(int[] a, int[] b, int k){
    int n = a.length, m = b.length;

    // ensure a is smaller
    if(n > m) return kthOptimal(b, a, k);

    int low = Math.max(0, k - m);
    int high = Math.min(k, n);

    while(low <= high){
        int cut1 = low + (high - low)/2;
        int cut2 = k - cut1;

        int l1 = (cut1 == 0) ? Integer.MIN_VALUE : a[cut1 - 1];
        int l2 = (cut2 == 0) ? Integer.MIN_VALUE : b[cut2 - 1];

        int r1 = (cut1 == n) ? Integer.MAX_VALUE : a[cut1];
        int r2 = (cut2 == m) ? Integer.MAX_VALUE : b[cut2];

        if(l1 <= r2 && l2 <= r1){
            return Math.max(l1, l2);
        }
        else if(l1 > r2){
            high = cut1 - 1;
        } 
        else {
            low = cut1 + 1;
        }
    }
    return -1;
}
```

**💭 Intuition Behind the Approach:**
- We find a partition where exactly k elements are on the left.
- The maximum on the left is the k-th smallest.
- This avoids merging and uses binary search on cut index.

**Complexity (Time & Space):**
- Time: O(log(min(m,n)))
  * Binary search on smaller array.
- Space: O(1)

### Approach 3: K-th Element Using Recursion (Divide K by 2)

**Idea:**
- Compare a[k/2] and b[k/2].
- Eliminate k/2 elements from one array.
- Recurse with reduced k.

**Java Code:**
```java
public static int kthRecursive(int[] a, int i, int[] b, int j, int k){
    if(i >= a.length) return b[j + k - 1];
    if(j >= b.length) return a[i + k - 1];
    if(k == 1) return Math.min(a[i], b[j]);

    int midA = (i + k/2 - 1 < a.length) ? a[i + k/2 - 1] : Integer.MAX_VALUE;
    int midB = (j + k/2 - 1 < b.length) ? b[j + k/2 - 1] : Integer.MAX_VALUE;

    if(midA < midB){
        return kthRecursive(a, i + k/2, b, j, k - k/2);
    } else {
        return kthRecursive(a, i, b, j + k/2, k - k/2);
    }
}

Recursion Tree Example (k = 7)
k=7
 |
 |-- compare a[3], b[3]
 |
 k = 7 - 3 = 4
 |
 |-- compare a[?], b[?]
 |
 k = 4 - 2 = 2
 |
 k = 1 or 2
 return answer
```

**💭 Intuition Behind the Approach:**
- You eliminate half of k on each call → like binary search on rank.

**Complexity (Time & Space):**
- Time: O(log(k))
- Space: O(log(k)) recursion stack

---

## 6. Justification / Proof of Optimality

- Arrays are sorted → k-th element depends only on ordering around index k.
- Binary-search partition uses monotonic behavior of partitions.
- Recursive elimination removes half the candidates each step → optimal.
---

## 7. Variants / Follow-Ups

- Find median of two sorted arrays
- Find k-th smallest in multiple sorted arrays
- Find k-th element in infinite sorted streams
- Find k-th largest in sorted arrays (just adjust indexing)
---

## 8. Tips & Observations

- Always binary search on the smaller array.
- Use ±∞ when cut goes out of bounds.
- For k very small (1,2) handle separately to avoid mistakes.
- Partition logic is same as median problem.

- **⚠️ Pitfalls**
    - Wrong bounds for binary search (low = max(0, k-m) and high = min(k,n) are mandatory)
    - Missing base cases for recursion
    - Off-by-one in returning a[i + k - 1] or b[j + k - 1]
    - Forgetting to ensure smaller array in binary search
---

<!-- #endregion -->
