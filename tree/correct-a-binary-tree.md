---
description: LeetCode 1660
---

# Correct a Binary Tree

{% embed url="https://leetcode.com/problems/correct-a-binary-tree/" %}

You have a binary tree with a small defect. There is **exactly one** invalid node where its right child incorrectly points to another node at the **same depth** but to the **invalid node's right**.

Given the root of the binary tree with this defect, `root`, return _the root of the binary tree after **removing** this invalid node **and every node underneath it** (minus the node it incorrectly points to)._

**Custom testing:**

The test input is read as 3 lines:

* `TreeNode root`
* `int fromNode` (**not available to** `correctBinaryTree`)
* `int toNode` (**not available to** `correctBinaryTree`)

After the binary tree rooted at `root` is parsed, the `TreeNode` with value of `fromNode` will have its right child pointer pointing to the `TreeNode` with a value of `toNode`. Then, `root` is passed to `correctBinaryTree`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/22/ex1v2.png)

```
Input: root = [1,2,3], fromNode = 2, toNode = 3
Output: [1,null,3]
Explanation: The node with value 2 is invalid, so remove it.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/22/ex2v3.png)

```
Input: root = [8,3,1,7,null,9,4,2,null,null,null,5,6], fromNode = 7, toNode = 4
Output: [8,3,1,null,null,9,4,null,null,5,6]
Explanation: The node with value 7 is invalid, so remove it and the node underneath it, node 2.
```

**Constraints:**

* The number of nodes in the tree is in the range `[3, 104]`.
* `-109 <= Node.val <= 109`
* All `Node.val` are **unique**.
* `fromNode != toNode`
* `fromNode` and `toNode` will exist in the tree and will be on the same depth.
* `toNode` is to the **right** of `fromNode`.
* `fromNode.right` is `null` in the initial tree from the test data.

{% hint style="info" %}
有一个node的右指针指向了错误的node，用hashset记录节点，如果有重复的，删除即可。
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
    private Set<Integer> set = new HashSet<>();
    public TreeNode correctBinaryTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        if (root.right != null && set.contains(root.right.val)) {
            return null;
        }
        
        set.add(root.val);
        root.right = correctBinaryTree(root.right);
        root.left = correctBinaryTree(root.left);
        return root;
    }
}
```
