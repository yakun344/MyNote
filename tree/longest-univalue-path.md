---
description: LeetCode 687
---

# Longest Univalue Path

{% embed url="https://leetcode.com/problems/longest-univalue-path/" %}

Given the `root` of a binary tree, return _the length of the longest path, where each node in the path has the same value_. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```
Input: root = [5,4,5,1,1,5]
Output: 2
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

```
Input: root = [1,4,5,4,4,5]
Output: 2
```

**Constraints:**

* The number of nodes in the tree is in the range `[0, 104]`.
* `-1000 <= Node.val <= 1000`
* The depth of the tree will not exceed `1000`.

```java
class Solution {
    int result;
    public int longestUnivaluePath(TreeNode root) {
        if (root == null) return 0;
        result = 0;
        dfs(root);
        return result;
    }
    
    //和当前node值一样的最长的单边值
    //用的时候（求result）可以两边都用
    private int dfs(TreeNode node) {
        if (node == null) {
            return 0;
        }
        
        int left = dfs(node.left);
        int right = dfs(node.right);
        
        int pl = 0;
        int pr = 0;
        if (node.left != null && node.val == node.left.val) {
            pl = left + 1;
        }
        if (node.right != null && node.val == node.right.val) {
            pr = right + 1;
        }
        result = Math.max(result, pl + pr);
        return Math.max(pl, pr);
    }
}
```
