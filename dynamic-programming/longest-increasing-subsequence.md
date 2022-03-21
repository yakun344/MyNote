---
description: LeetCode 300
---

# Longest Increasing Subsequence

{% embed url="https://leetcode.com/problems/longest-increasing-subsequence/" %}

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

**Constraints:**

* `1 <= nums.length <= 2500`
* `-104 <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

![](<../.gitbook/assets/image (37).png>)

{% hint style="info" %}
dp\[i]记录以nums\[i]结尾的最长subsequence的长度。

对于每一个nums\[i]，长度至少是dp\[i] = 1。

nums\[j]在nums\[i]前面，如果nums\[j] < nums\[i]，就可以保证i和j这两个数字是单调递增的，此时可以更新dp\[i]。

状态转移方程dp\[i] = max(dp\[i], dp\[j] + 1)。

当j == i时，更新max。

时间复杂度O(n^2)。
{% endhint %}

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int max = 0;
        for (int i = 0; i < nums.length; ++i) {
            dp[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    dp[i] = dp[i] > dp[j] + 1 ? dp[i] : dp[j] + 1;
                }
            }
            
            if (dp[i] > max) {
                max = dp[i];
            }
        }
        
        return max;
    }
}
```
