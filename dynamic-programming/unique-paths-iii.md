---
description: LeetCode 980
---

# Unique Paths III

{% embed url="https://leetcode.com/problems/unique-paths-iii/" %}



You are given an `m x n` integer array `grid` where `grid[i][j]` could be:

* `1` representing the starting square. There is exactly one starting square.
* `2` representing the ending square. There is exactly one ending square.
* `0` representing empty squares we can walk over.
* `-1` representing obstacles that we cannot walk over.

Return _the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once_.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique1.jpg)

```
Input: grid = [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
Output: 2
Explanation: We have the following two paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique2.jpg)

```
Input: grid = [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
Output: 4
Explanation: We have the following four paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
```

**Example 3:**![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique3-.jpg)

```
Input: grid = [[0,1],[2,0]]
Output: 0
Explanation: There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.
```

**Constraints:**

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 20`
* `1 <= m * n <= 20`
* `-1 <= grid[i][j] <= 2`
* There is exactly one starting cell and one ending cell.

```java
class Solution {
    int count = 0, x, y, sum;
    int[] rdir = new int[]{0, 1, 0, -1};
    int[] cdir = new int[]{1, 0, -1, 0};
    public int uniquePathsIII(int[][] grid) {
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == 0) {
                    sum++;
                }
                if (grid[i][j] == 1) {
                    x = i;
                    y = j;
                }
            }
        }
        
        dfs(grid, x, y);
        
        return count;
    }
    
    private void dfs(int[][] grid, int r, int c) {
        
        for (int i = 0; i < 4; ++i) {
            int m = r + rdir[i];
            int n = c + cdir[i];
            
            if (m < 0 || n < 0 || m >= grid.length || n >= grid[0].length || grid[m][n] < 0) {
                continue;
            }
            
            if (grid[m][n] == 1) {
                continue;
            }
            
            if (grid[m][n] == 2) {
                if (sum == 0) {
                    count++;
                }
                continue;
            }
            
            grid[m][n] = -2;
            sum--;
            dfs(grid, m, n);
            sum++;
            grid[m][n] = 0;
        }
    }
}
```
