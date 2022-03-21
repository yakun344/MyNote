---
description: LeetCode 416
---

# Partition Equal Subset Sum

{% embed url="https://leetcode.com/problems/partition-equal-subset-sum" %}

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

&#x20;

**Example 1:**

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

&#x20;

**Constraints:**

* `1 <= nums.length <= 200`
* `1 <= nums[i] <= 100`

![](<../.gitbook/assets/image (53).png>)

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int total = 0;
        for (int n : nums) {
            total += n;
        }
        
        if (total % 2 != 0) {
            return false;
        }
        
        int subSum = total / 2;
        int n = nums.length;
        boolean[][] dp = new boolean[n + 1][subSum + 1];
        
        dp[0][0] = true;
        for (int i = 1; i <= n; ++i) {
            int cur = nums[i - 1];
            for (int j = 0; j <= subSum; ++j) {
                if (j < cur) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - cur];
                }
            }
        }
        
        return dp[n][subSum];
    }
}
```
