---
description: LeetCode 37
---

# Sudoku Solver

{% embed url="https://leetcode.com/problems/sudoku-solver/" %}



Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

**Example 1:**![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

```
Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
Explanation: The input board is shown above and the only valid solution is shown below:


```

**Constraints:**

* `board.length == 9`
* `board[i].length == 9`
* `board[i][j]` is a digit or `'.'`.
* It is **guaranteed** that the input board has only one solution.

{% hint style="info" %}
用DFS逐行枚举可能的值。

用dfs函数来返回当前坐标搜索下去能否得到符合条件的解。

当j == 9的时候，当前行已经枚举完成，接着枚举下一行。

当i == 9的时候，所有行已经枚举完成并找到合适的解， 返回true。

如果当前位置是空，那么继续搜下一个坐标。

对要判断的当前坐标，枚举0 - 9，去掉不符合条件的坐标，把符合条件坐标的数字改成当前数字并backtrack。

如果搜完0 - 9行还没有返回，说明没找到正确的值，返回false。

注意⚠️这里有个技巧，判断3 \* 3里是否有重复的值的时候:

#### rowIndex = (row / 3) \* 3 + num / 3;

#### colIndex = (col / 3 ) \* 3 + num % 3;

num = 0 - 9

e.g. row = 4, col = 5

\[3, 3] \[3, 4] \[3, 5]

\[4, 3] \[4, 4] \[4, 5]

\[5, 3] \[5, 4] \[5, 5]

由此找到\[4, 5]所在3 \* 3格子的所有值。
{% endhint %}

![](<../.gitbook/assets/image (22).png>)

```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtrack(board, 0, 0);
        return;
    }
    
    private boolean backtrack(char[][] board, int i, int j) {
        if (j == 9) {
            return backtrack(board, i + 1, 0);
        }
        
        if (i == 9) {
            return true;
        }
        
        if (board[i][j] != '.') {
            return backtrack(board, i, j + 1);
        }
        
        for (char ch = '1'; ch <= '9'; ++ch) {
            if (!isValid(board, i, j, ch)) {
                continue;
            }
            
            board[i][j] = ch;
            if (backtrack(board, i, j + 1)) {
                return true;
            }
            board[i][j] = '.';
        }
        
        return false;
    }
    
    private boolean isValid(char[][] board, int i, int j, char ch) {
        for (int num = 0; num < 9; ++ num) {
            if (board[i][num] == ch) {
                return false;
            }
            
            if (board[num][j] == ch) {
                return false;
            }
            
            if (board[(i / 3) * 3 + num / 3][(j / 3) * 3 + num % 3] == ch) {
                return false;
            }
        }
        
        return true;
    }
}
```
