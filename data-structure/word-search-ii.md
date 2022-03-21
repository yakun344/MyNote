---
description: LeetCode 212
---

# Word Search II

{% embed url="https://leetcode.com/problems/word-search-ii" %}



Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

```
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

```
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
```

&#x20;

**Constraints:**

* `m == board.length`
* `n == board[i].length`
* `1 <= m, n <= 12`
* `board[i][j]` is a lowercase English letter.
* `1 <= words.length <= 3 * 104`
* `1 <= words[i].length <= 10`
* `words[i]` consists of lowercase English letters.
* All the strings of `words` are unique.

![](<../.gitbook/assets/image (47).png>)

![](<../.gitbook/assets/image (52).png>)

```java
class Solution {
    private int[] rdir = new int[]{0, 1, 0, -1};
    private int[] cdir = new int[]{1, 0, -1, 0};
    public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new ArrayList<>();
        TrieNode root = buildTrie(words);
        for (int i = 0; i < board.length; ++i) {
            for (int j = 0; j < board[0].length; ++j) {
                dfs(board, rdir, cdir, i, j, root, res);
            }
        }
        
        return res;
    }
    
    private void dfs(char[][] board,
                    int[] rdir,
                    int[] cdir,
                    int i,
                    int j,
                    TrieNode root,
                    List<String> res) {
        char c = board[i][j];
        if (root.next[c - 'a'] == null) {
            return;
        }
        
        root = root.next[c - 'a'];
        if (root.word != null) {    //找到这个单词了
            res.add(root.word);
            root.word = null;   //删除，不用再找当前这个了
        }                        //找到之后继续往下搜，否则会漏掉解
        
        board[i][j] = '#';
        
        for (int idx = 0; idx < 4; ++idx) {
            int row = rdir[idx] + i;
            int col = cdir[idx] + j;
            
            if (row < 0 || row >= board.length ||
               col < 0 || col >= board[0].length ||
               board[row][col] == '#') {
                continue;
            }
            
            dfs(board, rdir, cdir, row, col, root, res);
            
        }
        
        board[i][j] = c;
    }
    
    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String str : words) {
            TrieNode p = root;
            for (char c : str.toCharArray()) {
                if (p.next[c - 'a'] == null) {
                    p.next[c - 'a'] = new TrieNode();
                }
                p = p.next[c - 'a'];
            }
            
            p.word = str;
        }
        
        return root;
    }
    
    private class TrieNode{
        TrieNode[] next = new TrieNode[26];
        String word;
    }
}
```
