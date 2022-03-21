---
description: '298'
---

# Binary Tree Longest Consecutive Sequence

{% embed url="https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/" %}



Given the `root` of a binary tree, return _the length of the longest consecutive sequence path_.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path needs to be from parent to child (cannot be the reverse).

**Example 1:**![](https://assets.leetcode.com/uploads/2021/03/14/consec1-1-tree.jpg)

```
Input: root = [1,null,3,2,4,null,null,null,5]
Output: 3
Explanation: Longest consecutive sequence path is 3-4-5, so return 3.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/03/14/consec1-2-tree.jpg)

```
Input: root = [2,null,3,2,null,1]
Output: 2
Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 3 * 104]`.
* `-3 * 104 <= Node.val <= 3 * 104`

#### Traverse & Divide Conquer

{% hint style="info" %}
判断parent.val + 1 = root.val，如果是的话当前root就满足条件。

helper函数里传入node，parentNode，经过当前节点，且除当前节点以外最长的满足条件的长度。

返回值为longest consecutive length。
{% endhint %}

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
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        return helper(root, null, 0);
    }
    
    private int helper(TreeNode root, TreeNode parent, int lenWithoutRoot) {
        if (root == null) {
            return 0;
        }
        
        int len = (parent != null && parent.val + 1 == root.val) ? lenWithoutRoot + 1 : 1;
        parent = root;
        
        int left = helper(root.left, root, len);
        int right = helper(root.right, root, len);
        return Math.max(len, Math.max(left, right));
    }
}
```

{% hint style="info" %}
全局记一个longest，在divide conquer函数里，返回值为经过当前节点最长的满足条件的长度，sub来记录当前的长度，至少是1。

分治，然后更新当前sub的值，最后和全局longest比较。
{% endhint %}

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
    private int longest;
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        longest = 0;
        divideConquer(root);
        return longest;
    }
    
    private int divideConquer(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int sub = 1;
        
        int left = divideConquer(root.left);
        int right = divideConquer(root.right);
        
        if (left != 0 && root.val + 1 == root.left.val) {
            sub = Math.max(sub, left + 1);
        }
        if (right != 0 && root.val + 1 == root.right.val) {
            sub = Math.max(sub, right + 1);
        }
        
        if (sub > longest) {
            longest = sub;
        }
        
        return sub;
    }
}
```
