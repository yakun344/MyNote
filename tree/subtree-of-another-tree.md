---
description: LeetCode 572
---

# Subtree of Another Tree

{% embed url="https://leetcode.com/problems/subtree-of-another-tree/" %}



Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg)

```
Input: root = [3,4,5,1,2,null,null,0], subRoot = [4,1,2]
Output: false
```

**Constraints:**

* The number of nodes in the `root` tree is in the range `[1, 2000]`.
* The number of nodes in the `subRoot` tree is in the range `[1, 1000]`.
* `-104 <= root.val <= 104`
* `-104 <= subRoot.val <= 104`

{% hint style="info" %}
可以AC的版本，在tree上traverse每个节点的时候都要调用一次isSameTree。即使有node的值是重复的也可以判断是不是和子树一样。

不可以AC的版本是默认在tree上node的val是unique的。
{% endhint %}

#### BFS Approach

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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        Deque<TreeNode> queue = new LinkedList<>();
        queue.addLast(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.removeFirst();
            if (isSameTree(node, subRoot)) {
                return true;
            }
            
            if (node.left != null) {
                queue.addLast(node.left);
            }
            if (node.right != null) {
                queue.addLast(node.right);
            }
        }
        
        return false;
    }
    
    private boolean isSameTree(TreeNode node1, TreeNode node2) {
        if (node1 == null && node2 == null) {
            return true;
        }
        
        if (node1 == null || node2 == null) {
            return false;
        }
        
        if (node1.val != node2.val) {
            return false;
        }
        
        return isSameTree(node1.left, node2.left) && isSameTree(node1.right, node2.right);
    }
}
//AC
```

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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        Deque<TreeNode> queue = new LinkedList<>();
        queue.addLast(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.removeFirst();
            if (node.val == subRoot.val) {
                return isSameTree(node, subRoot);
            }
            
            if (node.left != null) {
                queue.addLast(node.left);
            }
            if (node.right != null) {
                queue.addLast(node.right);
            }
        }
        
        return false;
    }
    
    private boolean isSameTree(TreeNode node1, TreeNode node2) {
        if (node1 == null && node2 == null) {
            return true;
        }
        
        if (node1 == null || node2 == null) {
            return false;
        }
        
        if (node1.val != node2.val) {
            return false;
        }
        
        return isSameTree(node1.left, node2.left) && isSameTree(node1.right, node2.right);
    }
}
//Not AC
```

#### DFS Approach

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
    private boolean IS_SUBTREE;
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        
        helper(root, subRoot);
        return IS_SUBTREE;
    }
    private void helper(TreeNode root, TreeNode subRoot) {
        if (root == null) {
            return;
        }
        
        if (isSameTree(root, subRoot)) {
            IS_SUBTREE = true;
            return;
        }
        
        helper(root.left, subRoot);
        helper(root.right, subRoot);
    }
    private boolean isSameTree(TreeNode node1, TreeNode node2) {
        if (node1 == null && node2 == null) {
            return true;
        }
        
        if (node1 == null || node2 == null) {
            return false;
        }
        
        if (node1.val != node2.val) {
            return false;
        }
        
        return isSameTree(node1.left, node2.left) && isSameTree(node1.right, node2.right);
    }
}
```
