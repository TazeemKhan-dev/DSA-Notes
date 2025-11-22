<!-- #region 38-Union of Two Sorted Arrays -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q38: Union of Two Sorted Arrays</h1>
<br>

## 1. Understand the Problem
- **Paraphrase:** Combine two sorted arrays into one sorted array containing all distinct elements.

---

## 2. Constraints

- 1 <= n, m <= 10^5
- -10^6 <= arr1[i], arr2[i] <= 10^6
- Arrays are sorted.


---

## 3. Examples & Edge Cases

**Example 1 (Edge Case):**
Input:
```text
arr1 = [], arr2 = [1, 2, 3]
```
Output:
```text
0[1, 2, 3]
```

**Example 2 (Normal Case):**
Input:
```text
arr1 = [1, 2, 3], arr2 = [2, 3, 4]
```
Output:
```text
[1, 2, 3, 4]
```


---

## 4. Approaches

### Approach 1: Brute Force (Merge & Set)

- **Idea:**
  - Merge both arrays into one array.
  - Use a HashSet to remove duplicates.
  - Convert HashSet to sorted array.

**Java Code:**
```java
public static int[] unionBrute(int[] arr1, int[] arr2) {
    Set<Integer> set = new HashSet<>();
    for(int num : arr1) set.add(num);
    for(int num : arr2) set.add(num);
    int[] result = set.stream().sorted().mapToInt(i -> i).toArray();
    return result;
}
```

**Complexity:**
- Time: O((n + m) log(n + m)) → sorting after merge.
- Space: O(n + m) → for merged array and HashSet.

### Approach 2: Optimal (Two Pointer Technique)

- **Idea:**
  - Use two pointers i and j for arr1 and arr2.
  - Compare elements:
  - If arr1[i] < arr2[j] → add arr1[i] and move i.
  - If arr1[i] > arr2[j] → add arr2[j] and move j.
  - If equal → add one element and move both pointers.
  - Skip duplicates by checking the last element added.
  - Add remaining elements from both arrays.

**Java Code:**
```java
public static List<Integer> unionOptimal(int[] arr1, int[] arr2) {
    List<Integer> union = new ArrayList<>();
    int i = 0, j = 0;
    while(i < arr1.length && j < arr2.length){
        int val;
        if(arr1[i] < arr2[j]){
            val = arr1[i++];
        } else if(arr1[i] > arr2[j]){
            val = arr2[j++];
        } else {
            val = arr1[i];
            i++; j++;
        }
        if(union.isEmpty() || union.get(union.size() - 1) != val){
            union.add(val);
        }
    }
    while(i < arr1.length){
        if(union.get(union.size() - 1) != arr1[i])
            union.add(arr1[i]);
        i++;
    }
    while(j < arr2.length){
        if(union.get(union.size() - 1) != arr2[j])
            union.add(arr2[j]);
        j++;
    }
    return union;
}
```

**Complexity:**
- Time: O(n + m) → single pass through both arrays.
- Space: O(n + m) → for result array.


---

## 5. Justification / Proof of Optimality

- Optimal approach avoids sorting after merge → faster O(n+m)
- Maintains sorted order naturally.
- Handles duplicates efficiently.

---

## 6. Variants / Follow-Ups

- Intersection of two sorted arrays → instead of union.
- k-sorted arrays union → merge k arrays using priority queue.
- Unsorted arrays → sort first or use HashSet.
- Find union without extra space if arrays are mutable.

<!-- #endregion -->

