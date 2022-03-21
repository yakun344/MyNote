---
description: LeetCode 1676
---

# Lowest Common Ancestor of a Binary Tree IV

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv/" %}

Given the `root` of a binary tree and an array of `TreeNode` objects `nodes`, return _the lowest common ancestor (LCA) of **all the nodes** in_ `nodes`. All the nodes will exist in the tree, and all values of the tree's nodes are **unique**.

Extending the [**definition of LCA on Wikipedia**](https://en.wikipedia.org/wiki/Lowest\_common\_ancestor): "The lowest common ancestor of `n` nodes `p1`, `p2`, ..., `pn` in a binary tree `T` is the lowest node that has every `pi` as a **descendant** (where we allow **a node to be a descendant of itself**) for every valid `i`". A **descendant** of a node `x` is a node `y` that is on the path from node `x` to some leaf node.

**Example 1:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [4,7]
Output: 2
Explanation: The lowest common ancestor of nodes 4 and 7 is node 2.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [1]
Output: 1
Explanation: The lowest common ancestor of a single node is the node itself.

```

**Example 3:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [7,6,2,4]
Output: 5
Explanation: The lowest common ancestor of the nodes 7, 6, 2, and 4 is node 5.
```

**Example 4:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [0,1,2,3,4,5,6,7,8]
Output: 3
Explanation: The lowest common ancestor of all the nodes is the root node.
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 104]`.
* `-109 <= Node.val <= 109`
* All `Node.val` are **unique**.
* All `nodes[i]` will exist in the tree.
* All `nodes[i]` are distinct.

{% hint style="info" %}
把nodes数组存到hash set里，分治。

如果当前节点在set，返回当前节点。

如果左右两边都有node在set里，返回当前节点；

如果只有左边有，返回左边；如果只有右边有，返回右边；

如果没找到（子树中和自身都不在set里），返回空。
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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        Set<TreeNode> set = new HashSet<>();
        for (TreeNode node : nodes) {
            set.add(node);
        }
        TreeNode res = divideConquer(root, set);
        return res;
    }
    
    private TreeNode divideConquer(TreeNode node, Set<TreeNode> set) {
        if (node == null) {
            return null;
        }
        
        TreeNode left = divideConquer(node.left, set);
        TreeNode right = divideConquer(node.right, set);
        
        if (set.contains(node)) {
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
