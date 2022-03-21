---
description: LintCode 42
---

# Maximum Subarray II

{% embed url="https://www.lintcode.com/problem/42/" %}

Description

Given an array of integers, find two non-overlapping subarrays which have the largest sum.\
The number in each subarray should be contiguous.\
Return the largest sum.

The subarray should contain at least one numberExample

**Example 1:**

Input:

```
nums = [1, 3, -1, 2, -1, 2]
```

Output:

```
7
```

Explanation:

the two subarrays are \[1, 3] and \[2, -1, 2] or \[1, 3, -1, 2] and \[2].\
**Example 2:**

Input:

```
nums = [5,4]
```

Output:

```
9
```

Explanation:

the two subarrays are \[5] and \[4].Challenge

Can you do it in time complexity O(_n_) ?

{% hint style="info" %}
Kadane's Algorithm

Maximum Subarray的变形，记录两个non-overlapping subarray的最大和。

left\[]数组记录从左到右以i结尾最大和。

right\[]数组记录从右到左以i结尾最大和。

因为两个subarray是non-overlapping，所以枚举划分的线，找到全局的最大值即可。

res = Math.max(res, left\[i] + right\[i + 1])。
{% endhint %}

```java
public class Solution {
    /*
     * @param nums: A list of integers
     * @return: An integer denotes the sum of max two non-overlapping subarrays
     */
    public int maxTwoSubArrays(List<Integer> nums) {
        // write your code here
        int n = nums.size();
        int[] left = new int[n];
        int[] right = new int[n];

        int cur = nums.get(0);
        int max = cur;
        left[0] = nums.get(0);
        for (int i = 1; i < n; ++i) {
            cur = Math.max(cur + nums.get(i), nums.get(i));
            max = Math.max(max, cur);
            left[i] = max;
        }

        cur = nums.get(n - 1);
        max = cur;
        right[n - 1] = cur;
        for (int i = n - 2; i >= 0; --i) {
            cur = Math.max(cur + nums.get(i), nums.get(i));
            max = Math.max(max, cur);
            right[i] = max;
        }

        int res = Integer.MIN_VALUE;
        for (int i = 0; i < n - 1; ++i) {
            res = Math.max(res, left[i] + right[i + 1]);
        }

        return res;
    }
}
```
