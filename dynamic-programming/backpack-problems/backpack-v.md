---
description: LintCode 563
---

# Backpack V

{% embed url="https://www.lintcode.com/problem/backpack-v/description" %}

Given n items with size `nums[i]` which an integer array and all positive numbers. An integer `target` denotes the size of a backpack. Find the number of possible fill the backpack.

`Each item may only be used once`

#### Example

Given candidate items `[1,2,3,3,7]` and target `7`,

```
A solution set is: 
[7]
[1, 3, 3]
```

return `2`

```java
public class Solution {
    /**
     * @param nums: an integer array and all positive numbers
     * @param target: An integer
     * @return: An integer
     */
    public int backPackV(int[] nums, int target) {
        // write your code here
        
        int m = target;
        int n = nums.length;
        //dp[i][j]代表对于前i个物品装j的空间，有多少个方案数，就是方案数的个数
        int[][] dp = new int[n + 1][m + 1];
        
        //初始化0个物品0个空间有1种方案数
        dp[0][0] = 1;
        //对于装或者不装两种情况，它的方案数是叠加的
        for (int i = 1; i < n + 1; ++i) {
            for (int j = 0; j < m + 1; ++j) {
                
                //枚举每一个物品，如果选择不装的话，可以有i-1，j的情况
                dp[i][j] = dp[i - 1][j];
                
                //可以装的话，它的方案数就是前一个的状态
                //加上装满j-A[i - 1]的方案数
                if (j >= nums[i - 1]) {
                    dp[i][j] += dp[i - 1][j - nums[i - 1]];
                }
            }
        }
        
        //答案就是前n个物品，装满m空间的方案数
        return dp[n][m];
    }
}
```
