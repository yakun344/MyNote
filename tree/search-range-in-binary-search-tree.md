---
description: LintCode 11
---

# Search Range in Binary Search Tree

{% embed url="https://www.lintcode.com/problem/search-range-in-binary-search-tree/" %}

#### Description

Given a binary search tree and a range `[k1, k2]`, return node values within a given range in ascending order.Have you met this question in a real interview?  YesProblem Correction

#### Example

**Example 1:**

```
Input：{5},6,10
Output：[]
        5
it will be serialized {5}
No number between 6 and 10
```

**Example 2:**

```
Input：{20,8,22,4,12},10,22
Output：[12,20,22]
Explanation：
        20
       /  \
      8   22
     / \
    4   12
it will be serialized {20,8,22,4,12}
[12,20,22] between 10 and 22
```

#### 解题思路

这题考查的是二叉查找树的性质，以及二叉树的中序遍历。

二叉查找树满足左子树所有节点的值都小于当前节点的值，右子树所有节点的值都大于当前节点的值。二叉查找树的中序遍历是一个排好序的序列。

这题我们在中序遍历的过程中将在数值范围内的值按序加入到数组中，就能得到最终的结果。

#### 代码思路

二叉树中序遍历：

1. 如果当前节点为空，直接返回。
2. 先遍历左节点。
3. 判断当前节点的值是否在范围内，如果是，加入结果数组中。
4. 再遍历右节点。

这题有一个可以剪枝的技巧，如果已经可以确定左子树或右子树不在数值范围内，可以不遍历相应的子树。

#### 复杂度分析

设二叉树的节点数为`N`。

#### 时间复杂度

* 遍历一遍二叉树的时间复杂度为`O(N)`。

#### 空间复杂度

* 递归的空间开销取决于树的最大深度，空间复杂度为`O(N)`。

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
     * @param root: param root: The root of the binary search tree
     * @param k1: An integer
     * @param k2: An integer
     * @return: return: Return all keys that k1<=key<=k2 in ascending order
     */
    public List<Integer> searchRange(TreeNode root, int k1, int k2) {
        // write your code here
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        dfs(root, k1, k2, result);
        return result;
    }

    private void dfs(TreeNode root, int k1, int k2, List<Integer> result) {
        if (root == null) return;

        if (root.val > k1) {
            dfs(root.left, k1, k2, result);
        }

        if (k1 <= root.val && root.val <= k2) {
            result.add(root.val);
        }

        if (root.val < k2) {
            dfs(root.right, k1, k2, result);
        }
    }
}
```
