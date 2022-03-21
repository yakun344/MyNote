---
description: LeetCode 111
---

# Minimum Depth of Binary Tree

{% embed url="https://leetcode.com/problems/minimum-depth-of-binary-tree/" %}

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

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

return its minimum depth = 2.

#### Breadth-First Search Approach

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
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int count = 1;
        Deque<TreeNode> queue = new LinkedList<>();
        queue.addLast(root);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                TreeNode node = queue.removeFirst();
                if (node.left == null && node.right == null) {
                    return count;
                }
                if (node.left != null) {
                    queue.addLast(node.left);
                }
                if (node.right != null) {
                    queue.addLast(node.right);
                }
            }
            count++;
        }
        
        return count;
    }
}
```

#### Depth-First Search

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
    
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return dc(root);
    }
    private int dc(TreeNode node) {
        if (node == null) {
            return Integer.MAX_VALUE;
        }

        if (node.left == null && node.right == null) {
            return 1;
        }

        return Math.min(dc(node.left), dc(node.right)) + 1;
    }
}
```

#### Traverse Approach

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
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int left = minDepth(root.left);
        int right = minDepth(root.right);
        
        if (left != 0 && right != 0) {
            return Math.min(left, right) + 1;
        }
        else if (left != 0) {
            return left + 1;
        }
        else {
            return right + 1;
        }
    }
}
```
