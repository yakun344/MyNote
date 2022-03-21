---
description: LeetCode 51 LintCode 33
---

# N-Queens

{% embed url="https://leetcode.com/problems/n-queens/" %}

{% embed url="https://www.lintcode.com/problem/n-queens/description" %}

The _n_-queens puzzle is the problem of placing _n_ queens on an _n_×_n_ chessboard such that no two queens attack each other.

![](https://assets.leetcode.com/uploads/2018/10/12/8-queens.png)

Given an integer _n_, return all distinct solutions to the _n_-queens puzzle.

Each solution contains a distinct board configuration of the _n_-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

**Example:**

```
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

1. 科学命名变量名
2. stringbuilder method的用法
3. 因为每一层只有一个Queen，所以我们可以用一个List\<Integer> 来代表n\*n棋盘中Queen的位置，例如：\[0, 3, 1, 2].
4. 递归的定义，在n\*n的棋盘中找到所有能够使Queen相互不攻击的排列方式，并且把结果加入到result中。

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> results = new ArrayList<>();
        if (n <= 0) {
            return results;
        }
        List<Integer> col = new ArrayList<>();
        search(n, col, results);
        
        return results;
    }
    
    private void search(int n, List<Integer> col, List<List<String>> results) {
        if (col.size() == n) {
            results.add(drawChessBoard(col));
            return;
        }
        
        for (int colIndex = 0; colIndex < n; ++ colIndex) {
            if (isValid(colIndex, col)) {
                col.add(colIndex);
                search(n, col, results);
                col.remove(col.size() - 1);
            }
        }
    }
    
    private boolean isValid(int colIndex, List<Integer> col) {
        int row = col.size();
        for (int rowIndex = 0; rowIndex < row; ++rowIndex) {
        
            //y == ny
            if (col.get(rowIndex) == colIndex) {
                return false;
            }
            
            //x - nx == y - ny
            if (rowIndex - row == col.get(rowIndex) - colIndex) {
                return false;
            }
            
            //x + y == nx + ny
            if (rowIndex + col.get(rowIndex) == row + colIndex) {
                return false;
            }
        }
        
        return true;
    }
    
    private List<String> drawChessBoard(List<Integer> col) {
        List<String> chess = new ArrayList<>();
        for (int i = 0; i < col.size(); ++i) {
            StringBuilder sb = new StringBuilder();
            for (int j = 0; j < col.size(); ++j) {
                sb.append(j == col.get(i) ? 'Q' : '.');
            }
            chess.add(sb.toString());
        }
        
        return chess;
    }
}
```
