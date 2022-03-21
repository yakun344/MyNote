---
description: LeetCode 236
---

# Lowest Common Ancestor of a Binary Tree

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/" %}

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest\_common\_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

Given the following binary tree:  root = \[3,5,1,6,2,0,8,null,null,7,4]![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Example 1:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Note:**

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the binary tree.

{% hint style="info" %}
A,B 都在以root为根都二叉树里，return LCA(A,B)

如果A,B 都不在root为根都二叉树里，return null

如果只有A在，return A

如果只有B在，returnB
{% endhint %}

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
        //当找到这个公共祖先，返回这个root
        if (root == null || root == p || root == q) {
            return root;
        }
        
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        //如果找到了公共祖先，在根的两端，返回这个根
        if (left != null && right != null) {
            return root;
        }
        
        //如果在左边找到了，右边没找到，说明pq都在左端，返回这个左端为根的node
        if (left != null) {
            return left;
        }
        
        //verse vice
        if (right != null) {
            return right;
        }
        
        return null;
    }
}
```
