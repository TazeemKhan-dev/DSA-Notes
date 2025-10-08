<!-- #region 49-Majority Element I, II, n/k -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q49: Majority Element I, II, n/k</h1>

## 1. Problem Understanding

- Majority Element I: Return the element that appears more than n/2 times in the array. Guaranteed to exist.
- Majority Element II: Return all elements appearing more than n/3 times. Output can be in any order.
- Majority Element n/k: Generalized problem: return all elements appearing more than n/k times.
- Key differences:
- I → Single element, threshold n/2
- II → Multiple elements, threshold n/3
- n/k → Multiple elements, threshold n/k
---

## 2. Algorithm

### Algorithm 1: Boyer–Moore Voting Algorithm

- The Boyer–Moore Voting Algorithm is a mathematical elimination approach used to find elements that appear more frequently than a certain threshold (like n/2, n/3, or n/k).
- It relies on the principle of pairing and canceling out occurrences of different elements.
- If one element appears more than n/x times, then there can be at most x - 1 such elements.


- **Intuition**
    - Think of each number as a “vote.”
    - When two different numbers appear, they cancel each other’s votes.
    - The number(s) that remain after all cancellations are potential majority elements.
    - Finally, a verification step ensures these candidates truly exceed the threshold.

- **How It Works**
    - Maintain counters and candidates (up to k - 1 of them if searching for elements > n/k).
    - Iterate through the array:
    - If the current number matches one of the candidates → increment its count.
    - Else if there’s an empty slot (count = 0) → assign this number as a new candidate.
    - Else → decrement all existing counters (as this element "cancels out" votes).
    - After one pass, possible majority elements remain.
    - Verify actual frequencies to confirm valid majorities.

- **Example (Conceptual)**
    - Let’s say the array is [a, b, a, c, a, b, a].
    - Every time a meets b or c, one of its votes gets canceled.
    - However, since a appears more than half of the time, it cannot be completely eliminated.
    - The final surviving candidate will be a.

- **Key Observations**
    - For n/2 majority, we track 1 candidate.
    - For n/3 majority, we track 2 candidates.
    - For n/k majority, we track (k - 1) candidates.
    - The algorithm generalizes cleanly with the same elimination principle.

- **Complexity**
    - Time Complexity: O(n) — single pass for selection + optional verification.
    - Space Complexity: O(1) — constant space for fixed k.

- **Why It’s Efficient**
    - Traditional counting methods (like HashMaps) use O(n) space.
    - Boyer–Moore reduces this to O(1) by keeping only a limited number of potential candidates.
    - It leverages mathematical guarantees about frequency limits to ensure correctness.

- **In Summary**
    - Purpose: Find majority elements (> n/x occurrences)
    - Concept: Pairing and canceling out elements
    - Guarantee: At most (x - 1) candidates
    - Phases:
        * Voting (finding potential candidates)
        * Verification (confirming actual frequencies)
    - Advantages: Linear time, constant space, intuitive logic
---

## 3. Constraints

- n == nums.length
- 1 ≤ n ≤ 10^5
- 2 ≤ n for II and n/k
- -10^4 ≤ nums[i] ≤ 10^4
- Threshold: n/2, n/3, or n/k depending on variant
- Output: sorted ascending if required (II and n/k)
---

## 4. Edge Cases

- All elements are the same → single majority
- No element meets the threshold → not possible for I, possible for n/k (return empty)
- Array with negative numbers
- Array of size 1
- Elements appear exactly at the threshold boundary
- Multiple elements qualify in n/k or II
---

## 5. Examples

```text
Example 1 (Majority Element I):
Input: nums = [7, 0, 0, 1, 7, 7, 2, 7, 7] → Output: 7
Explanation: 7 appears 5 times in size 9 array → majority

Example 2 (Majority Element II):
Input: nums = [1, 2, 1, 1, 3, 2, 2] → Output: [1, 2]
Explanation: n/3 = 7/3 = 2, elements appearing ≥3 times: [1, 2]

Example 3 (Majority Element n/k, k=4):
Input: nums = [1, 2, 2, 3, 2, 1, 1, 4], k = 4 → Output: [1, 2]
Explanation: n/k = 8/4 = 2, elements appearing >2 times: [1, 2]
```

---

## 6. Approaches

### Approach 1: Brute Force

**Idea:**
- Count frequency of each element and check against threshold.

**Steps:**
- Use a hash map to count occurrences.
- Check elements appearing more than n/2, n/3, or n/k times.
- Return results.

**Java Code:**
```java
class MajorityElementBrute {
    public int majorityElement(int[] nums) { // n/2
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);
        int n = nums.length;
        for (int key : count.keySet()) if (count.get(key) > n/2) return key;
        return -1; // never occurs
    }

    public List<Integer> majorityElementII(int[] nums) { // n/3
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);
        int n = nums.length;
        List<Integer> res = new ArrayList<>();
        for (int key : count.keySet()) if (count.get(key) > n/3) res.add(key);
        Collections.sort(res);
        return res;
    }

    public List<Integer> majorityElementNK(int[] nums, int k) { // n/k
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);
        int n = nums.length;
        List<Integer> res = new ArrayList<>();
        for (int key : count.keySet()) if (count.get(key) > n/k) res.add(key);
        Collections.sort(res);
        return res;
    }
}
```

