---
description: LeetCode 63
---

# Unique Paths II

{% embed url="https://leetcode.com/problems/unique-paths-ii/" %}



A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as `1` and `0` respectively in the grid.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1
```

**Constraints:**

* `m == obstacleGrid.length`
* `n == obstacleGrid[i].length`
* `1 <= m, n <= 100`
* `obstacleGrid[i][j]` is `0` or `1`.

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        
        if (obstacleGrid[0][0] == 1) {
            return 0;
        }
        
        for (int i = 0; i < m; ++i) {
            if (obstacleGrid[i][0] == 1) {
                dp[i][0] = 0;
                break;
            } else {
                dp[i][0] = 1;
            }
        }
        
        for (int j = 0; j < n; ++j) {
            if (obstacleGrid[0][j] == 1) {
                dp[0][j] = 0;
                break;
            } else {
                dp[0][j] = 1;
            }
        }
        
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        
        return dp[m - 1][n - 1];
    }
}
```

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        
        int[][] dp = new int[m][n];
        
        //??????start??????osbtacle????????????????????????????????????finish
        if (obstacleGrid[0][0] == 1) {
            return 0;
        } 
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                
                //start????????????1
                if (i == 0 && j == 0) {
                    dp[0][0] = 1;
                    continue;
                }
                
                //???0???????????????????????????obstacle???????????????????????????????????????????????????
                if (i == 0 && obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                    continue;
                } else if (i == 0) {
                    dp[i][j] = dp[i][j - 1];
                    continue;
                }
                
                //???0???????????????????????????obstacle??????????????????????????????????????????
                if (j == 0 && obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                    continue;
                } else if (j == 0) {
                    dp[i][j] = dp[i - 1][j];
                    continue;
                }
                
                //??????obstacle????????????????????????dp[i - 1][j]???do[i][j - 1]?????????
                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        
        return dp[m - 1][n - 1];
    }
}
```
