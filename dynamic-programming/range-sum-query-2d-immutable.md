---
description: LeetCode 304
---

# Range Sum Query 2D - Immutable

{% embed url="https://leetcode.com/problems/range-sum-query-2d-immutable" %}

Given a 2D matrix `matrix`, handle multiple queries of the following type:

* Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the NumMatrix class:

* `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
* `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/sum-grid.jpg)

```
Input
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
Output
[null, 8, 11, 12]

Explanation
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)
```

&#x20;

**Constraints:**

* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 200`
* `-105 <= matrix[i][j] <= 105`
* `0 <= row1 <= row2 < m`
* `0 <= col1 <= col2 < n`
* At most `104` calls will be made to `sumRegion`.

**Algorithm**

We used a cumulative sum array in the [1D version](https://leetcode.com/course/chapters/leetcode-101/range-sum-query-immutable/). We notice that the cumulative sum is computed with respect to the origin at index 0. Extending this analogy to the 2D case, we could pre-compute a cumulative region sum with respect to the origin at (0, 0).

![Sum OD](https://leetcode.com/static/images/courses/sum\_od.png)\
Sum(OD) is the cumulative region sum with respect to the origin at (0, 0).

How do we derive Sum(ABCD) using the pre-computed cumulative region sum?

![Sum OB](https://leetcode.com/static/images/courses/sum\_ob.png)\
Sum(OB) is the cumulative region sum on top of the rectangle.

![Sum OC](https://leetcode.com/static/images/courses/sum\_oc.png)\
Sum(OC) is the cumulative region sum to the left of the rectangle.

![Sum OA](https://leetcode.com/static/images/courses/sum\_oa.png)\
Sum(OA) is the cumulative region sum to the top left corner of the rectangle.

Note that the region Sum(OA) is covered twice by both Sum(OB) and Sum(OC). We could use the principle of inclusion-exclusion to calculate Sum(ABCD) as following:

Sum(ABCD) = Sum(OD) - Sum(OB) - Sum(OC) + Sum(OA)

```java
class NumMatrix {
    private int[][] dp;
    public NumMatrix(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return;
        dp = new int[matrix.length + 1][matrix[0].length + 1];
        for (int r = 0; r < matrix.length; ++r) {
            for (int c = 0; c < matrix[0].length; ++c) {
                dp[r + 1][c + 1] = dp[r + 1][c] + dp[r][c + 1] + matrix[r][c] - dp[r][c];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return dp[row2 + 1][col2 + 1] - dp[row1][col2 + 1] - dp[row2 + 1][col1] + dp[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```
