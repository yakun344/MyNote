---
description: LeetCode 52
---

# N-Queens II

{% embed url="https://leetcode.com/problems/n-queens-ii/" %}



The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _the number of distinct solutions to the **n-queens puzzle**_.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown.
```

**Example 2:**

```
Input: n = 1
Output: 1
```

**Constraints:**

* `1 <= n <= 9`

```java
class Solution {
    private int count = 0;
    public int totalNQueens(int n) {
        if (n <= 0) {
            return 0;
        }
        
        List<Integer> col = new ArrayList<>();
        
        dfs(n, col);
        return count;
    }
    
    private void dfs(int n, List<Integer> col) {
        if (col.size() == n) {
            count++;
            return;
        }
        
        for (int colIndex = 0; colIndex < n; ++colIndex) {
            if (isValid(colIndex, col)) {
                col.add(colIndex);
                dfs(n, col);
                col.remove(col.size() - 1);
            }
        }
    }
    
    private boolean isValid(int colIndex, List<Integer> col) {
        for (int rowIndex = 0; rowIndex < col.size(); ++rowIndex) {
            if (col.get(rowIndex) == colIndex) return false;
            if (rowIndex - col.size() == col.get(rowIndex) - colIndex) return false;
            if (rowIndex + col.get(rowIndex) == col.size() + colIndex) return false;
        }
        
        return true;
    }
}
```
