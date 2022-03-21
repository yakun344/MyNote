---
description: LeetCode 1099
---

# Two Sum Less Than K

Given an array `A` of integers and integer `K`, return the maximum `S` such that there exists `i < j` with `A[i] + A[j] = S` and `S < K`. If no `i, j` exist satisfying this equation, return -1.

**Example 1:**

```
Input: A = [34,23,1,24,75,33,54,8], K = 60
Output: 58
Explanation: 
We can use 34 and 24 to sum 58 which is less than 60.
```

**Example 2:**

```
Input: A = [10,20,30], K = 15
Output: -1
Explanation: 
In this case it's not possible to get a pair sum less that 15.
```

**Note:**

1. `1 <= A.length <= 100`
2. `1 <= A[i] <= 1000`
3. `1 <= K <= 2000`

* 先给数组排序
* 整体记录一个result，初始值可以是-1或者INTEGER.MIN\_VALUE
* 左指针指向头，右指针指向尾
* 移动指针，当前的左右指针的和记录为cur
* cur < K的时候，再和整体的result比较，如果这个值大，就更新result
* 循环移动左右指针
* 返回结果

```java
class Solution {
    public int twoSumLessThanK(int[] A, int K) {
        int result = -1;
        if (A == null || A.length == 0) {
            return result;
        }
        Arrays.sort(A);
        int left = 0, right = A.length - 1;
        while (left < right) {
            int cur = A[left] + A[right];
            if (cur < K) {
                if (cur > result) {
                    result = cur;
                }
                left++;
            } else {
                right--;
            }
        }
        return result;
    }
}
```