**Complexity (Time & Space):**
- Majority Element I → Time: O(n), Space: O(n)
- Majority Element II → Time: O(n), Space: O(n)
- Majority Element n/k → Time: O(n), Space: O(n)

### Approach 2: Better / Improved (Sorting)

**Idea:**
- Sort array and pick elements at threshold index.

**Steps:**
- Sort array.
- For n/2 → middle element is majority.
- For n/3 or n/k → count elements while traversing to check frequency.

**Java Code:**
```java
class MajorityElementSort {
    public int majorityElement(int[] nums) { // n/2
        Arrays.sort(nums);
        return nums[nums.length/2];
    }

    public List<Integer> majorityElementII(int[] nums) { // n/3
        Arrays.sort(nums);
        List<Integer> res = new ArrayList<>();
        int n = nums.length, count = 1;
        for (int i=1;i<n;i++){
            if(nums[i]==nums[i-1]) count++;
            else {
                if(count>n/3) res.add(nums[i-1]);
                count=1;
            }
        }
        if(count>n/3) res.add(nums[n-1]);
        return res;
    }

    public List<Integer> majorityElementNK(int[] nums,int k){ // n/k
        Arrays.sort(nums);
        List<Integer> res = new ArrayList<>();
        int n=nums.length, count=1;
        for(int i=1;i<n;i++){
            if(nums[i]==nums[i-1]) count++;
            else{
                if(count>n/k) res.add(nums[i-1]);
                count=1;
            }
        }
        if(count>n/k) res.add(nums[n-1]);
        return res;
    }
}
```

**Complexity (Time & Space):**
- All variants → Time: O(n log n), Space: O(1) or O(n) depending on sort implementation

### Approach 3: Optimal / Most Efficient (Boyer-Moore Voting)

**Idea:**
- Use Boyer-Moore Voting Algorithm for majority elements.
- For n/k → generalized version maintaining k-1 candidates.

**Steps:**
- I → single candidate.
- II → two candidates.
- n/k → maintain k-1 candidates and counts.
- Verify candidates against threshold.

**Java Code:**
```java
class MajorityElementOptimal {
    public int majorityElement(int[] nums) { // n/2
        int count=0, candidate=0;
        for(int num: nums){
            if(count==0) candidate=num;
            count += (num==candidate)?1:-1;
        }
        return candidate;
    }

    public List<Integer> majorityElementII(int[] nums){ // n/3
        int n = nums.length;
        int cand1=0,cand2=1,count1=0,count2=0;
        for(int num:nums){
            if(num==cand1) count1++;
            else if(num==cand2) count2++;
            else if(count1==0){cand1=num;count1=1;}
            else if(count2==0){cand2=num;count2=1;}
            else{count1--;count2--;}
        }
        List<Integer> res = new ArrayList<>();
        count1=0; count2=0;
        for(int num:nums){
            if(num==cand1) count1++;
            else if(num==cand2) count2++;
        }
        if(count1>n/3) res.add(cand1);
        if(count2>n/3) res.add(cand2);
        Collections.sort(res);
        return res;
    }

    public List<Integer> majorityElementNK(int[] nums,int k){ // n/k
        Map<Integer,Integer> map = new HashMap<>();
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        int n=nums.length;
        List<Integer> res = new ArrayList<>();
        for(int key: map.keySet()){
            if(map.get(key)>n/k) res.add(key);
        }
        Collections.sort(res);
        return res;
    }
}
```

**Complexity (Time & Space):**
- Majority Element I → Time: O(n), Space: O(1)
- Majority Element II → Time: O(n), Space: O(1)
- Majority Element n/k → Time: O(n), Space: O(n)

---

## 7. Justification / Proof of Optimality

- Brute Force → Simple, works but uses extra space.
- Sorting → Reduces space, slightly slower due to O(n log n)
- Boyer-Moore → Best for I & II, minimal space and linear time.
- n/k generalization → Hash map required for multiple candidates, optimal for small k.
---

## 8. Variants / Follow-Ups

- Majority Element in a stream
- Find elements appearing more than n/4, n/5 times
- Sliding window majority element
- Top K frequent elements
---

## 9. Tips & Observations

- Threshold-based problems → think in terms of n/2, n/3, n/k.
- Boyer-Moore is extremely efficient for n/2 and n/3.
- Sorting works universally but costs O(n log n).
- For generalized n/k → maximum of k-1 elements can qualify.
- Always verify candidates for n/k to avoid false positives.
---

<!-- #endregion -->
