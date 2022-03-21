---
description: LeetCode 74
---

# Search a 2D Matrix

{% embed url="https://leetcode.com/problems/search-a-2d-matrix/" %}

Write an efficient algorithm that searches for a value in an `m x n` matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

**Constraints:**

* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 100`
* `-104 <= matrix[i][j], target <= 104`

{% hint style="info" %}
矩阵二分，中点的位置mid = start + (end - start) / 2;

中点的坐标matrix\[mid / matrix\[0].length]\[mid % matrix\[0].length];
{% endhint %}

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int start = 0, end = matrix.length * matrix[0].length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            int row = mid / matrix[0].length, col = mid % matrix[0].length;
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (matrix[start / matrix[0].length][start % matrix[0].length] == target) {
            return true;
        } else if (matrix[end / matrix[0].length][end % matrix[0].length] == target) {
            return true;
        }
        return false;
    }
}
```
