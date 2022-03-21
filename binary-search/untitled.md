---
description: LeetCode 240
---

# Search a 2D Matrix II

{% embed url="https://leetcode.com/problems/search-a-2d-matrix-ii/" %}

Write an efficient algorithm that searches for a `target` value in an `m x n` integer `matrix`. The `matrix` has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid.jpg)

```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
Output: false
```

**Constraints:**

* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= n, m <= 300`
* `-109 <= matix[i][j] <= 109`
* All the integers in each row are **sorted** in ascending order.
* All the integers in each column are **sorted** in ascending order.
* `-109 <= target <= 109`

{% hint style="info" %}
时间复杂度O(m + n),从左下角往右上的方向走，因为在每一个row都是单调递增，每一个col也单调递增，于是可以走到头看能不能找到target
{% endhint %}

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length - 1, col = 0;
        while (row >= 0 && col < matrix[0].length) {
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] < target) {
                col++;
            } else {
                row--;
            }
        }
        return false;
    }
}
```

#### Approach 2(HashSet, not recommend):Time Complexity:O(i \* j)

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < matrix.length; ++i) {
            for (int j = 0; j < matrix[0].length; ++j) {
                set.add(matrix[i][j]);
            }
        }
        if (!set.add(target)) {
            return true;
        }
        else {
            return false;
        }
    }
}
```
