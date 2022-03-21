---
description: LeetCode 1730
---

# Shortest Path to Get Food

{% embed url="https://leetcode.com/problems/shortest-path-to-get-food/" %}



You are starving and you want to eat food as quickly as possible. You want to find the shortest path to arrive at any food cell.

You are given an `m x n` character matrix, `grid`, of these different types of cells:

* `'*'` is your location. There is **exactly one** `'*'` cell.
* `'#'` is a food cell. There may be **multiple** food cells.
* `'O'` is free space, and you can travel through these cells.
* `'X'` is an obstacle, and you cannot travel through these cells.

You can travel to any adjacent cell north, east, south, or west of your current location if there is not an obstacle.

Return _the **length** of the shortest path for you to reach **any** food cell_. If there is no path for you to reach food, return `-1`.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/09/21/img1.jpg)

```
Input: grid = [["X","X","X","X","X","X"],["X","*","O","O","O","X"],["X","O","O","#","O","X"],["X","X","X","X","X","X"]]
Output: 3
Explanation: It takes 3 steps to reach the food.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/09/21/img2.jpg)

```
Input: grid = [["X","X","X","X","X"],["X","*","X","O","X"],["X","O","X","#","X"],["X","X","X","X","X"]]
Output: -1
Explanation: It is not possible to reach the food.
```

**Example 3:**![](https://assets.leetcode.com/uploads/2020/09/21/img3.jpg)

```
Input: grid = [["X","X","X","X","X","X","X","X"],["X","*","O","X","O","#","O","X"],["X","O","O","X","O","O","X","X"],["X","O","O","O","O","#","O","X"],["X","X","X","X","X","X","X","X"]]
Output: 6
Explanation: There can be multiple food cells. It only takes 6 steps to reach the bottom food.
```

**Example 4:**

```
Input: grid = [["O","*"],["#","O"]]
Output: 2
```

**Example 5:**

```
Input: grid = [["X","*"],["#","X"]]
Output: -1
```

**Constraints:**

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 200`
* `grid[row][col]` is `'*'`, `'X'`, `'O'`, or `'#'`.
* The `grid` contains **exactly one** `'*'`.

{% hint style="info" %}
bfs层级遍历，每一层加一次step, 用全局变量step来记录层数。

也会有一种情况就是遍历了所有的node之后还是没找到target，这个时候就需要用全局变量reached来记录是不是找到了target。
{% endhint %}

```java
class Solution {
    private int steps = 0;
    private boolean reached = false;
    public int getFood(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        boolean[][] visited= new boolean[grid.length][grid[0].length];
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == '*') {
                    visited[i][j] = true;
                    bfs(i, j, grid, visited);
                }
            }
        }
        if (reached) {
            return steps;
        } else {
            return -1;
        }
    }
    
    private void bfs(int row, int col, char[][] grid, boolean[][] visited) {
        int[] rdir = new int[]{-1, 0, 1, 0};
        int[] cdir = new int[]{0, 1, 0, -1};
        Deque<int[]> queue = new LinkedList<>();
        queue.addFirst(new int[]{row, col});
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int j = 0; j < size; ++j) {
                int[] node = queue.removeLast();
                for (int i = 0; i < 4; ++i) {
                    int m = node[0] + rdir[i];
                    int n = node[1] + cdir[i];
                    if (m >= 0 && m < grid.length && n >= 0 && n < grid[0].length && !visited[m][n]) {
                        if (grid[m][n] == 'X') {
                            continue;
                        }
                        if (grid[m][n] == '#') {
                            steps++;
                            reached = true;
                            return;
                        }
                        if (grid[m][n] == 'O') {
                            visited[m][n] = true;
                            queue.addFirst(new int[]{m, n});
                        }
                    }
                }
            }
            steps++;
        }
    }
}
```
