---
description: LeetCode 98
---

# Validate Binary Search Tree

{% embed url="https://leetcode.com/problems/validate-binary-search-tree/" %}

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

**Example 1:**

```
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```

**Example 2:**

```
    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

#### Traverse Approach:

{% hint style="info" %}
利用这个性质，如果二叉树的inorder traverse是升序，它就是BST，反之就不是。所以我们可以用中序遍历，一边遍历一边判断是不是升序。全局使用一个boolean来判断每次遍历的时候lastNode和node的关系是不是升序，如果不是boolean isvalid = false，如果是lastNode指向（curNode)Node。
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

    //只需存上一个node就可以
    private TreeNode lastNode;
    private boolean isValid;
    
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        lastNode = null;
        isValid = true;
        inOrderTraverse(root);
        
        return isValid;
    }
    private void inOrderTraverse(TreeNode root) {
        if (root == null) {
            return;
        }
        
        //首先访问左子树，根和lastNode进行比较
        inOrderTraverse(root.left);
        if (lastNode != null && lastNode.val >= root.val) {
            isValid = false;
            return;
        }
        //更新lastNode
        lastNode = root;
        inOrderTraverse(root.right);
    }
}
```

#### Divide and Conquer Approach:

{% hint style="info" %}
当前根节点的值比左子树最大的值大，并且比右子树最小的值小，以当前节点为根的子树就是BST。

注意⚠️判断当前以node节点为根的子树的时候，要判断左边子树最小节点是不是大于等于node节点，右边子树最大节点是不是小于等于node节点。而**判断当前子树是BST的时候，ResultType要从右边找最大值，从左边找最小值**，如果（左右没找到）没有就是根节点。
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
class ResultType{
    public boolean isBST;
    //如果跟踪左右子树最大小值的话，空节点很难定义，所以可以只跟踪大小node，用null判断
    //public int maxValue, minValue;
    public TreeNode maxNode, minNode;
    public ResultType(boolean isBST) {
        this.isBST = isBST;
        this.maxNode = null;
        this.minNode = null;
    }
}
class Solution {
    public boolean isValidBST(TreeNode root) {
        return divideConquer(root).isBST;
    }
    private ResultType divideConquer(TreeNode root) {
        if (root == null) {
            return new ResultType(true);
        }
        
        ResultType left = divideConquer(root.left);
        ResultType right = divideConquer(root.right);
        
        if (!left.isBST || !right.isBST) {
            return new ResultType(false);
        }
        
        if (left.maxNode != null && left.maxNode.val >= root.val) {
            return new ResultType(false);
        }
        if (right.minNode != null && right.minNode.val <= root.val) {
            return new ResultType(false);
        }
        
        ResultType result = new ResultType(true);
        result.maxNode = right.maxNode != null ? right.maxNode : root;
        result.minNode = left.minNode != null ? left.minNode : root;
        
        return result;
    }
}
```
