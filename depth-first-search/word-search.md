---
description: LeetCode 79
---

# Word Search

{% embed url="https://leetcode.com/problems/word-search" %}

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

{% hint style="info" %}
Try every valid char in the board(so we need a if-statement), remember to change current character in order to avoid duplicate paths.
{% endhint %}

![](<../.gitbook/assets/image (44).png>)

```java
class Solution {
    private int[] rdir = new int[]{0, 1, 0, -1};
    private int[] cdir = new int[]{1, 0, -1, 0};
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; ++i) {
            for (int j = 0; j < board[0].length; ++j) {
                if (board[i][j] == word.charAt(0)) {
                    if (dfs(board, word, rdir, cdir, i, j, 0)) {
                        return true;
                    }
                }
            }
        }
        
        return false;
    }
    
    private boolean dfs(char[][] board,
                       String word,
                       int[] rdir,
                       int[] cdir,
                       int i,
                       int j,
                       int start) {
        if (start == word.length() - 1) {
            return true;
        }
        
        char ch = board[i][j];
        board[i][j] = '#';
        
        for (int idx = 0; idx < 4; ++idx) {
            int m = i + rdir[idx];
            int n = j + cdir[idx];
            
            if (m < 0 || m >= board.length ||
                n < 0 || n >= board[0].length ||
                board[m][n] != word.charAt(start + 1)) continue;
            
            if (dfs(board, word, rdir, cdir, m, n, start + 1)) {
                return true;
            }
        }
        
        board[i][j] = ch;
        return false;
    }
}
```
