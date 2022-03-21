---
description: LeetCode 106
---

# Construct Binary Tree from Inorder and Postorder Traversal

{% embed url="https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/" %}

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

**Constraints:**

* `1 <= inorder.length <= 3000`
* `postorder.length == inorder.length`
* `-3000 <= inorder[i], postorder[i] <= 3000`
* `inorder` and `postorder` consist of **unique** values.
* Each value of `postorder` also appears in `inorder`.
* `inorder` is **guaranteed** to be the inorder traversal of the tree.
* `postorder` is **guaranteed** to be the postorder traversal of the tree.

{% hint style="info" %}
思路同[Construct Binary Tree from Preorder and Postorder Traversal](construct-binary-tree-from-preorder-and-postorder-traversal.md)

比较简洁的办法是找到以当前节点为根的右子树节点的数量。
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
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int N = inorder.length;
        if (N == 0) {
            return null;
        }
        
        TreeNode root = new TreeNode(postorder[N - 1]);
        if (N == 1) {
            return root;
        }
        
        int rootIndex = 0;
        for (int i = 0; i < N; ++i) {
            if (inorder[i] == postorder[N - 1]) {
                rootIndex = i;
            }
        }
        
        root.left = buildTree(Arrays.copyOfRange(inorder, 0, rootIndex),
                             Arrays.copyOfRange(postorder, 0, rootIndex));
        root.right = buildTree(Arrays.copyOfRange(inorder, rootIndex + 1, N),
                              Arrays.copyOfRange(postorder, rootIndex, N - 1));
        return root;
    }
}
```
