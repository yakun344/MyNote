---
description: LeetCode 114 & LintCode 453
---

# Flatten Binary Tree to Linked List

{% embed url="https://leetcode.com/problems/flatten-binary-tree-to-linked-list/" %}

{% embed url="https://www.lintcode.com/problem/flatten-binary-tree-to-linked-list/description" %}

Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

```
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:

```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

#### Traverse Approach️

![](<../.gitbook/assets/image (16).png>)

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

    //lastNode用来记录上一次访问的点
    private TreeNode lastNode = null;
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        
        //lastnode不为空的时候，left指向空，right指向当前的根
        if (lastNode != null) {
            lastNode.left = null;
            lastNode.right = root;
        }
        
        //随着traverse更新lastnode
        //right一定要存一下，因为faltten(root.left)之后right指针从lastnode指向了当前root
        lastNode = root;
        TreeNode right = root.right;
        
        //recursively 调用左边和右边
        flatten(root.left);
        flatten(right);
        
        return;
    }
}
```

#### Divide and Conquer Approach

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
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        
        divideConquer(root);
        return;
    }
    
    private TreeNode divideConquer(TreeNode root) {
        if (root == null) {
            return root;
        }
        
        TreeNode leftLast = divideConquer(root.left);
        TreeNode rightLast = divideConquer(root.right);
        
        //把左边的最后一个和右子树的根连接起来
        //根的右边指向根的左边这个node(leftLast)
        if (leftLast != null) {
            leftLast.right = root.right;
            root.right = root.left;
            root.left = null;
        }
        
        if (rightLast != null) {
            return rightLast;
        }
        
        if (leftLast != null) {
            return leftLast;
        }
        
        return root;
    }
}
```

