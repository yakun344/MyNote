---
description: LeetCode 62
---

# Unique Paths

{% embed url="https://leetcode.com/problems/unique-paths/" %}



A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Example 1:**![](https://assets.leetcode.com/uploads/2018/10/22/robot\_maze.png)

```
Input: m = 3, n = 7
Output: 28
```

**Example 2:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

**Example 3:**

```
Input: m = 7, n = 3
Output: 28
```

**Example 4:**

```
Input: m = 3, n = 3
Output: 6
```

**Constraints:**

* `1 <= m, n <= 100`
* It's guaranteed that the answer will be less than or equal to `2 * 109`.

![](<../.gitbook/assets/image (31).png>)

#### Dynamic Programming

Time Complexity: O(mn)  Space Complexity: O(mn)

{% hint style="info" %}
到达坐标为(i, j)的点，有两种方式，一种是从(i - 1, j)，一种是从(i, j - 1)。

用dp二维数组，每一个坐标代表robot到达当前点有几种方式。

dp\[i]\[j] = dp\[i - 1]\[j] + dp\[i]\[j - 1]。

初始状态，当i == 0或j == 0时，dp\[i]\[j] = 1. dp\[0]\[0] = 1.
{% endhint %}

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i == 0 || j == 0) {
                    dp[i][j] = 1;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        
        return dp[m - 1][n - 1];
    }
}
```

#### Memoization Search

Time Complexity: O(mn)  Space Complexity: O(mn)

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] memo = new int[m + 1][n + 1];
        return search(m, n, memo);
    }
    
    private int search(int m, int n, int[][] memo) {
        if (m <= 0 || n <= 0) {
            return 0;
        }
        if (m == 1 && n == 1) {
            return 1;
        }
        
        if (memo[m][n] == 0) {
            memo[m][n] = search(m - 1, n, memo) + search(m, n - 1, memo);
        }
        
        return memo[m][n];
    }
}
```

#### TLE

Time Complexity: O(2 ^ (mn)) Space Complexity: O(1)&#x20;

```java
class Solution {

    //O(2^(m + n))
    private int count = 0;
    public int uniquePaths(int m, int n) {
        dfs(1, 1, m, n);
        return count;
    }
    private void dfs(int x, int y, int m, int n) {
        if (x == m && y == n) {
            count++;
            return;
        }
        
        if (isValid(x, y + 1, m, n)) {
            dfs(x, y + 1, m, n);
        }
        
        if (isValid(x + 1, y, m, n)) {
            dfs(x + 1, y, m, n);
        }
    }
    private boolean isValid(int x, int y, int m, int n) {
        if (x > m || y > n) {
            return false;
        }
        return true;
    }
}
```
