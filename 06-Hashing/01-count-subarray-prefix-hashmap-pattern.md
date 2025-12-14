<!-- #region 01-COUNT SUBARRAY (Prefix + HashMap Pattern) -->

<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q01: COUNT SUBARRAY (Prefix + HashMap Pattern)</h1>

## 1. Problem Understanding

- **SUM-BASED COUNT PROBLEMS**
- 1️⃣ Subarray Sum Equals K
  * Store: prefixSum
  * Check: prefixSum - K
  * Base: map.put(0,1)
  * ✅ You’ve DONE & UNDERSTOOD
- 2️⃣ Subarray Sum Equals 0
  * Store: prefixSum
  * Check: same prefixSum
  * Base: map.put(0,1)
  * ⚠️ Same as sum=K where K=0
  * ✅ You’ve DONE (implicitly)
- 3️⃣ Count Subarrays with Sum Less Than K
  * ❌ Does NOT use this pattern
  * Uses sliding window (only for positives)
- 4️⃣ Subarray Sum Divisible by K
  * Store: prefixSum % K (remainder)
  * Check: same remainder
  * Base: map.put(0,1)
  * Negative fix: (sum % k + k) % k
  * ✅ You’ve DONE & ASKED DOUBTS → means learned
- 5️⃣ Count Subarrays with Even Sum
  * Store: sum % 2
  * Check: same parity
  * Base: map.put(0,1)
  * 🧠 Variant of divisible-by-K (K=2)
- 6️⃣ Count Subarrays with Sum in Range [L, R]
  * ⚠️ Advanced
  * Uses prefix sums + ordered map / Fenwick tree
  * ❌ Not basic hashmap pattern

- **XOR-BASED COUNT PROBLEMS**
- 7️⃣ Subarray XOR Equals K
  * Store: prefixXor
  * Check: prefixXor ^ K
  * Base: map.put(0,1)
  * ✅ You’ve DONE & UNDERSTOOD
- 8️⃣ Count Subarrays with XOR = 0
  * Check: same prefixXor
  * Base: map.put(0,1)
  * 🧠 XOR version of zero-sum
  * ✅ You can do this now easily
  * Store: prefixXor

- **⚖️ EQUAL-FREQUENCY PROBLEMS (VERY IMPORTANT)**
- 9️⃣ Equal Number of 0s and 1s
  * Transform: 0 → -1, 1 → +1
  * Store: prefixSum
  * Check: same sum
  * Base: map.put(0,1)
  * ✅ You’ve DONE
- 🔟 Equal Number of 0s, 1s, and 2s
  * Store:
  * (count1 - count0)
  * (count2 - count1)
  * Check: same pair
  * Base: map.put("0#0",1)
  * ✅ You’ve DONE (and debugged mistakes)
- 1️⃣1️⃣ Equal Number of Even and Odd
  * Transform:
  * even → +1
  * odd → -1
  * Store: prefixSum
  * Check: same sum
  * 🧠 Same logic as 0s & 1s
- 1️⃣2️⃣ Equal Vowels and Consonants
  * Transform:
  * vowel → +1
  * consonant → -1
  * Store: prefixSum
  * Check: same sum
  * 🧠 Pattern recognition test

- **📐 DIFFERENCE / BALANCE PROBLEMS**
- 1️⃣3️⃣ Longest / Count Subarray with Given Difference
  * Store: prefixDiff
  * Check: prefixDiff - K
  * ⚠️ COUNT → hashmap freq
  * ⚠️ LONGEST → first index map
  * 🧠 Same prefix math
- 1️⃣4️⃣ Count Subarrays Where Ones > Zeros
  * ❌ Different (monotonic / prefix + BIT)
- 🧪 MODULO / REMAINDER VARIANTS
- 1️⃣5️⃣ Count Subarrays with Sum % K == R
  * Store: prefixSum % K
  * Check: (rem - R + K) % K
  * 🧠 Generalized divisible-by-K

- **FINAL MEMORY TABLE (WRITE THIS)**
- COUNT subarray problems that use hashmap:
- 1. Exact sum → store SUM → check SUM - K
- 2. XOR target → store XOR → check XOR ^ K
- 3. Divisible → store REMAINDER → check SAME
- 4. Equal counts → store DIFFERENCES
- 5. Zero target → store PREFIX → check SAME
---

<!-- #endregion -->
