---
description: LintCode 125
---

# Backpack II

{% embed url="https://www.lintcode.com/problem/backpack-ii/description" %}

There are `n` items and a backpack with size `m`. Given array `A` representing the size of each item and array `V` representing the value of each item.

What's the maximum value can you put into the backpack?

#### Example

**Example 1:**

```
Input: m = 10, A = [2, 3, 5, 7], V = [1, 5, 2, 4]
Output: 9
Explanation: Put A[1] and A[3] into backpack, getting the maximum value V[1] + V[3] = 9 
```

**Example 2:**

```
Input: m = 10, A = [2, 3, 8], V = [2, 5, 8]
Output: 10
Explanation: Put A[0] and A[2] into backpack, getting the maximum value V[0] + V[2] = 10 
```

#### Challenge

O(nm) memory is acceptable, can you do it in O(m) memory?

#### Notice

1. `A[i], V[i], n, m` are all integers.
2. You can not split an item.
3. The sum size of the items you want to put into backpack can not exceed `m`.
4. Each item can only be picked up once

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @param V: Given n items with value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int[] V) {
        // write your code here
        
        
        //dp[i][j]表示前i个物体，在容量j的情况下，能取到的最大值
        int[][] dp = new int[A.length + 1][m + 1];
        for (int i = 0; i < A.length + 1; ++i) {
            for (int j = 0; j < m + 1; ++j) {
                
                //为了减少代码量，设定初始条件第一行/列为0
                if (i == 0 || j == 0) {
                    dp[i][j] = 0;
                    
                //如果不取第i个物体，当前的价值就是dp[i - 1][j]
                } else if (j < A[i - 1]) {
                    
                //如果取第i个物体，当前的价值就是dp[i - 1][j - A[i - 1]] + V[i - 1];
                //dp[i][j]就是不取当前的物品，而且剩余的空间能容纳当前的物品时，加入该物品
                    dp[i][j] = dp[i - 1][j];
                    
                //最大价值取两者最大
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - A[i - 1]] + V[i - 1]);
                }
            }
        }
        
        //递推出最大结果，在dp[][]右下角
        return dp[A.length][m];
    }
}
```
