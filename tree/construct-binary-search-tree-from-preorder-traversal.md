---
description: LeetCode 1008
---

# Construct Binary Search Tree from Preorder Traversal

{% embed url="https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/" %}



Given an array of integers preorder, which represents the **preorder traversal** of a BST (i.e., **binary search tree**), construct the tree and return _its root_.

It is **guaranteed** that there is always possible to find a binary search tree with the given requirements for the given test cases.

A **binary search tree** is a binary tree where for every node, any descendant of `Node.left` has a value **strictly less than** `Node.val`, and any descendant of `Node.right` has a value **strictly greater than** `Node.val`.

A **preorder traversal** of a binary tree displays the value of the node first, then traverses `Node.left`, then traverses `Node.right`.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

```
Input: preorder = [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

**Example 2:**

```
Input: preorder = [1,3]
Output: [1,null,3]
```

**Constraints:**

* `1 <= preorder.length <= 100`
* `1 <= preorder[i] <= 108`
* All the values of `preorder` are **unique**.

{% hint style="warning" %}
必背题目，有空完善一下recursion版本。
{% endhint %}

用node和child指针分别记录上一次访问的节点和当前访问的节点。

栈顶如果比child值小，说明之前访问过，所以要pop出去。

比较当前node和child大小并且连接。

把当前访问的child push到栈中。

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
    public TreeNode bstFromPreorder(int[] preorder) {
        int n = preorder.length;
        if (n == 0) {
            return null;
        }
        
        TreeNode root = new TreeNode(preorder[0]);
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        for (int i = 1; i < n; ++i) {
            TreeNode node = stack.peek();
            TreeNode child = new TreeNode(preorder[i]);
            
            //找当前child的父节点
            while (!stack.isEmpty() && stack.peek().val < child.val) {
                node = stack.pop();
            }
            
            if (node.val < child.val) {
                node.right = child;
            } else {
                node.left = child;
            }
            
            stack.push(child);
        }
        return root;
    }
}
```
