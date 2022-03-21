---
description: LeetCode 105
---

# Construct Binary Tree from Preorder and Inorder Traversal

{% embed url="https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/" %}



Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

**Constraints:**

* `1 <= preorder.length <= 3000`
* `inorder.length == preorder.length`
* `-3000 <= preorder[i], inorder[i] <= 3000`
* `preorder` and `inorder` consist of **unique** values.
* Each value of `inorder` also appears in `preorder`.
* `preorder` is **guaranteed** to be the preorder traversal of the tree.
* `inorder` is **guaranteed** to be the inorder traversal of the tree.

{% hint style="info" %}
从递归的定义出发。
{% endhint %}

![](<../.gitbook/assets/image (18).png>)

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int N = preorder.length;
        if (N == 0) return null;
        TreeNode root = new TreeNode(preorder[0]);
        if (N == 1) {
            return root;
        }
        
        int rootIndex = 0;
        for (int i = 0; i < N; ++i) {
            if (preorder[0] == inorder[i]) {
                rootIndex = i;
                break;
            }
        }
        //int rightChildren = rootIndex - 1;
        root.left = buildTree(Arrays.copyOfRange(preorder, 1, 1 + rootIndex),
                             Arrays.copyOfRange(inorder, 0, rootIndex));
        root.right = buildTree(Arrays.copyOfRange(preorder, rootIndex + 1, N),
                              Arrays.copyOfRange(inorder, rootIndex + 1, N));
        return root;
    }
}
```
