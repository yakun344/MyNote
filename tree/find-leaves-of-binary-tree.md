---
description: LeetCode 366
---

# Find Leaves of Binary Tree

{% embed url="https://leetcode.com/problems/find-leaves-of-binary-tree" %}

Given the `root` of a binary tree, collect a tree's nodes as if you were doing this:

* Collect all the leaf nodes.
* Remove all the leaf nodes.
* Repeat until the tree is empty.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/16/remleaves-tree.jpg)

```
Input: root = [1,2,3,4,5]
Output: [[4,5,3],[2],[1]]
Explanation:
[[3,5,4],[2],[1]] and [[3,4,5],[2],[1]] are also considered correct answers since per each level it does not matter the order on which elements are returned.
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 100]`.
* `-100 <= Node.val <= 100`

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
    public List<List<Integer>> findLeaves(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        helper(root, res);
        
        return res;
    }
    
    private int helper(TreeNode root, List<List<Integer>> res) {
        if (root == null) {
            return -1;
        }
        
        int level = 1 + Math.max(helper(root.left, res), helper(root.right, res));
        if (res.size() < level + 1) {
            res.add(new ArrayList<>());
        }
        
        res.get(level).add(root.val);
        return level;
    }
}
```
