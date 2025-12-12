<!-- #region 191-Avoid Flood in The City -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q191: Avoid Flood in The City</h1>

## 1. Problem Statement

- You are given an array rains:
  * If rains[i] > 0, it rains on lake rains[i], and that lake becomes full.
  * If rains[i] == 0, it is a dry day, and you may choose any lake to dry.
- Your task:
- Return an array ans of same length:
  * If it rains at day i, ans[i] = -1
  * If dry day (0), ans[i] = lake_to_dry
- Goal: avoid any lake from being rained on while already full.
- If it is impossible → return an empty array.
- If multiple valid outputs exist → return any
---

## 2. Problem Understanding

- This is a scheduling / greedy problem.
- Key observation:
  * If lake x gets rain on day i, and it rains on x again on some future day j, we must dry lake x at some dry day between i and j.
  * We must use dry days wisely.
  * Whenever we see a repeated rain on lake x, we must find a dry day > lastRain[x] and assign that dry day to drying x.
- Thus:
  * Track last rain day of each lake.
  * Maintain a set of all available dry days (indices where rains[i] = 0).
  * For each lake that rains again, find the earliest dry day that is after the last rain day.
  * If such a day does not exist → flood → return empty array.
- Data structures:
  * lastRain = HashMap<lake, day>
  * TreeSet<Integer> dryDays for dry day positions
  * ans[] for output
---

## 3. Constraints

- 1 ≤ n ≤ 100000
- 0 ≤ rains[i] ≤ 1e9
- Greedy + TreeSet → required
---

## 4. Edge Cases

- No dry days at all → must not repeat any lake
- Many repeated rains → must match dry day order perfectly
- If lake never rains twice → no need to dry it
- Drying an empty lake does not harm — can output any dummy lake number
---

## 5. Examples

```text
Example 1
Input

4
5 2 3 4
Output

-1 -1 -1 -1
Explanation

After the first day full lakes are [5]
After the second day full lakes are [5,2]
After the third day full lakes are [5,2,3]
After the fourth day full lakes are [5,2,3,4]
There's no day to dry any lake and there is no flood in any lake.
Example 2
Input

6
2 1 0 0 1 2
Output

-1 -1 1 2 -1 -1
Explanation  : 
After the first day full lakes are [2]
After the second day full lakes are [1,2]
After the third day, we dry lake 1. Full lakes are [2]
After the fourth day, we dry lake 2. There is no full lakes.
After the fifth day, full lakes are [1].
After the sixth day, full lakes are [1,2].
It is easy that this scenario is flood-free. [-1,-1,2,1,-1,-1] is another acceptable scenario.
```

---

## 6. Approaches

### Approach 1: Brute Force (Impossible)

**Idea:**
- On each repeated rain, scan all previous dry days to pick one.
- Why impossible?
  * Worst case O(n²).

### Approach 2: Greedy + TreeSet (Optimal)

**Idea:**
- Maintain:
  * A map lastRain[lake] = day
  * A TreeSet of dry days (indices where rains[i] == 0)
  * When lake x rains at day i:
    * If this lake already rained before on day p
    * We must pick a dry day d such that d > p
    * Use TreeSet.ceiling(p+1) to find the earliest suitable dry day
    * Assign that dry day to drying this lake
    * Remove that day from TreeSet
  * Mark ans[i] = -1 on rain days

**Steps:**
- Iterate through array:
  * If rains[i] > 0:
    * If lake seen before → must find a drying day after lastRain
    * If none exists → return empty
    * Else assign dry day
    * Update lastRain
  * If rains[i] == 0:
    * Add day index to TreeSet
    * Temporarily set ans[i] = 1 (or any placeholder)

**Java Code:**
```java
public int[] avoidFlood(int[] rains) {
    int n = rains.length;
    int[] ans = new int[n];

    Map<Integer, Integer> lastRain = new HashMap<>();
    TreeSet<Integer> dryDays = new TreeSet<>();

    for (int i = 0; i < n; i++) {

        if (rains[i] == 0) {
            dryDays.add(i);
            ans[i] = 1;  // placeholder; will be overwritten if needed
        } 
        else {
            int lake = rains[i];
            ans[i] = -1;

            if (lastRain.containsKey(lake)) {
                int prev = lastRain.get(lake);
                Integer dry = dryDays.ceiling(prev + 1);

                if (dry == null) {
                    return new int[0];
                }

                ans[dry] = lake;
                dryDays.remove(dry);
            }

            lastRain.put(lake, i);
        }
    }

    return ans;
}
```

**💭 Intuition Behind the Approach:**
- The only dry days that matter are the ones occurring after a lake was last filled.
- We must pick the earliest dry day possible, or we block future lakes from being saved.
- TreeSet enforces "earliest valid dry day" greedily.
- This is the same logic behind interval scheduling and avoiding conflicts.

**Complexity (Time & Space):**
- Time: O(n log n)
  * Because TreeSet operations (ceiling, remove) are log n
- Space: O(n)
  * For lastRain map, answer array, and dryDays set

---

## 7. Variants / Follow-Ups

- Schedule tasks with cooldown
- Prevent repeated bookings
- Flood scheduling variants (LeetCode #1488)
---

## 8. Tips & Observations

- Always dry the lake that appears earliest in future to avoid conflicts
- TreeSet.ceiling(x) is the core trick
- Placeholder values on dry days are fine until assigned
- Drying unnecessary lakes does not break correctness
---

## 9. Pitfalls

- Forgetting to remove dry day after using it
- Assigning dry day to wrong lake
- Not handling placeholder value correctly
- Using linear search instead of TreeSet → TLE
---

<!-- #endregion -->
