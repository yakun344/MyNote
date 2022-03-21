---
description: LeetCode 1746
---

# Maximum Subarray Sum After One Operation

{% embed url="https://leetcode.com/problems/maximum-subarray-sum-after-one-operation/" %}

You are given an integer array `nums`. You must perform **exactly one** operation where you can **replace** one element `nums[i]` with `nums[i] * nums[i]`.&#x20;

Return _the **maximum** possible subarray sum after **exactly one** operation_. The subarray must be non-empty.

**Example 1:**

```
Input: nums = [2,-1,-4,-3]
Output: 17
Explanation: You can perform the operation on index 2 (0-indexed) to make nums = [2,-1,16,-3]. Now, the maximum subarray sum is 2 + -1 + 16 = 17.
```

**Example 2:**

```
Input: nums = [1,-1,1,1,-1,-1,1]
Output: 4
Explanation: You can perform the operation on index 1 (0-indexed) to make nums = [1,1,1,1,-1,-1,1]. Now, the maximum subarray sum is 1 + 1 + 1 + 1 = 4.
```

**Constraints:**

* `1 <= nums.length <= 105`
* `-104 <= nums[i] <= 104`

{% hint style="info" %}
subarray的最值问题，很多都可以用Kadane's 解决。

max1 以nums\[i] 为结尾最大的值；

max2 以nums\[i] 为结尾多一个操作数的最大值。
{% endhint %}

```java
class Solution {
    public int maxSumAfterOperation(int[] nums) {
        int max1 = nums[0];
        int max2 = nums[0] * nums[0];
        int res = Math.max(max1, max2);
        
        for (int i = 1; i < nums.length; ++i) {
            max2 = Math.max(max2 + nums[i], Math.max(max1 + nums[i] * nums[i], nums[i] * nums[i]));
            max1 = Math.max(max1 + nums[i], nums[i]);
            res = Math.max(res, Math.max(max1, max2));
        }
        
        return res;
    }
}
```
