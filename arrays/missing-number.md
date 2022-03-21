---
description: LeetCode 268 LintCode 196
---

# Missing Number

{% embed url="https://leetcode.com/problems/missing-number/" %}

{% embed url="https://www.lintcode.com/problem/missing-number/description" %}

Given an array containing n distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.

**Example 1:**

```
Input: [3,0,1]
Output: 2
```

**Example 2:**

```
Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```

**Note**:\
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

{% hint style="info" %}
Be careful the base case.

双指针，前后两个指针，i指针指向当前的点，当前的数字没出现在它应该出现的位置，就换到它本来的位置。
{% endhint %}

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length, i = 0;
        while (i < n) {
            while (nums[i] != i && nums[i] < n) {
                int t = nums[i];
                nums[i] = nums[t];
                nums[t] = t;
            }
            ++i;
        }
        
        for (i = 0; i < n; ++i) {
            if (nums[i] != i) {
                return i;
            }
        }
        
        return n;
    }
}
```
