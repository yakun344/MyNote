---
description: LeetCode 1644
---

# Lowest Common Ancestor of a Binary Tree II

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii/" %}

Given the `root` of a binary tree, return _the lowest common ancestor (LCA) of two given nodes,_ `p` _and_ `q`. If either node `p` or `q` **does not exist** in the tree, return `null`. All values of the nodes in the tree are **unique**.

According to the [**definition of LCA on Wikipedia**](https://en.wikipedia.org/wiki/Lowest\_common\_ancestor): "The lowest common ancestor of two nodes `p` and `q` in a binary tree `T` is the lowest node that has both `p` and `q` as **descendants** (where we allow **a node to be a descendant of itself**)". A **descendant** of a node `x` is a node `y` that is on the path from node `x` to some leaf node.

**Example 1:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5. A node can be a descendant of itself according to the definition of LCA.
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 10
Output: null
Explanation: Node 10 does not exist in the tree, so return null.
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 104]`.
* `-109 <= Node.val <= 109`
* All `Node.val` are **unique**.
* `p != q`

&#x20;**Follow up:** Can you find the LCA traversing the tree, without checking nodes existence?

{% hint style="info" %}
TreeNode p和q可能有不存在的情况，所以我们可以用两个全局变量来记录找到p和q的情况。

1.用全局变量findP和findQ来记录找到p和q的情况。在mian function中查看是不是两个都找到，并返回找到的结果。

2.Divide and conquer，如果node是要找的，就更新全局变量，**并且返回当前的node**。

3.当node为A或者B的时候，继续寻找下一个，直到根节点。

注意⚠️：和[Lowest Common Ancestor of a Binary Tree I](lowest-common-ancestor-of-a-binary-tree.md)不同的是，要用两个全局变量记录找到p和q的情况。
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
    private boolean findP = false, findQ = false;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode res = divideConquer(root, p, q);
        if (findP && findQ) {
            return res;
        } else {
            return null;
        }
    }
    
    private TreeNode divideConquer(TreeNode node, TreeNode p, TreeNode q) {
        if (node == null) {
            return node;
        }
        
        TreeNode left = divideConquer(node.left, p, q);
        TreeNode right = divideConquer(node.right, p, q);
        
        if (node == p || node == q) {
            findP = (node == p) || findP;
            findQ = (node == q) || findQ;
            return node;
        }
        
        if (left != null && right != null) {
            return node;
        } else if (left != null) {
            return left;
        } else if (right != null) {
            return right;
        } else {
            return null;
        }
    }
}
```
