---
description: LeetCode  302
---

# Smallest Rectangle Enclosing Black Pixels

{% embed url="https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/" %}

[https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/](https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/)

An image is represented by a binary matrix with `0` as a white pixel and `1` as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location `(x, y)` of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

**Example:**

```
Input:
[
  "0010",
  "0110",
  "0100"
]
and x = 0, y = 2

Output: 6
```

{% hint style="info" %}
注意⚠️题目中黑色像素是联通的，只有一块区域，所以可以用二分法在区间上查找，返回(rightBound - leftBound + 1) \* (botBound - topBound + 1)即可

时间复杂度O(nlogm + mlogn);
{% endhint %}

```java
class Solution {
    public int minArea(char[][] image, int x, int y) {
        if (image == null || image.length == 0) {
            return 0;
        }
        
        int row = image.length;
        int col = image[0].length;
        int result = 0;

        int leftBound = bsLeft(image, 0, y);
        int rightBound = bsRight(image, y, col - 1);
        int topBound = bsTop(image, 0, x);
        int botBound = bsBot(image, x, row - 1);
        result = (rightBound - leftBound + 1) * (botBound - topBound + 1);
        return result;
    }
    
    private int bsLeft(char[][] image, int start, int end) {
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (isEmptyCol(image, mid)) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (isEmptyCol(image, start)) {
            return end;
        } else {
            return start;
        }
    }
    
    private int bsRight(char[][] image, int start, int end) {
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (isEmptyCol(image, mid)) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (isEmptyCol(image, end)) {
            return start;
        } else {
            return end;
        }
    }

    private int bsTop(char[][] image, int start, int end) {
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (isEmptyRow(image, mid)) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (isEmptyRow(image, start)) {
            return end;
        } else {
            return start;
        }
    }

    private int bsBot(char[][] image, int start, int end) {
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (isEmptyRow(image, mid)) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (isEmptyRow(image, end)) {
            return start;
        } else {
            return end;
        }
    }

    private boolean isEmptyCol(char[][] image, int index) {
        for (int i = 0; i < image.length; ++i) {
            if (image[i][index] == '1') {
                return false;
            }
        }
        return true;
    }

    private boolean isEmptyRow(char[][] image, int index) {
        for (int i = 0; i < image[0].length; ++i) {
            if (image[index][i] == '1') {
                return false;
            }
        }
        return true;
    }
}

//⚠️题目中黑色像素是联通的，只有一块区域，所以可以用二分法在区间上查找，返回(rightBound - leftBound + 1) * (botBound - topBound + 1)即可
```

#### Depth-First Search Approach

```java
class Solution {
    private int left, right, top, bottom;
    public int minArea(char[][] image, int x, int y) {
        if (image == null || image.length == 0) {
            return 0;
        }
        
        int[] rdir = new int[]{0, -1, 0, 1};
        int[] cdir = new int[]{1, 0, -1, 0};
        image[x][y] = '0';
        left = right = y;
        top = bottom = x;
        dfs(image, x, y, rdir, cdir);
        return (right - left + 1) * (bottom - top + 1);
    }
    
    private void dfs(char[][] image,
                    int x,
                    int y,
                    int[] rdir,
                    int[] cdir) {
        for (int i = 0; i < 4; ++i) {
            int m = x + rdir[i];
            int n = y + cdir[i];
            if (m >= 0 && m < image.length && n >= 0 && n < image[0].length) {
                if (image[m][n] == '1') {
                    image[m][n] = '0';
                    left = Math.min(left, n);
                    right = Math.max(right, n);
                    top = Math.min(top, m);
                    bottom = Math.max(bottom, m);
                    dfs(image, m, n, rdir, cdir);
                }
            }
        }
    }
}
```

#### Breadth-First Search Approach

```java
class Solution {
    private int left, right, top, bottom;
    public int minArea(char[][] image, int x, int y) {
        left = right = y;
        top = bottom = x;
        
        int[] rdir = new int[]{0, 1, 0, -1};
        int[] cdir = new int[]{1, 0, -1, 0};
        
        Set<int[]> set = new HashSet<>();
        Deque<int[]> queue = new LinkedList<>();
        
        set.add(new int[]{x, y});
        queue.addLast(new int[]{x, y});
        image[x][y] = '0';
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] cur = queue.removeFirst();
                for (int j = 0; j < 4; ++j) {
                    int m = cur[0] + rdir[j];
                    int n = cur[1] + cdir[j];
                    if (!isValid(m, n, image)) continue;
                    if (set.contains(new int[]{m, n})) continue;
                    
                    left = Math.min(left, n);
                    right = Math.max(right, n);
                    top = Math.min(top, m);
                    bottom = Math.max(bottom, m);
                    
                    image[m][n] = '0';
                    int[] next = new int[]{m, n};
                    queue.addLast(next);
                    set.add(next);
                }
            }
        }
        
        return (right - left + 1) * (bottom - top + 1);
    }
    private boolean isValid(int m, int n, char[][] image) {
        if (m < 0 || m >= image.length || n < 0 || n >= image[0].length) {
            return false;
        }
        
        if (image[m][n] == '0') {
            return false;
        }
        
        return true;
    }
}
```
