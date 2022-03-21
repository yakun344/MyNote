---
description: LeetCode 270 LintCode 900
---

# Closest Binary Search Tree Value

{% embed url="https://leetcode.com/problems/closest-binary-search-tree-value/" %}

{% embed url="https://www.lintcode.com/problem/closest-binary-search-tree-value/description" %}

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

**Note:**

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

**Example:**

```
Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4
```

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
    public int closestValue(TreeNode root, double target) {
        if (root == null) {
            return -1;
        }
        
        TreeNode lower = root;
        TreeNode upper = root;
        
        while (root != null) {
            if (target > root.val) {
                lower = root;
                root = root.right;
            } else if (target < root.val) {
                upper = root;
                root = root.left;
            } else {
                return root.val;
            }
        }
        
        if (Math.abs(upper.val - target) > Math.abs(lower.val - target)) {
            return lower.val;
        } else {
            return upper.val;
        }
    }
}
```

