---
description: LeetCode 549
---

# Binary Tree Longest Consecutive Sequence II

{% embed url="https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/" %}



Given the `root` of a binary tree, return _the length of the longest consecutive path in the tree_.

This path can be either increasing or decreasing.

* For example, `[1,2,3,4]` and `[4,3,2,1]` are both considered valid, but the path `[1,2,4,3]` is not valid.

On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/03/14/consec2-1-tree.jpg)

```
Input: root = [1,2,3]
Output: 2
Explanation: The longest consecutive path is [1, 2] or [2, 1].
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/03/14/consec2-2-tree.jpg)

```
Input: root = [2,1,3]
Output: 3
Explanation: The longest consecutive path is [1, 2, 3] or [3, 2, 1].
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 3 * 104]`.
* `-3 * 104 <= Node.val <= 3 * 104`

{% hint style="danger" %}
找从任意点出发到任意点最长到consecutive path，可以用Divide & Conquer来做。每个节点的返回值需要用ResultType包装，包含当前节点下最长的consecutive path，最长的升序序列，最长的降序序列。把up和down拼接到一起，再和左右子树最长的maxLen比，找到最大的maxLen就是最长的consecutive path。

具体divideConquer函数里要找到当前root的up和down。

找到left和right的up并且对比大小，找到一个最大的up；

找到left和right的down并且对比大小，找一个最小的down；

当前的res.maxLen就是up + down + 1。

注意⚠️这里up + down + 1一定能拼成一个当前最大的consecutive path，然后再和左右子树的maxLen比较，找到一个当前root为根全局最大的maxLen。
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
    public class ResultType {
        int maxLen;
        int up;
        int down;
        public ResultType(int maxLen, int up, int down) {
            this.maxLen = maxLen;
            this.up = up;
            this.down = down;
        }
    }
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return divideConquer(root).maxLen;
    }
    private ResultType divideConquer(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0, 0);
        }
        
        ResultType left = divideConquer(root.left);
        ResultType right = divideConquer(root.right);
        
        ResultType res = new ResultType(0, 0, 0);
        if (root.left != null && root.left.val + 1 == root.val) {
            res.down = Math.max(res.down, left.down + 1);
        }
        if (root.left != null && root.left.val - 1 == root.val) {
            res.up = Math.max(res.up, left.up + 1);
        }
        if (root.right != null && root.right.val + 1 == root.val) {
            res.down = Math.max(res.down, right.down + 1);
        }
        if (root.right != null && root.right.val - 1 == root.val) {
            res.up = Math.max(res.up, right.up + 1);
        }
        
        res.maxLen = res.up + res.down + 1;
        res.maxLen = Math.max(res.maxLen, Math.max(left.maxLen, right.maxLen));
        return res;
    }
}
```
