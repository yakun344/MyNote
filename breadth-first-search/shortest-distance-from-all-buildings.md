---
description: LeetCode 317
---

# Shortest Distance from All Buildings

{% embed url="https://leetcode.com/problems/shortest-distance-from-all-buildings/" %}

You want to build a house on an emptyland which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values **0**, **1** or **2**, where:

* Each **0** marks an empty land which you can pass by freely.
* Each **1** marks a building which you cannot pass through.
* Each **2** marks an obstacle which you cannot pass through.

**Example:**

```
Input: [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]

1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

Output: 7 

Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2),
             the point (1,2) is an ideal empty land to build a house, as the total 
             travel distance of 3+3+1=7 is minimal. So return 7.
```

**Note:**\
There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

```java
class Solution {
    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }

        int result = Integer.MAX_VALUE;
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == 0) {
                    result = Math.min(result, bfs(grid, new int[]{i, j}));
                }
            }
        }
        return result == Integer.MAX_VALUE ? -1 : result;
    }

    private int bfs(int[][] grid, int[] node) {
        Deque<int[]> queue = new LinkedList<>();
        Set<String> visited = new HashSet<>();

        int[] rdir = new int[]{-1, 0, 1, 0};
        int[] cdir = new int[]{0, 1, 0, -1};

        queue.addFirst(node);
        visited.add(node[0] + "," + node[1]);

        int dist = 0;
        int sum = 0;

        while (!queue.isEmpty()) {
            dist++;
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] cord = queue.removeLast();
                for (int k = 0; k < 4; ++k) {
                    int m = cord[0] + rdir[k];
                    int n = cord[1] + cdir[k];
                    
                    if (isValid(grid, m, n, visited)) {
                        visited.add(m + "," + n);
                        if (grid[m][n] == 1) {
                            sum += dist;
                        }
                        if (grid[m][n] == 0) {
                            queue.addFirst(new int[]{m, n});
                        }
                    }
                }
            }
        }
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == 1 && !visited.contains(i + "," + j)) {
                    return Integer.MAX_VALUE;
                }
            }
        }
        return sum;
    }

    private boolean isValid(int[][] grid, int m, int n, Set<String> visited) {
        if (m < 0 || m >= grid.length || n < 0 || n >= grid[0].length) {
            return false;
        }

        if (visited.contains(m + "," + n)) {
            return false;
        }

        if (grid[m][n] == 2) {
            return false;
        }

        return true;
    }
}
```

* Bfs starts from "0", if we find a building that was not visited, so we can's set house.&#x20;
* Update current cordinate sum through sum += dist.&#x20;
* Update between two cordinates dist++.
* Set\<int\[]> should be avoided, Set\<String> is a better way.
