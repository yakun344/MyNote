---
description: LeetCode 124
---

# Binary Tree Maximum Path Sum

{% embed url="https://leetcode.com/problems/binary-tree-maximum-path-sum/" %}

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any path_.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 3 * 104]`.
* `-1000 <= Node.val <= 1000`

{% hint style="info" %}
分治法:

递归函数定义，找到以当前节点为根的，并且包含当前根结点，左右子树里路径最大的值。

max全局变量，更新不同node节点得出的最大值。sum记录当前节点可能的最大和。返回值为sum。

**思路**： 有4种情况要考虑 left + root & right + root & root & left + right + root 不断往上传递的值 只可能是 1， 2， 3中的一种 第四种情况只可能保存在 max里面 而不可能在 sum里。
{% endhint %}

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
    private int max;
    public int maxPathSum(TreeNode root) {
        if (root == null) {
            return 0;
        }
        max = Integer.MIN_VALUE;
        helper(root);
        return max;
    }
    private int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int left = helper(root.left);
        int right = helper(root.right);
        
        //因为是从下往上单向传值，所以sum不能同时向上传递root,l,r
        //但是比较大小的时候，需要考虑当前root为转折点的情况。
        int sum = Math.max(root.val, Math.max(left + root.val, right + root.val));
        max = Math.max(max, Math.max(sum, left + right + root.val));
        return sum;
    }
}
```
