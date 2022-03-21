---
description: LeetCode 200
---

# Number of Islands

{% embed url="https://leetcode.com/problems/number-of-islands/" %}

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**

```
Input:
11000
11000
00100
00011

Output: 3
```

```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        
        //Store all nodes' cordinates valued '1' into a list;
        List<int[]> nodes = new ArrayList<>();
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == '1') {
                    nodes.add(new int[]{i, j});
                }
            }
        }
        int count = 0; 
        
        for (int[] node : nodes) {
            int r = node[0], c = node[1];
            if (grid[r][c] == '1') {
                bfs(grid, new int[] {r, c});
                ++count;
            }
        }
        
        return count;
    }
    private void bfs(char[][] grid, int[] cord) {
        int r = cord[0], c = cord[1];
        int R = grid.length, C = grid[0].length;
        
        int[] rdir = new int[]{-1, 0, 1, 0};
        int[] cdir = new int[]{0, 1, 0, -1};
        
        //Turn '1' to '0' when visited;
        grid[r][c] = '0';
        
        Deque<int[]> queue = new LinkedList<>();
        queue.addFirst(cord);
        
        while (!queue.isEmpty()) {
            int[] node = queue.removeLast();
            r = node[0];
            c = node[1];
            for (int i = 0; i < 4; ++i) {
                int m = r + rdir[i];
                int n = c + cdir[i];
                
                //Remember the variable's caps;
                if (m >= 0 && m < R && n >= 0 && n < C) {
                    if (grid[m][n] == '0') continue;
                    grid[m][n] = '0';
                    queue.addFirst(new int[]{m, n});
                }
            }
        }
    }
}
```

{% hint style="info" %}
再刷一变code简短了很多。
{% endhint %}

#### Breadth First Search Approach

```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == '1') {
                    bfs(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    
    private void bfs(char[][] grid, int row, int col) {
        Queue<int[]> queue = new LinkedList<>();
        int[] rdir = new int[]{-1, 0, 1, 0};
        int[] cdir = new int[]{0, -1, 0, 1};
        grid[row][col] = '0';
        queue.offer(new int[]{row, col});
        while (!queue.isEmpty()) {
            int[] last = queue.poll();
            int r = last[0];
            int c = last[1];
            for (int i = 0; i < 4; ++i) {
                int m = r + rdir[i];
                int n = c + cdir[i];
                if (m >= 0 && m < grid.length && n >= 0 && n < grid[0].length) {
                    if (grid[m][n] == '0') {
                        continue;
                    }
                    
                    grid[m][n] = '0';
                    queue.offer(new int[]{m, n});
                }
            }
        }
    }
}
```

#### Depth First Search Approach

```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j < grid[0].length; ++j) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count++;
                }
            }
        }
        
        return count;
    }
    
    private void dfs(char[][] grid, int row, int col) {
        
        
        int[] rdir = new int[]{1, 0, -1, 0};
        int[] cdir = new int[]{0, 1, 0, -1};
        
        for (int i = 0; i < 4; ++i) {
            int m = row + rdir[i];
            int n = col + cdir[i];
            if (m >= 0 && m < grid.length && n >= 0 && n < grid[0].length) {
                if (grid[m][n] == '1') {
                    grid[m][n] = '0';
                    dfs(grid, m, n);
                }
            }
        }
    }
}
```
