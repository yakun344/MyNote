---
description: LeetCode 484
---

# Find Permutation

{% embed url="https://leetcode.com/problems/find-permutation/" %}



A permutation `perm` of `n` integers of all the integers in the range `[1, n]` can be represented as a string `s` of length `n - 1` where:

* `s[i] == 'I'` if `perm[i] < perm[i + 1]`, and
* `s[i] == 'D'` if `perm[i] > perm[i + 1]`.

Given a string `s`, reconstruct the lexicographically smallest permutation `perm` and return it.

**Example 1:**

```
Input: s = "I"
Output: [1,2]
Explanation: [1,2] is the only legal permutation that can represented by s, where the number 1 and 2 construct an increasing relationship.
```

**Example 2:**

```
Input: s = "DI"
Output: [2,1,3]
Explanation: Both [2,1,3] and [3,1,2] can be represented as "DI", but since we want to find the smallest lexicographical permutation, you should return [2,1,3]
```

**Constraints:**

* `1 <= s.length <= 105`
* `s[i]` is either `'I'` or `'D'`.

```java
/*
遇到连续的k个D，就把当前index到index + k个数字都反转。
IDIIDD
1234567 sorted
1324765 res
**/
class Solution {
    public int[] findPermutation(String s) {
        int n = s.length();
        int[] arr = new int[n + 1];
        
        for (int i = 0; i <= n; ++i) {
            arr[i] = i + 1;
        }
        
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == 'D') {
                int cur = i;
                while (i < n && s.charAt(i) == 'D') {
                    i++;
                }
                reverse(arr, cur, i);
            }
        }
        return arr;
    }
    
    private void reverse(int[] arr, int i, int j) {
        while (i < j) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            
            i++;
            j--;
        }
    }
}
```
