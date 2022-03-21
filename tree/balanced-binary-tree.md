---
description: LeetCode 110
---

# Balanced Binary Tree

{% embed url="https://leetcode.com/problems/balanced-binary-tree/" %}

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of _every_ node differ in height by no more than 1.

**Example 1:**

Given the following tree `[3,9,20,null,null,15,7]`:

```
    3
   / \
  9  20
    /  \
   15   7
```

Return true.\
\
**Example 2:**

Given the following tree `[1,2,2,3,3,null,null,4,4]`:

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

Return false.

#### Divide and Conquer Approach 1:

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
    
    //Make your code readable
    //define a constant variable -1, stand for not balanced;
    private int NOT_BALANCED = -1;
    public boolean isBalanced(TreeNode root) {
        return maxDepth(root) != NOT_BALANCED;
    }
    
    //when the tree is balanced, return the depth
    //when the tree is not balanced, return NOT_BALANCED
    private int maxDepth(TreeNode root) {
        
        //when root is null, it's balanced, return the depth
        if (root == null) {
            return 0;
        }
        
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        
        if (left == NOT_BALANCED || right == NOT_BALANCED) {
            return NOT_BALANCED;
        }
        
        if (Math.abs(left - right) > 1) {
            return NOT_BALANCED;
        }
        
        return Math.max(left, right) + 1;
    }
}
```

#### Divide and Conquer Approach 2:

* 左子树为平衡二叉树，右子树也为平衡二叉树
* 左右子树的高度差<=1

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
    class ResultType {
        public int depth;
        public boolean isBalanced;
        
        public ResultType(int depth, boolean isBalanced) {
            this.depth = depth;
            this.isBalanced = isBalanced;
        }
    }
    
    public boolean isBalanced(TreeNode root) {
        return maxDepth(root).isBalanced;
    }
    
    private ResultType maxDepth(TreeNode root) {
        if (root == null) {
            return new ResultType(0, true);
        }
        
        ResultType left = maxDepth(root.left);
        ResultType right = maxDepth(root.right);
        
        if (!left.isBalanced || !right.isBalanced) {
            return new ResultType(-1, false);
        }
        
        if (Math.abs(left.depth - right.depth) > 1) {
            return new ResultType(-1, false);
        }
        
        return new ResultType(Math.max(left.depth, right.depth) + 1, true);
    }
}
```

* When the return type is differ from what we want, we can pack a ResultType class.
* If we don't have enough time to do so, we can do approach 1.
* The code is much more readable and clear.
