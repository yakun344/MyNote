---
description: LeetCode 814
---

# Binary Tree Pruning

{% embed url="https://leetcode.com/problems/binary-tree-pruning" %}

Given the `root` of a binary tree, return _the same tree where every subtree (of the given tree) not containing a_ `1` _has been removed_.

A subtree of a node `node` is `node` plus every node that is a descendant of `node`.

&#x20;

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028\_2.png)

```
Input: root = [1,null,0,0,1]
Output: [1,null,0,null,1]
Explanation: 
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.
```

**Example 2:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028\_1.png)

```
Input: root = [1,0,1,0,0,0,1]
Output: [1,null,1,null,1]
```

**Example 3:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/05/1028.png)

```
Input: root = [1,1,0,1,1,0,1,0]
Output: [1,1,0,1,1,null,1]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 200]`.
* `Node.val` is either `0` or `1`.

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
    class ResultType{
        boolean isValid;
        TreeNode node;
        public ResultType(TreeNode node, boolean isValid) {
            this.node = node;
            this.isValid = isValid;
        }
    }
    public TreeNode pruneTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        if (!dc(root).isValid) {
            return null;
        }
        
        return root;
    }
    private ResultType dc(TreeNode node) {
        if (node == null) {
            return new ResultType(null, false);
        }
        
        ResultType left = dc(node.left);
        ResultType right = dc(node.right);
        
        ResultType res = new ResultType(node, false);
        if (left.isValid || right.isValid || node.val == 1) {
            res.isValid = true;
        }
        
        if (!left.isValid) {
            node.left = null;
        }
        if (!right.isValid) {
            node.right = null;
        }
        
        return res;
    }
}
```
