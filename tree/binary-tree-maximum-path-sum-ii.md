---
description: LintCode 475
---

# Binary Tree Maximum Path Sum II

{% embed url="https://www.lintcode.com/problem/475/" %}



Description

Given a binary tree, find the maximum path sum from root.

The path may end at any node in the tree and contain at least one node (that is,the root node) in it.Example

**Example 1:**

```
Input: {1,2,3}
Output: 4
Explanation:
    1
   / \
  2   3
1+3=4
```

**Example 2:**

```
Input: {1,-1,-1}
Output: 1
Explanation:
    1
   / \
  -1 -1
```

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param root: the root of binary tree.
     * @return: An integer
     */
    public int maxPathSum2(TreeNode root) {
        // write your code here
        if (root == null) {
            return Integer.MIN_VALUE;
        }

        int left = maxPathSum2(root.left);
        int right = maxPathSum2(root.right);
        return root.val + Math.max(0, Math.max(left, right));
    }
}
```
