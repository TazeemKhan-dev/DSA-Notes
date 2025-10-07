<!-- #region 49-Majority Element (I, II & n/k — Combined) -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q49: Majority Element (I, II & n/k — Combined)</h1>
<br>

## 1. Understand the Problem

- We have three related variations of the Majority Element problem:
- Variant I (Majority Element I)
- Input: Integer array nums of size n.
- Task: Return the element that appears more than n/2 times.
- Guarantee: The majority element always exists.
- Variant II (Majority Element II)
- Input: Integer array nums of size n.
- Task: Return all elements that appear more than n/3 times.
- Output can be in any order; for this note, we sort ascending.
- Variant III (Generalized n/k Majority)
- Input: Integer array nums of size n and integer k ≥ 2.
- Task: Return all elements that appear more than n/k times.
- Output sorted ascending.

---

## 2. Constraints

- n == nums.length
- 1 ≤ n ≤ 10^5
- 2 ≤ k ≤ n (for n/k variant)
- -10^4 ≤ nums[i] ≤ 10^4
- Variant I: One element appears more than n/2 times
- Variant II: Elements appear more than n/3 times

---

## 3. Examples & Edge Cases

**Example 1 (Variant I):**
Input:
```text
[7,0,0,1,7,7,2,7,7]
```
Output:
```text
7
7 appears 5 times > 9/2
```

**Example 2 (Variant II):**
Input:
```text
[1,2,1,1,3,2,2]
```
Output:
```text
[1,2]
n/3 = 2, 1 and 2 appear ≥3 times
```

**Example 3 (Variant III (n/k)):**
Input:
```text
4
```
Output:
```text
[1,2]
n/4 = 2, 1 and 2 appear >2 times
```


---

## 4. Approaches

### Approach 1: Brute Force

- **Idea:**
  - Idea:
  - Count frequency of every element and check which elements satisfy n/2, n/3, or n/k conditions.
  - Steps:
  - Traverse array.
  - Maintain frequency map for each element.
  - Check count > n/2 (Variant I), > n/3 (Variant II), > n/k (Variant III).

**Java Code:**
```java
import java.util.*;

class MajorityBrute {
    // Variant I
    static int majorityElementI(int[] nums) {
        Map<Integer, Integer> freq = new HashMap<>();
        int n = nums.length;
        for (int num : nums) freq.put(num, freq.getOrDefault(num, 0) + 1);
        for (int num : freq.keySet())
            if (freq.get(num) > n/2) return num;
        return -1;
    }

    // Variant II
    static List<Integer> majorityElementII(int[] nums) {
        Map<Integer, Integer> freq = new HashMap<>();
        int n = nums.length;
        for (int num : nums) freq.put(num, freq.getOrDefault(num, 0) + 1);
        List<Integer> res = new ArrayList<>();
        for (int num : freq.keySet())
            if (freq.get(num) > n/3) res.add(num);
        Collections.sort(res);
        return res;
    }

    // Variant III - General n/k
    static List<Integer> majorityElementNK(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();
        int n = nums.length;
        for (int num : nums) freq.put(num, freq.getOrDefault(num, 0) + 1);
        List<Integer> res = new ArrayList<>();
        for (int num : freq.keySet())
            if (freq.get(num) > n/k) res.add(num);
        Collections.sort(res);
        return res;
    }
}
```

**Complexity:**
- Time: O(n) same for all
- Space: O(n) same for all

### Approach 2: Better / Sorting

- **Idea:**
  - Idea:
  - Sort array → majority elements will be at predictable positions.
  - Steps:
  - Sort array.
  - Variant I: element at index n/2 is majority.
  - Variant II: check elements at intervals for count > n/3.
  - Variant III: check elements at intervals for count > n/k.

**Java Code:**
```java
import java.util.*;

class MajorityBetter {
    // Variant I
    static int majorityElementI(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
    }

    // Variant II
    static List<Integer> majorityElementII(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int count = 1;
            while (i+1 < n && nums[i] == nums[i+1]) { i++; count++; }
            if (count > n/3) res.add(nums[i]);
        }
        Collections.sort(res);
        return res;
    }

    // Variant III
    static List<Integer> majorityElementNK(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int count = 1;
            while (i+1 < n && nums[i] == nums[i+1]) { i++; count++; }
            if (count > n/k) res.add(nums[i]);
        }
        Collections.sort(res);
        return res;
    }
}
```

