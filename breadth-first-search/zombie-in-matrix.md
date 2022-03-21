---
description: LintCode 598
---

# Zombie in Matrix

{% embed url="https://www.lintcode.com/problem/zombie-in-matrix/" %}



Description

Give a two-dimensional grid, each grid has a value, 2 for wall, 1 for zombie, 0 for human (numbers 0, 1, 2).Zombies can turn the nearest people(up/down/left/right) into zombies every day, but can not through wall. How long will it take to turn all people into zombies? Return `-1` if can not turn all people into zombies.Example

Example 1:

```
Input:
[[0,1,2,0,0],
 [1,0,0,2,1],
 [0,1,0,0,0]]
Output:
2
```

Example 2:

```
Input:
[[0,0,0],
 [0,0,0],
 [0,0,1]]
Output:
4
```

{% hint style="info" %}
遍历一次grid，记录所有的people，统计所有的zombie，把zombie放入queue进行bfs。

bfs遍历的层数即是感染的天数。
{% endhint %}

```java
public class Solution {
    /**
     * @param grid: a 2D integer grid
     * @return: an integer
     */
    public int zombie(int[][] grid) {
        // write your code here
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int people = 0;
        Deque<int[]> queue = new LinkedList<>();
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == 1) {
                    queue.addFirst(new int[]{i, j});
                } else if (grid[i][j] == 0) {
                    people++;
                }
            }
        }

        if (people == 0) {
            return 0;
        }

        int[] rdir = new int[]{-1, 0, 1, 0};
        int[] cdir = new int[]{0, 1, 0, -1};
        int day = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] cor = queue.removeLast();
                for (int j = 0; j < 4; ++j) {
                    int m = cor[0] + rdir[j];
                    int n = cor[1] + cdir[j];
                    if (m >= 0 && m < grid.length && n >= 0 && n < grid[0].length) {
                        if (grid[m][n] == 1 || grid[m][n] == 2) {
                            continue;
                        }
                        grid[m][n] = 1;
                        people--;
                        if (people == 0) {
                            day++;
                            return day;
                        }
                        queue.addFirst(new int[]{m, n});
                    }
                }
            }
            day++;
        }

        return -1;
    }
}
```
