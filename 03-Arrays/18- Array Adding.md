<!-- #region 18- Array Adding -->

<br>
<h1 style="text-align:center; font-size:2.5em; font-weight:bold;">Q18: Array Adding</h1>
<br>

## 1. Understand the Problem
- **Read & Identify:** We are given two arrays, arr1 and arr2, each representing the digits of a number. The arrays may have different sizes.
- **Goal:** Add the two numbers represented by the arrays and return the result as an array of digits.
- **Paraphrase:** Treat arrays as numbers: most significant digit at index 0.  Perform addition like elementary school addition, handling carry.

---

## 2. Constraints

**Constraints:**
- 0 <= arr1[i], arr2[i] < 10
- Arrays may have different lengths


---

## 3. Examples & Edge Cases

**Example 1 (Normal Case):**
Input:
```text
2
2 1
4
1 2 3 4
```
Output:
```text
1 2 5 5
```


---

## 4. Approaches

### Approach 1: Simulate Addition from End

- **Idea:**
  - Start from the last index of both arrays
  - Add corresponding digits and carry
  - Store result in a list or array
  - Reverse the result at the end

**Java Code:**
```java
public static int[] addArrays(int[] arr1, int[] arr2) {
    int n1 = arr1.length, n2 = arr2.length;
    int i = n1 - 1, j = n2 - 1;
    int carry = 0;
    java.util.ArrayList<Integer> resList = new java.util.ArrayList<>();

    while (i >= 0 || j >= 0 || carry != 0) {
        int sum = carry;
        if (i >= 0) sum += arr1[i--];
        if (j >= 0) sum += arr2[j--];
        resList.add(sum % 10);
        carry = sum / 10;
    }

    // Reverse the result
    int[] result = new int[resList.size()];
    for (int k = 0; k < resList.size(); k++) {
        result[k] = resList.get(resList.size() - 1 - k);
    }
    return result;
}
```

**Complexity:**
- Time: O(max(n1, n2)) → each digit processed once
- Space: O(max(n1, n2) + 1) → for result

### Approach 2: Convert Arrays to Numbers Using long (Limited)

- **Idea:**
  - Convert arrays to numbers using long
  - Add them
  - Convert the sum back to array

**Java Code:**
```java
static int[] calSum(int a[], int b[], int n, int m) {
    // your code here
    long n1=0,n2=0,res=0;
    for(int i=0;i<n;i++){
        n1=n1*10+(long)a[i];
    }
    for(int i=0;i<m;i++){
        n2=n2*10+(long)b[i];
    }
    res=n1+n2;

    int s=0;
    long res1=0;
    while(res>0){
      res1=res1*10+res%10;
      res/=10;
      s++;
    }
    int [] z =new int[s];
    for(int i=0;i<s;i++){
      z[i]=(int)(res1%10);
      res1/=10;
    }
    return z;

  }
Limitations:

Fails for large arrays (overflow of long)

Works only if number fits in long
```

**Complexity:**
- Time: O(n+m+max(n,m)+max(n,m))=O(n+m)
- Space: res1 and other long/int variables → O(1)  Result array z[] of size s ≈ max(n, m) + 1 → O(max(n, m))  ✅ Overall Space Complexity:  𝑂 ( max ⁡ ( 𝑛 , 𝑚 ) ) O(max(n,m))


---

## 5. Justification / Proof of Optimality

- Approach 1 is optimal and works for arrays of any size.
- Approach 2 is simpler but limited by long size.
- Both approaches correctly handle carry and array lengths.

---

## 6. Variants / Follow-Ups

- Subtract two numbers represented by arrays
- Multiply two numbers represented by arrays
- Add multiple arrays of digits
- Handle arrays in reverse order (least significant digit first)

<!-- #endregion -->

