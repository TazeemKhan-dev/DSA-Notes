<!-- #region 190-Snapshot Array -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q190: Snapshot Array</h1>

## 1. Problem Statement

- You must design a data structure SnapshotArray supporting:
  * SnapshotArray(int length)
    * Initialize array of given size, all values = 0.
  * void set(int index, int val)
    * Set arr[index] = val.
  * int snap()
    * Take a snapshot, return a snap_id (snap count − 1).
  * int get(int index, int snap_id)
    * Return value at that index as of that snapshot.
- You will be given operations and must output results for snap() and get().
---

## 2. Problem Understanding

- We need a persistent array structure where:
  * Many updates happen (set)
  * After each snapshot, we must be able to retrieve old values
  * Up to 50k operations, so storing full copies per snap (length up to 50k) is too slow (50k * 50k = 2.5B → impossible)
- We must store only the changes per index.
- Key idea:
  * Maintain, for each index, a list of (snap_id, value) pairs.
  * On set, update the latest entry.
  * On snap, increment snap counter.
  * On get, binary search the version ≤ snap_id.
- This gives efficient storage and fast queries.
---

## 3. Constraints

- 1 ≤ length ≤ 50k
- Total operations ≤ 50k
- Values ≤ 1e9
- Snapshots may be large in count
- Must support fast get → binary search required
---

## 4. Edge Cases

- Multiple set() calls before a snap() → only last one matters
- Querying snap_id for an index that was never set → should return 0
- Setting the same value multiple times should not break logic
- Sparse updates → most indices unchanged
---

## 5. Examples

```text
Example 1:

set 0 6
snap → returns 0
set 0 3
get 0 0 → 6


Example 2:

set 1 4
set 3 34
snap → 0
get 3 0 → 34
set 3 5
snap → 1
get 3 1 → 5
```

---

## 6. Approaches

### Approach 1: Brute Force Snapshots (Invalid for Constraints)

**Idea:**
- Store a full copy of the array on every snap.

**Complexity (Time & Space):**
- Time: O(n) per snap → bad
- Space: O(n * snaps) → impossible

### Approach 2: Map of Snapshots Per Index (Optimal)

**Idea:**
- For each index, maintain:
  * index → list of (snap_id, value)
- Process:
  * set(i, v) → append or update the latest entry for current snap.
  * snap() → increment snap counter, return previous.
  * get(i, snap_id) → binary search list of versions.

**Steps:**
- Maintain snap_id = 0
- For each index, store a list of (snap, value) pairs.
- On set, if last snap matches current snap → overwrite; else append new version.
- On snap, return snap_id++.
- On get, binary search to find largest snap <= snap_id

**Java Code:**
```java
class SnapshotArray {
    
    List<int[]>[] history;
    int snapId = 0;

    public SnapshotArray(int length) {
        history = new ArrayList[length];
        for (int i = 0; i < length; i++) {
            history[i] = new ArrayList<>();
            history[i].add(new int[]{0, 0}); // at snap 0, value = 0
        }
    }

    public void set(int index, int val) {
        List<int[]> list = history[index];
        int[] last = list.get(list.size() - 1);

        if (last[0] == snapId) {
            last[1] = val; // same snap — update last value
        } else {
            list.add(new int[]{snapId, val});
        }
    }

    public int snap() {
        return snapId++;
    }

    public int get(int index, int snap_id) {
        List<int[]> list = history[index];

        int l = 0, r = list.size() - 1;
        int ans = 0;

        while (l <= r) {
            int mid = (l + r) / 2;
            if (list.get(mid)[0] <= snap_id) {
                ans = list.get(mid)[1];
                l = mid + 1;
            } else {
                r = mid - 1;
            }
        }

        return ans;
    }
}
```

**💭 Intuition Behind the Approach:**
- Snapshots do NOT require copying the array.
- We only store changed values.
- Binary search allows retrieval in logarithmic time.
- This is classic persistent array design using versioning.

**Complexity (Time & Space):**
- Time:
  * set → O(1)
  * snap → O(1)
  * get → O(log M) where M = number of changes at index
- Space:
  * O(total number of set operations)
- Optimal for given constraints.

---

## 7. Justification / Proof of Optimality

- Storing full arrays is impossible.
- Tracking only changes is efficient and avoids duplication.
- Binary search ensures fast retrieval.
- This is the standard official solution used in competitive programming and interviews.
---

## 8. Variants / Follow-Ups

- Snapshot matrix
- Fully persistent arrays
- Time-travel data structures
- Immutable arrays with versioning
---

## 9. Tips & Observations

- Only store entries when value changes.
- Early snapshots share 0-values initially.
- Always binary search by snap_id.
- List of snapshot changes stays sorted automatically.
---

## 10. Pitfalls

- Overwriting value incorrectly if same snap occurs multiple set() calls
- Forgetting to initialize with (0,0)
- Using HashMap instead of List → slower
---

<!-- #endregion -->
