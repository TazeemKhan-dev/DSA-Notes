<!-- #region 217- Merge Intervals -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q221: Merge Intervals</h1>

## 1. Problem Statement

- You are given n intervals, where each interval is represented by a start and end time.
- Your task is to merge all overlapping intervals and return the resulting list of mutually exclusive intervals.
- Each merged interval should be printed on a new line.
---

## 2. Problem Understanding

- Intervals may be given in any order
  * Two intervals overlap if:
  * current.start <= previous.end
- Overlapping intervals must be merged into a single interval:
  * merged.start = min(start)
  * merged.end   = max(end)
- Non-overlapping intervals remain separate
- Key insight:
  * Sorting intervals by start time makes merging straightforward.
---

## 3. Constraints

- 1 <= n <= 10000
- Interval values can be large
- Output should be sorted by start time
---

## 4. Edge Cases

- Single interval
- All intervals overlapping
- No intervals overlapping
- Intervals fully contained within others
- Intervals touching at boundaries ([1,3] and [3,5] → overlapping)
---

## 5. Examples

```text
Input:
1 3
2 4
6 8
9 10

Output:
1 4
6 8
9 10
Explanation

Given intervals: [1,3],[2,4],[6,8],[9,10], we have only two overlapping intervals here,[1,3] and [2,4]. Therefore we will merge these two and return [1,4],[6,8], [9,10].


Input:
6 8
1 9
2 4
6 7

Output:
1 9
```

---

## 6. Approaches

### Approach 1: Brute Force (Repeated Merging)

**Idea:**
- Compare every interval with every other interval
- Merge overlapping ones repeatedly until no more merges are possible

**Steps:**
- Loop through all pairs of intervals
- Merge if overlapping
- Repeat until no changes occur

**Java Code:**
```java
public List<int[]> mergeBrute(List<int[]> intervals) {
    boolean merged;
    do {
        merged = false;
        for (int i = 0; i < intervals.size(); i++) {
            for (int j = i + 1; j < intervals.size(); j++) {
                int[] a = intervals.get(i);
                int[] b = intervals.get(j);

                if (a[1] >= b[0] && b[1] >= a[0]) {
                    a[0] = Math.min(a[0], b[0]);
                    a[1] = Math.max(a[1], b[1]);
                    intervals.remove(j);
                    merged = true;
                    break;
                }
            }
            if (merged) break;
        }
    } while (merged);

    return intervals;
}
```

**💭 Intuition Behind the Approach:**
- Keep merging overlapping intervals until no overlap remains
- Direct but inefficient

**Complexity (Time & Space):**
- Time: O(N^2) — repeated pairwise checks
- Space: O(1) — in-place merging

### Approach 2: Sorting + Linear Merge (Greedy)

**Idea:**
- Sort intervals by start time
- Merge intervals greedily in one pass

**Steps:**
- Sort intervals based on start time
- Initialize current interval
- For each next interval:
  * If overlapping → extend current interval
  * Else → store current and move on

**Java Code:**
```java
public List<int[]> mergeIntervals(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> res = new ArrayList<>();

    int start = intervals[0][0];
    int end = intervals[0][1];

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= end) {
            end = Math.max(end, intervals[i][1]);
        } else {
            res.add(new int[]{start, end});
            start = intervals[i][0];
            end = intervals[i][1];
        }
    }
    res.add(new int[]{start, end});
    return res;
}
```

**💭 Intuition Behind the Approach:**
- Sorting ensures overlapping intervals are adjacent
- Greedy merge avoids unnecessary comparisons

**Complexity (Time & Space):**
- Time: O(N log N) — sorting
- Space: O(N) — output list

### Approach 3: Stack-Based Merge (Alternative Optimal)

**Idea:**
- Sort intervals
- Use a stack to merge overlapping intervals

**Steps:**
- Sort intervals by start
- Push first interval to stack
- For each interval:
  * If overlapping → merge with stack top
  * Else → push new interval

**Java Code:**
```java
public List<int[]> mergeStack(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    Stack<int[]> st = new Stack<>();

    for (int[] in : intervals) {
        if (st.isEmpty() || st.peek()[1] < in[0]) {
            st.push(in);
        } else {
            st.peek()[1] = Math.max(st.peek()[1], in[1]);
        }
    }

    return new ArrayList<>(st);
}
```

**💭 Intuition Behind the Approach:**
- Stack stores already merged intervals
- Similar to linear greedy merge but explicit

**Complexity (Time & Space):**
- Time: O(N log N) — sorting
- Space: O(N) — stack

---

## 7. Justification / Proof of Optimality

- Sorting is required to group overlapping intervals
- Linear merging after sorting is optimal
- Stack-based version is optional and conceptually similar
---

## 8. Variants / Follow-Ups

- Insert interval
- Interval intersection
- Non-overlapping intervals count
- Meeting room problems
---

## 9. Tips & Observations

- Always sort by start time first
- Touching intervals (end == start) are overlapping
- Greedy works because earlier start dominates
---

## 10. Pitfalls

- Forgetting to add last interval
- Not sorting input
- Confusing overlapping vs disjoint intervals
- Modifying input without caution
---

<!-- #endregion -->
