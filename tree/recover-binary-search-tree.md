---
description: LeetCode 99
---

# Recover Binary Search Tree

{% embed url="https://leetcode.com/problems/recover-binary-search-tree" %}

You are given the `root` of a binary search tree (BST), where the values of **exactly** two nodes of the tree were swapped by mistake. _Recover the tree without changing its structure_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg)

```
Input: root = [1,3,null,null,2]
Output: [3,1,null,null,2]
Explanation: 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

```
Input: root = [3,1,4,null,null,2]
Output: [2,1,4,null,null,3]
Explanation: 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[2, 1000]`.
* `-231 <= Node.val <= 231 - 1`

&#x20;

**Follow up:** A solution using `O(n)` space is pretty straight-forward. Could you devise a constant `O(1)` space solution?

{% hint style="info" %}
二叉树中序遍历，找第一次降序的 last(第一次找到可能root就是第二个节点，所以找到第一次降序的last同时要记录root)，和第二次降序的root。
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
    public TreeNode node1, node2, last;
    public void recoverTree(TreeNode root) {
        if (root == null) {
            return;
        }
        
        traverse(root);
        reverse(node1, node2);
        return;
    }
    public void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        
        traverse(root.left);
        if (last != null && last.val > root.val) {
            node2 = root;
            if (node1 == null) {
                node1 = last;
            } else {
                return;
            }
        }
        
        last = root;
        traverse(root.right);
    }
    public void reverse(TreeNode node1, TreeNode node2) {
        int temp = node1.val;
        node1.val = node2.val;
        node2.val = temp;
    }
}
```
