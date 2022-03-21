---
description: LeetCode 322
---

# Coin Change

{% embed url="https://leetcode.com/problems/coin-change/" %}



You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

**Example 4:**

```
Input: coins = [1], amount = 1
Output: 1
```

**Example 5:**

```
Input: coins = [1], amount = 2
Output: 2
```

**Constraints:**

* `1 <= coins.length <= 12`
* `1 <= coins[i] <= 231 - 1`
* `0 <= amount <= 104`

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        //dp数组开多大，取决于数组最后一个要用到的index
        int[] dp = new int[amount + 1];
        
        //初始状态，用转移方程算不出来的，需要手动定义
        dp[0] = 0;
        for (int i = 1; i <= amount; ++i) {
            
            //正无穷表示无法组成大小为i的硬币们
            dp[i] = Integer.MAX_VALUE;
            for (int j = 0; j < coins.length; ++j) {
                if (i >= coins[j] && dp[i - coins[j]] != Integer.MAX_VALUE) {
                    //转移方程
                    //拼出i需要的最少硬币数 = min(coins[0],...) + 1;
                    //拼出i - coins[j]所需要的最小数目再加上1
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        
        if (dp[amount] == Integer.MAX_VALUE) {
            return -1;
        }
        
        return dp[amount];
    }
    //O(S * n)
}
```
