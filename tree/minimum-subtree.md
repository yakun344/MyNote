---
description: LintCode 596
---

# Minimum Subtree

{% embed url="https://www.lintcode.com/problem/minimum-subtree/description" %}

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

Example 1:

```
Input:
{1,-5,2,1,2,-4,-5}
Output:1
Explanation:
The tree is look like this:
     1
   /   \
 -5     2
 / \   /  \
1   2 -4  -5 
The sum of whole tree is minimum, so return the root.
```

Example 2:

```
Input:
{1}
Output:1
Explanation:
The tree is look like this:
   1
There is one and only one subtree in the tree. So we return 1.
```

```java
/**
 * Definition of TreeNode:
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
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */

    public int minSum = 0;
    public TreeNode subtree = null;
    public TreeNode findSubtree(TreeNode root) {
        // write your code here
        dfs(root);
        return subtree;
    }

    public int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = dfs(root.left);
        int right = dfs(root.right);

        int res = left + right + root.val;
        
        if (subtree == null || minSum > res) {
            subtree = root;
            minSum = res;
            return minSum;
        }

        return res;
    }
}
```