**Complexity:**
- Time: O(n log n) same for all
- Space: O(1) extra space same for all

### Approach 3: Optimal / Most Efficient (Boyer-Moore Voting)

- **Idea:**
  - Idea:
  - Variant I: Single majority (> n/2) → standard Boyer-Moore Voting Algorithm.
  - Variant II: Maximum 2 elements > n/3 → Extended Boyer-Moore.
  - Variant III: Maximum k-1 elements > n/k → Generalized Boyer-Moore.
  - Boyer-Moore Voting Algorithm Explanation:
  - Principle: a majority element will survive all pair cancellations.
  - Maintain candidate(s) and count(s).
  - Traverse array:
  - If element matches candidate → increment count
  - Else if empty slot → assign candidate with count = 1
  - Else → decrement all counts by 1 (pair cancellation)
  - At end of first pass → potential candidates.
  - Second pass → validate counts > n/2, n/3, or n/k.

**Java Code:**
```java
import java.util.*;

class MajorityOptimal {

    // Variant I - Single majority (> n/2)
    static int majorityElementI(int[] nums) {
        int count = 0, candidate = 0;
        for (int num : nums) {
            if (count == 0) candidate = num;
            count += (num == candidate) ? 1 : -1;
        }
        return candidate;
    }

    // Variant II - Multiple majority (> n/3)
    static List<Integer> majorityElementII(int[] nums) {
        int n = nums.length;
        int count1=0, count2=0, candidate1=0, candidate2=1;
        for(int num : nums){
            if(num == candidate1) count1++;
            else if(num == candidate2) count2++;
            else if(count1==0){ candidate1=num; count1=1; }
            else if(count2==0){ candidate2=num; count2=1; }
            else { count1--; count2--; }
        }
        count1=0; count2=0;
        for(int num : nums){
            if(num==candidate1) count1++;
            else if(num==candidate2) count2++;
        }
        List<Integer> res = new ArrayList<>();
        if(count1 > n/3) res.add(candidate1);
        if(count2 > n/3) res.add(candidate2);
        Collections.sort(res);
        return res;
    }

    // Variant III - General n/k
    static List<Integer> majorityElementNK(int[] nums, int k) {
        int n = nums.length;
        if(k<2) return new ArrayList<>();
        Map<Integer,Integer> candidates = new HashMap<>();
        for(int num: nums){
            if(candidates.containsKey(num))
                candidates.put(num,candidates.get(num)+1);
            else if(candidates.size() < k-1)
                candidates.put(num,1);
            else{
                List<Integer> keys = new ArrayList<>(candidates.keySet());
                for(int key: keys){
                    candidates.put(key,candidates.get(key)-1);
                    if(candidates.get(key)==0) candidates.remove(key);
                }
            }
        }
        Map<Integer,Integer> actualCounts = new HashMap<>();
        for(int num: nums){
            if(candidates.containsKey(num))
                actualCounts.put(num, actualCounts.getOrDefault(num,0)+1);
        }
        List<Integer> res = new ArrayList<>();
        for(int key: actualCounts.keySet())
            if(actualCounts.get(key) > n/k) res.add(key);
        Collections.sort(res);
        return res;
    }
}
```

**Complexity:**
- Time: Variant I: O(n) ,Variant II: O(n), Variant III: O(n*k)
- Space: Variant I: O(1) space  , Variant II: O(1) space  , Variant III:  O(k) space


---

## 5. Justification / Proof of Optimality

- Brute: Simple, works for all k, but uses extra space.
- Better: Sorting is simple, moderate space/time, works for all variants.
- Optimal: Boyer-Moore is fastest and uses minimal space; generalized version handles arbitrary k efficiently.

---

## 6. Variants / Follow-Ups

- Maximum frequency element
- Count occurrences in streaming data
- Apply for large datasets using O(1) space

---

## 7. Tips & Observations

- Boyer-Moore principle: majority elements survive cancellations.
- Variant I always has exactly 1 candidate.
- Variant II: at most 2 candidates.
- Variant III: at most k-1 candidates.
- Sorting is easier but O(n log n).
- HashMap universal but O(n) extra space.
- Always validate candidates for n/3 or n/k cases.

<!-- #endregion -->

