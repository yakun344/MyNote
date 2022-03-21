---
description: LeetCode 104
---

# Maximum Depth of Binary Tree

{% embed url="https://leetcode.com/problems/maximum-depth-of-binary-tree/" %}

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Note:** A leaf is a node with no children.

**Example:**

Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

#### Traversal Approach

* 走一遍所有的节点
* 走到每一个节点的时候，记录每一个节点的信息
* 最后得到全局汇总的信息

{% hint style="info" %}
遍历时记录当前节点为根的最大深度
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

    //计一个最大深度
    private int depth;
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        //初始最大深度
        depth = 0;
        
        //走遍所有的节点，记录下每个节点的深度，和全局的最大深度比
        //当前的深度是1
        traverse(root, 1);
        return depth;
    }
    
    //当前的节点，当前的深度
    private void traverse(TreeNode node, int currDepth) {
    
        if (node == null) {
            return;
        }
        
        depth = Math.max(depth, currDepth);
        traverse(node.left, currDepth + 1);
        traverse(node.right, currDepth + 1);
    }
}
```

#### Divide and Conquer Approach

* 左子树的深度，右子树的深度
* 合并结果

{% hint style="info" %}
左子树和右子树之间的关系是整个树的深度是左右子树的最大深度+1
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
    
    //分治法需要返回值
    public int maxDepth(TreeNode root) {
        
        //处理异常情况
        if (root == null) {
            return 0;
        }
        
        //左右子树的最大深度
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        
        //全局的最大深度，合并
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```
