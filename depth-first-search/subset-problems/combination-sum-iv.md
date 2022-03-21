---
description: LeetCode 377
---

# Combination Sum IV

{% embed url="https://leetcode.com/problems/combination-sum-iv/" %}

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

**Example:**

```
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

**Follow up:**\
What if negative numbers are allowed in the given array?\
How does it change the problem?\
What limitation we need to add to the question to allow negative numbers?

{% hint style="danger" %}
Bottom-up, DP solution.
{% endhint %}

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 0; i < dp.length; ++i) {
            for (int num : nums) {
                if (i - num >= 0) dp[i] += dp[i - num];
            }
        }
        return dp[target];
    }
}
```
