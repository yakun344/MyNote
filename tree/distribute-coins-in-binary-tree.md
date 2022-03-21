---
description: LeetCode 979
---

# Distribute Coins in Binary Tree

{% embed url="https://leetcode.com/problems/distribute-coins-in-binary-tree" %}

You are given the `root` of a binary tree with `n` nodes where each `node` in the tree has `node.val` coins. There are `n` coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return _the **minimum** number of moves required to make every node have **exactly** one coin_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)

```
Input: root = [3,0,0]
Output: 2
Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)

```
Input: root = [0,3,0]
Output: 3
Explanation: From the left child of the root, we move two coins to the root [taking two moves]. Then, we move one coin from the root of the tree to the right child.
```

&#x20;

**Constraints:**

* The number of nodes in the tree is `n`.
* `1 <= n <= 100`
* `0 <= Node.val <= n`
* The sum of all `Node.val` is `n`.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int res;
    public int distributeCoins(TreeNode root) {
        res = 0;
        dfs(root);
        
        return res;
    }
    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int left = dfs(root.left);
        int right = dfs(root.right);
        
        //返回值统计总共计算左右应该分出去/拿回来的coin
        //正代表要拿的，负代表要给出去的
        res += Math.abs(left) + Math.abs(right);
        return root.val + left + right - 1;
    }
}
```
