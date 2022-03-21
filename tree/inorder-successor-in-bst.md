---
description: LeetCode 285
---

# Inorder Successor in BST

{% embed url="https://leetcode.com/problems/inorder-successor-in-bst/" %}



Given the `root` of a binary search tree and a node `p` in it, return _the in-order successor of that node in the BST_. If the given node has no in-order successor in the tree, return `null`.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_1.PNG)

```
Input: root = [2,1,3], p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_2.PNG)

```
Input: root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation: There is no in-order successor of the current node, so the answer is null.
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 104]`.
* `-105 <= Node.val <= 105`
* All Nodes will have unique values.

{% hint style="info" %}
首先要确定中序遍历的后继:

* 如果该节点有右子节点, 那么后继是其右子节点的子树中最左端的节点
* 如果该节点没有右子节点, 那么后继是离它最近的祖先, 该节点在这个祖先的左子树内.

使用循环实现:

* 查找该节点, 并在该过程中维护上述性质的祖先节点
* 查找到后, 如果该节点有右子节点, 则后继在其右子树内; 否则后继就是维护的那个祖先节点

使用递归实现:

* 如果根节点小于或等于要查找的节点, 直接进入右子树递归
* 如果根节点大于要查找的节点, 则暂存左子树递归查找的结果, 如果是 `null`, 则直接返回当前根节点; 反之返回左子树递归查找的结果.

在递归实现中, 暂存左子树递归查找的结果就相当于循环实现中维护的祖先节点.
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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (root == null || p == null) {
            return null;
        }
        
        if (root.val <= p.val) {
            return inorderSuccessor(root.right, p);
        } else {
            TreeNode left = inorderSuccessor(root.left, p);
            return (left != null) ? left : root;
        }
    }
}
```

```java
public class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode successor = null;
        while (root != null && root.val != p.val) {
            if (root.val > p.val) {
                successor = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }

        if (root == null) {
            return null;
        }

        if (root.right == null) {
            return successor;
        }

        root = root.right;
        while (root.left != null) {
            root = root.left;
        }

        return root;
    }
}
```
