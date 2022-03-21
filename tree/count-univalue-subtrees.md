---
description: LeetCode 250
---

# Count Univalue Subtrees

{% embed url="https://leetcode.com/problems/count-univalue-subtrees" %}

Given the `root` of a binary tree, return the number of **uni-value** subtrees.

A **uni-value subtree** means all nodes of the subtree have the same value.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/21/unival\_e1.jpg)

```
Input: root = [5,1,5,5,5,null,5]
Output: 4
```

**Example 2:**

```
Input: root = []
Output: 0
```

**Example 3:**

```
Input: root = [5,5,5,5,5,null,5]
Output: 6
```

&#x20;

**Constraints:**

* The number of the node in the tree will be in the range `[0, 1000]`.
* `-1000 <= Node.val <= 1000`

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
    private int count;
    public int countUnivalSubtrees(TreeNode root) {
        if (root == null) {
            return 0;
        }
        dc(root);
        return count;
    }
    private boolean dc(TreeNode node) {
        if (node.left == null && node.right == null) {
            count++;
            return true;
        }
        
        boolean isValid = true;
        if (node.left != null) {
            isValid = dc(node.left) && 
                node.left.val == node.val;
        }
        
        if (node.right != null) {
            isValid = dc(node.right) && isValid &&
                node.right.val == node.val;
        }
        
        if (!isValid) {
            return false;
        }
        
        count++;
        return true;
    }
}
```
