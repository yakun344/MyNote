---
description: LintCode 92
---

# Backpack

{% embed url="https://www.lintcode.com/problem/backpack/description" %}

Given _n_ items with size Ai, an integer _m_ denotes the size of a backpack. How full you can fill this backpack?

#### Example

```
Example 1:
	Input:  [3,4,8,5], backpack size=10
	Output:  9

Example 2:
	Input:  [2,3,5,7], backpack size=12
	Output:  12
	
```

#### Challenge

O(n x m) time and O(m) memory.

O(n x m) memory is also acceptable if you do not know how to optimize memory.

#### Notice

You can not divide any item into small pieces.

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        // write your code here
        
        
        //初始化dp数组
        //i表示前i个物品
        //j表示当前空间大小
        //dp[i][j]表示当前(i, j)，前i个物品能否装满j个空间
        //能装满的话就是true，否则false；
        boolean dp[][] = new boolean[A.length + 1][m + 1];
        for (int i = 0; i < A.length + 1; ++i) {
            for (int j = 0; j < m + 1; ++j) {
                dp[i][j] = false;
            }
        }
        
        //前0个物品可以放得下0个空间
        //所以dp[0][0]为true；
        dp[0][0] = true;
        
        //状态转移
        for (int i = 1; i < A.length + 1; ++i) {
            for (int j = 0; j < m + 1; ++j) {
                
                //不取当前的物品，结果就是i - 1
                dp[i][j] = dp[i - 1][j];
                
                //取当前的物品，check能否放的下但前物品
                //可以的话dp[i][j]为true；
                if (j >= A[i - 1] && dp[i - 1][j - A[i - 1]]) {
                    dp[i][j] = true;
                }
            }
        }
        
        //画出格子后，从后往前找到最大的值
        for (int i = m; i >= 0; --i) {
            if (dp[A.length][i]) {
                return i;
            }
        }
        
        //没找到就返回false；
        return 0;
    }
}
```
