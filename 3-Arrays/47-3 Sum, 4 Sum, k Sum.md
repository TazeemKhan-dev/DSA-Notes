<!-- #region 47-3 Sum, 4 Sum, k Sum -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q47: 3 Sum, 4 Sum, k Sum</h1>
<br>

## 1. Understand the Problem

- 3 Sum
- Read & Identify: Given array nums, find all unique triplets [nums[i], nums[j], nums[k]] such that i != j != k and nums[i] + nums[j] + nums[k] == 0.
- Goal: Return all unique triplets with sum = 0.
- Paraphrase: Find combinations of three numbers summing to zero, without duplicates.
- 4 Sum
- Read & Identify: Given array nums and target, find all quadruplets [nums[a], nums[b], nums[c], nums[d]] summing to target.
- Goal: Return all unique quadruplets summing to target.
- Paraphrase: Find combinations of four numbers summing to target, avoiding duplicates.
- k Sum
- Read & Identify: Generalize to k numbers summing to target.
- Goal: Return all unique k-tuples summing to target.
- Paraphrase: Recursive or iterative approach for finding combinations of k numbers summing to target.

---

## 2. Input & Output

- **Input:**
```text
| Problem | n range      | nums[i] range        | target range        |
| ------- | ------------ | -------------------- | ------------------- |
| 3 Sum   | 1 ≤ n ≤ 3000 | -10⁴ ≤ nums[i] ≤ 10⁴ | 0                   |
| 4 Sum   | 1 ≤ n ≤ 200  | -10⁴ ≤ nums[i] ≤ 10⁴ | -10⁴ ≤ target ≤ 10⁴ |
| k Sum   | 1 ≤ n ≤ 1000 | -10⁴ ≤ nums[i] ≤ 10⁴ | -10⁵ ≤ target ≤ 10⁵ |
```


---

## 3. Examples & Edge Cases

**Example 1 (3 Sum):**
Input:
```text
[2, -2, 0, 3, -3, 5]
```
Output:
```text
[[-3, 0, 3], [-3, -2, 5], [-2, 0, 2]]
```

**Example 2 (4 Sum):**
Input:
```text
[1, -2, 3, 5, 7, 9], target = 7
```
Output:
```text
[[-2, 1, 3, 5]]
```

**Example 3 (k Sum):**
Input:
```text
[1, 2, 3, 4, 5], k = 3, target = 9
```
Output:
```text
[[1,3,5],[2,3,4]]
```


---

## 4. Approaches

### Approach 1: Brute Force (All combinations)

- **Idea:**
  - Generate all combinations of 3, 4, or k numbers.

**Java Code:**
```java
public List<List<Integer>> fourSumBrute(int[] nums, int target) {
    Set<List<Integer>> set = new HashSet<>();
    int n = nums.length;
    for (int i = 0; i < n-3; i++)
        for (int j = i+1; j < n-2; j++)
            for (int k = j+1; k < n-1; k++)
                for (int l = k+1; l < n; l++)
                    if (nums[i]+nums[j]+nums[k]+nums[l]==target){
                        int[] quad = {nums[i], nums[j], nums[k], nums[l]};
                        Arrays.sort(quad);
                        set.add(Arrays.asList(quad[0],quad[1],quad[2],quad[3]));
                    }
    return new ArrayList<>(set);
}
```

**Complexity:**
- Time: O(n³) for 3 Sum, O(n⁴) for 4 Sum, O(nᵏ log k) for k Sum
- Space: O(n) for storing results (HashSet to remove duplicates)

### Approach 2: Sorting + Two Pointers (Optimal)

- **Idea:**
  - Sort array.
  - Fix first k-2 numbers.
  - Use two pointers on remaining array to find pairs summing to target - sum(fixed numbers).
  - Skip duplicates.

**Java Code:**
```java
public List<List<Integer>> fourSumOptimal(int[] nums, int target) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    int n = nums.length;
    for (int i=0;i<n-3;i++){
        if(i>0 && nums[i]==nums[i-1]) continue;
        for(int j=i+1;j<n-2;j++){
            if(j>i+1 && nums[j]==nums[j-1]) continue;
            int left = j+1, right = n-1;
            while(left<right){
                long sum = (long)nums[i]+nums[j]+nums[left]+nums[right];
                if(sum==target){
                    res.add(Arrays.asList(nums[i],nums[j],nums[left],nums[right]));
                    while(left<right && nums[left]==nums[left+1]) left++;
                    while(left<right && nums[right]==nums[right-1]) right--;
                    left++; right--;
                } else if(sum<target) left++;
                else right--;
            }
        }
    }
    return res;
}
```

**Complexity:**
- Time: O(n³) for 4 Sum, O(n²) for 3 Sum
- Space: O(1) excluding output

### Approach 3: Recursion (Generalized k Sum)

- **Idea:**
  - Use recursion to reduce k Sum to 2 Sum:
  - Base case: 2 Sum using two pointers.
  - Recursive case: fix first element, recursively call (k-1) Sum on remaining array and adjusted target.

**Java Code:**
```java
public List<List<Integer>> kSum(int[] nums, int target, int k, int start){
    List<List<Integer>> res = new ArrayList<>();
    int n = nums.length;
    if(k==2){ // base case 2-sum
        int left = start, right = n-1;
        while(left<right){
            int sum = nums[left]+nums[right];
            if(sum==target){
                res.add(Arrays.asList(nums[left], nums[right]));
                while(left<right && nums[left]==nums[left+1]) left++;
                while(left<right && nums[right]==nums[right-1]) right--;
                left++; right--;
            } else if(sum<target) left++;
            else right--;
        }
    } else {
        for(int i=start;i<n-k+1;i++){
            if(i>start && nums[i]==nums[i-1]) continue;
            for(List<Integer> subset : kSum(nums,target-nums[i],k-1,i+1)){
                List<Integer> temp = new ArrayList<>();
                temp.add(nums[i]);
                temp.addAll(subset);
                res.add(temp);
            }
        }
    }
    return res;
}
```

**Complexity:**
- Time: O(n^(k-1))
- Space: O(k) recursion stack + output


---

## 5. Justification / Proof of Optimality

- Brute force works but infeasible for large n.
- Sorting + two pointers is optimal for 3 Sum / 4 Sum.
- Recursion generalizes to k Sum.
- Duplicates handled efficiently with sorting and skipping repeated elements.

---

## 6. Variants / Follow-Ups

- k Sum Closest: Find sum closest to target instead of exact match.
- Count instead of listing tuples.
- Return indices instead of values.
- Subset sum / combination sum problems – similar principles, with or without duplicates.

<!-- #endregion -->

