---
description: LeetCode 1197
---

# Minimum Knight Moves

{% embed url="https://leetcode.com/problems/minimum-knight-moves/" %}



In an **infinite** chess board with coordinates from `-infinity` to `+infinity`, you have a **knight** at square `[0, 0]`.

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

Return the minimum number of steps needed to move the knight to the square `[x, y]`.  It is guaranteed the answer exists.

**Example 1:**

```
Input: x = 2, y = 1
Output: 1
Explanation: [0, 0] → [2, 1]
```

**Example 2:**

```
Input: x = 5, y = 5
Output: 4
Explanation: [0, 0] → [2, 1] → [4, 2] → [3, 4] → [5, 5]
```

**Constraints:**

* `|x| + |y| <= 300`

{% hint style="danger" %}
划重点⚠️用HashSet来记录重复的点会超时，所以用一个小于输入规模的二维数组来记录重复情况。
{% endhint %}

```java
class Solution {
    public int minKnightMoves(int x, int y) {
        int[] rdir = new int[]{-1, -2, -2, -1, 1, 2, 2, 1};
        int[] cdir = new int[]{2, 1, -1, -2, 2, 1, -1, -2};
        
        int step = 0;
        
        boolean[][] visited = new boolean[605][605];
        Deque<int[]> queue = new LinkedList<>();
        //Set<int[]> visited = new HashSet<>();
        queue.addFirst(new int[]{0, 0});
        //visited.add(new int[]{0, 0});
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] cur = queue.removeLast();
                if (cur[0] == x && cur[1] == y) {
                    return step;
                }
                
                for (int j = 0; j < 8; ++j) {
                    int m = cur[0] + rdir[j];
                    int n = cur[1] + cdir[j];
                    int[] temp = new int[]{m, n};
                    
                    if (!visited[m + 302][n + 302]) {
                        visited[m + 302][n + 302] = true;
                        queue.addFirst(temp);
                    }
                }
            }
            step++;
        }
        
        return step;
    }
}
```
