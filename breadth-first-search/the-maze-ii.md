---
description: LeetCode 505
---

# The Maze II

{% embed url="https://leetcode.com/problems/the-maze-ii/" %}



There is a ball in a `maze` with empty spaces (represented as `0`) and walls (represented as `1`). The ball can go through the empty spaces by rolling **up, down, left or right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the `m x n` `maze`, the ball's `start` position and the `destination`, where `start = [startrow, startcol]` and `destination = [destinationrow, destinationcol]`, return _the shortest **distance** for the ball to stop at the destination_. If the ball cannot stop at `destination`, return `-1`.

The **distance** is the number of **empty spaces** traveled by the ball from the start position (excluded) to the destination (included).

You may assume that **the borders of the maze are all walls** (see examples).

**Example 1:**![](https://assets.leetcode.com/uploads/2021/03/31/maze1-1-grid.jpg)

```
Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: 12
Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.
The length of the path is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/03/31/maze1-2-grid.jpg)

```
Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [3,2]
Output: -1
Explanation: There is no way for the ball to stop at the destination. Notice that you can pass through the destination but you cannot stop there.
```

**Example 3:**

```
Input: maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], start = [4,3], destination = [0,1]
Output: -1
```

**Constraints:**

* `m == maze.length`
* `n == maze[i].length`
* `1 <= m, n <= 100`
* `maze[i][j]` is `0` or `1`.
* `start.length == 2`
* `destination.length == 2`
* `0 <= startrow, destinationrow <= m`
* `0 <= startcol, destinationcol <= n`
* Both the ball and the destination exist in an empty space, and they will not be in the same position initially.
* The maze contains **at least 2 empty spaces**.

{% hint style="info" %}
找最短路径，就不能用Queue搜索了，要用heap。

**注意⚠️：每次要处理从heap里poll出来的node，这样可以保证每次处理都是从start开始距离最短的点。**

每次移动，除了要维持一个visited的coordination，还要维持从start到当前坐标的路径长度。

剩下的事交给bfs。
{% endhint %}

```java
class Solution {
    public class Node {
        int x, y, len;
        public Node(int x, int y, int len){
            this.x = x;
            this.y = y;
            this.len = len;
        }
    }
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int[] rdir = new int[]{0, 1, 0, -1};
        int[] cdir = new int[]{1, 0, -1, 0};
        
        PriorityQueue<Node> heap = new PriorityQueue<>((a, b) ->{
            return a.len - b.len;
        });
        
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        heap.offer(new Node(start[0], start[1], 0));
        
        while (!heap.isEmpty()) {
            Node cur = heap.poll();
            if (cur.x == destination[0] && cur.y == destination[1]) {
                return cur.len;
            }
            if (visited[cur.x][cur.y]) continue;
            visited[cur.x][cur.y] = true;
            
            for (int i = 0; i < 4; ++i) {
                int nx = cur.x, ny = cur.y, nlen = cur.len;
                while (nx >= 0 && nx < maze.length && ny >= 0 && ny < maze[0].length && maze[nx][ny] == 0) {
                    nx += rdir[i];
                    ny += cdir[i];
                    nlen++;
                }
                nx -= rdir[i];
                ny -= cdir[i];
                nlen--;
                
                heap.offer(new Node(nx, ny, nlen));
            }
        }
        
        return -1;
    }
}
```
