---
description: LintCode 480 LeetCode 257
---

# Binary Tree Paths

{% embed url="https://www.lintcode.com/problem/binary-tree-paths/description" %}

{% embed url="https://leetcode.com/problems/binary-tree-paths/" %}

```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```

```
Example:
Note: A leaf is a node with no children.
Given a binary tree, return all root-to-leaf paths.
/**
```

{% hint style="info" %}
每次递归的时候，都会new一个新的List\<String>，然后把左右两边List里的String和当前的node.val拼接起来。判断是不是叶子结点，list == null。
{% endhint %}

```java
 /* Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param root: the root of the binary tree
     * @return: all root-to-leaf paths
     */
    public List<String> binaryTreePaths(TreeNode root) {
        // write your code here
        
        List<String> paths = new ArrayList<>();
        if (root == null) {
            return paths;
        }
        
        List<String> leftPaths = binaryTreePaths(root.left);
        List<String> rightPaths = binaryTreePaths(root.right);
        
        for (String path : leftPaths) {
            paths.add(root.val + "->" + path);
        }
        
        for (String path : rightPaths) {
            paths.add(root.val + "->" + path);
        }
        
        if (paths.size() == 0) {
            paths.add("" + root.val);
        }
        
        return paths;
    }
}
```
