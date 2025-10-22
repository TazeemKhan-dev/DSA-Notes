<!-- #region 0-Recursion on Arraylist -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q0: Recursion on Arraylist</h1>

## 1. Problem Understanding

- Recursion on ArrayList involves processing elements by index or removing elements recursively.
- Useful for:
- Traversal
- Sum / Product
- Searching
- Maximum / Minimum
- Backtracking (subsets / permutations)
---

## 2. Constraints

- ArrayList size = n
- Base case: index i == list.size()
- Avoid concurrent modification when modifying list during recursion
- Stack depth = O(n)
---

## 3. Edge Cases

- Empty list
- Single element list
- Duplicate elements
- Modifications during recursion (add/remove) must be carefully handled
---

## 4. Examples

```text
Input: [1, 2, 3] → Output: 1 2 3

Input: [1, 2, 3] → Sum = 6

Input: [1, 2] → Subsets = [], [1], [2], [1,2]
```

---

## 5. Approaches

### Approach 1: Traversal (Forward / Backward)

**Java Code:**
```java
Forward Traversal

void traverse(ArrayList<Integer> list, int i) {
    if (i == list.size()) return;
    System.out.print(list.get(i) + " ");
    traverse(list, i + 1);
}


Backward Traversal

void traverseReverse(ArrayList<Integer> list, int i) {
    if (i == list.size()) return;
    traverseReverse(list, i + 1);
    System.out.print(list.get(i) + " ");
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 2: Sum / Product

**Java Code:**
```java
Sum

int sumList(ArrayList<Integer> list, int i) {
    if (i == list.size()) return 0;
    return list.get(i) + sumList(list, i + 1);
}


Product

int productList(ArrayList<Integer> list, int i) {
    if (i == list.size()) return 1;
    return list.get(i) * productList(list, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 3: Search Element

**Java Code:**
```java
Linear Search

boolean searchList(ArrayList<Integer> list, int i, int key) {
    if (i == list.size()) return false;
    if (list.get(i) == key) return true;
    return searchList(list, i + 1, key);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 4: Find Maximum / Minimum

**Java Code:**
```java
Maximum

int maxList(ArrayList<Integer> list, int i) {
    if (i == list.size() - 1) return list.get(i);
    return Math.max(list.get(i), maxList(list, i + 1));
}


Minimum

int minList(ArrayList<Integer> list, int i) {
    if (i == list.size() - 1) return list.get(i);
    return Math.min(list.get(i), minList(list, i + 1));
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 5: Remove Element Recursively

**Idea:**
- Skip or remove elements while traversing.

**Java Code:**
```java
void removeElement(ArrayList<Integer> list, int i, int key) {
    if (i == list.size()) return;
    if (list.get(i) == key) {
        list.remove(i);
        removeElement(list, i, key); // do not increment index
    } else {
        removeElement(list, i + 1, key);
    }
}
```

**Complexity (Time & Space):**
- Time: O(n²) in worst case (due to remove shift)
- Space: O(n)

### Approach 6: Subsets (Backtracking)

**Idea:**
- Include/exclude elements recursively.

**Java Code:**
```java
void subsets(ArrayList<Integer> list, ArrayList<Integer> curr, int i) {
    if (i == list.size()) {
        System.out.println(curr);
        return;
    }
    curr.add(list.get(i));
    subsets(list, curr, i + 1);   // include
    curr.remove(curr.size() - 1);
    subsets(list, curr, i + 1);   // exclude
}
```

**Complexity (Time & Space):**
- Time: O(2ⁿ)
- Space: O(n)

### Approach 7: Permutations (Backtracking)

**Idea:**
- Swap elements to generate all permutations.

**Java Code:**
```java
void permute(ArrayList<Integer> list, int i) {
    if (i == list.size()) {
        System.out.println(list);
        return;
    }
    for (int j = i; j < list.size(); j++) {
        Collections.swap(list, i, j);
        permute(list, i + 1);
        Collections.swap(list, i, j); // backtrack
    }
}
```

**Complexity (Time & Space):**
- Time: O(n × n!)
- Space: O(n)

### Approach 8: Count / Sum with Condition

**Idea:**
- Example: Sum of even elements

**Java Code:**
```java
int sumEven(ArrayList<Integer> list, int i) {
    if (i == list.size()) return 0;
    int sum = (list.get(i) % 2 == 0) ? list.get(i) : 0;
    return sum + sumEven(list, i + 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 9: Reverse ArrayList Recursively

**Java Code:**
```java
void reverseList(ArrayList<Integer> list, int l, int r) {
    if (l >= r) return;
    Collections.swap(list, l, r);
    reverseList(list, l + 1, r - 1);
}
```

**Complexity (Time & Space):**
- Time: O(n)
- Space: O(n)

### Approach 10: Remove Duplicates Recursively

**Java Code:**
```java
void removeDuplicates(ArrayList<Integer> list, int i) {
    if (i >= list.size() - 1) return;
    if (list.get(i).equals(list.get(i + 1))) {
        list.remove(i + 1);
        removeDuplicates(list, i);
    } else {
        removeDuplicates(list, i + 1);
    }
}
```

**Complexity (Time & Space):**
- Time: O(n²) worst case
- Space: O(n)

---

## 6. Justification / Proof of Optimality

- ArrayList recursion is almost identical to array recursion.
- Key difference: methods like .get(), .add(), .remove() introduce extra cost.
- Useful for backtracking and combinatorial problems.
---

## 7. Variants / Follow-Ups

- Recursion on ArrayList of Strings
- 2D ArrayList recursion (list of lists)
- Nested backtracking (subsets of subsets)
---

## 8. Tips & Observations

- Always use index-based recursion to avoid ConcurrentModificationException.
- Recursive depth = size of list = O(n)
- Avoid modifying list during iteration without careful indexing.
- Backtracking requires restore state after recursion (remove / swap).
---

<!-- #endregion -->
