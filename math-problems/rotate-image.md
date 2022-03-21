---
description: LeetCode 48
---

# Rotate Image

{% embed url="https://leetcode.com/problems/rotate-image" %}



ou are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place\_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

**Example 3:**

```
Input: matrix = [[1]]
Output: [[1]]
```

**Example 4:**

```
Input: matrix = [[1,2],[3,4]]
Output: [[3,1],[4,2]]
```

&#x20;

**Constraints:**

* `matrix.length == n`
* `matrix[i].length == n`
* `1 <= n <= 20`
* `-1000 <= matrix[i][j] <= 1000`

{% hint style="info" %}
先上下反转，再以对称轴（左上到右下）反转。
{% endhint %}

```java
class Solution {
    public void rotate(int[][] matrix) {
        int s = 0, e = matrix.length - 1;
        while (s < e) {
            int[] temp = matrix[s];
            matrix[s] = matrix[e];
            matrix[e] = temp;
            s++;
            e--;
        }
        
        for (int i = 0; i < matrix.length; ++i) {
            for (int j = i + 1; j < matrix[0].length; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }//顺时针
        //int matrix.length = n;
        //for (int i = 0; i < n; ++i) {
        //    for (int j = 0; j < n; ++j) {
        //        int temp = matrix[i][j];
        //        matrix[i][j] = matrix[n - 1 - i][n - 1 - j];
        //        matrix[n - 1 - i][n - 1 - j] = temp;
        //    }
        
        return;
    }
}
```
