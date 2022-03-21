---
description: LeetCode 1091
---

# Shortest Path in Binary Matrix

{% embed url="https://leetcode.com/problems/shortest-path-in-binary-matrix/" %}



Given an `n x n` binary matrix `grid`, return _the length of the shortest **clear path** in the matrix_. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

* All the visited cells of the path are `0`.
* All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/02/18/example1\_1.png)

```
Input: grid = [[0,1],[1,0]]
Output: 2
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/02/18/example2\_1.png)

```
Input: grid = [[0,0,0],[1,1,0],[1,1,0]]
Output: 4
```

**Example 3:**

```
Input: grid = [[1,0,0],[1,1,0],[1,1,0]]
Output: -1
```

**Constraints:**

* `n == grid.length`
* `n == grid[i].length`
* `1 <= n <= 100`
* `grid[i][j] is 0 or 1`

{% hint style="info" %}
判断什么时候找到目标点，要综合考虑testcase。

如果\[\[0]]，那么出队列的时候就要判断是不是目标点。
{% endhint %}

```java
class Solution {
    private int path = 1;
    private boolean reached;
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return -1;
        }
        
        if (grid[0][0] != 0 || grid[grid.length - 1][grid[0].length - 1] != 0) {
            return -1;
        }
        
        boolean[][] visited = new boolean[grid.length][grid[0].length];
        visited[0][0] = true;
        bfs(0, 0, grid, visited);
        if (reached) {
            return path;
        } else {
            return -1;
        }
    }
    
    private void bfs(int row, int col, int[][] grid, boolean[][] visited) {
        int[] rdir = new int[]{-1, -1, -1, 0, 0, 1, 1, 1};
        int[] cdir = new int[]{0, 1, -1, 1, -1, 0, 1, -1};
        
        Deque<int[]> queue = new LinkedList<>();
        queue.addFirst(new int[]{row, col});
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] node = queue.removeLast();
                if (node[0] == grid.length - 1 && node[1] == grid[0].length - 1 && grid[node[0]][node[1]] == 0) {
                    reached = true;
                    return;
                }
                for (int j = 0; j < 8; ++j) {
                    int m = node[0] + rdir[j];
                    int n = node[1] + cdir[j];
                    
                    if (m >= 0 && m < grid.length && n >= 0 && n < grid[0].length) {
                        if (visited[m][n]) continue;
                        if (grid[m][n] == 1) continue;
                        visited[m][n] = true;
                        queue.addFirst(new int[]{m, n});
                    }
                }
            }
            path++;
        }
    }
}
```
