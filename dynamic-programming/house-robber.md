---
description: LeetCode 198
---

# House Robber

{% embed url="https://leetcode.com/problems/house-robber/" %}



You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

**Constraints:**

* `1 <= nums.length <= 100`
* `0 <= nums[i] <= 400`

{% hint style="info" %}
dp数组代表当前nums的index抢钱最多的方案。

dp\[i] = Math.max(dp\[i - 1], dp\[i - 2] + nums\[i - 1])

当前的index有两种情况，抢的话是index - 2的最优解加上nums\[index]这种情况，不抢的话也可能是index - 1的最优解。两者比大小选出一个值最大的就是当前index的最优情况。
{% endhint %}

```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        
        int[] dp = new int[nums.length + 1];
        dp[0] = 0;
        dp[1] = nums[0];
        for (int i = 2; i < dp.length; ++i) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
        }
        
        return dp[nums.length];
    }
}
```

```java
class Solution {
    public int rob(int[] nums) {
        int N = nums.length;
        
        if (N == 0) {
            return 0;
        }
        
        int[] dp = new int[N + 1];
        
        dp[N] = 0;
        dp[N - 1] = nums[N - 1];
        
        for (int i = N - 2; i >= 0; --i) {
            dp[i] = Math.max(dp[i + 1], dp[i + 2] + nums[i]);
        }
        
        return dp[0];
    }
}
```
