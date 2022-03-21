---
description: LeetCode 543
---

# Diameter of Binary Tree

{% embed url="https://leetcode.com/problems/diameter-of-binary-tree/" %}

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

**Example:**\
Given a binary tree\


```
          1
         / \
        2   3
       / \     
      4   5    
```

Return **3**, which is the length of the path \[4,2,1,3] or \[5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of edges between them.

{% hint style="info" %}
注意：只能有一个转折点
{% endhint %}

```java
class Solution {
    int result;
    public int diameterOfBinaryTree(TreeNode root) {
        result = 0;
        //不关心dfs结果，我们用result记录结果，在dfs过程中更新result
        dfs(root);
        return result;
    }
    
    //以当前节点为转折节点或者途径节点
    private int dfs(TreeNode node) {
        if (node == null) {
            return -1;
        }
        
        //左右子树加根结点的路径
        int left = dfs(node.left) + 1;
        int right = dfs(node.right) + 1;
        //把current node为当前转折点
        result = Math.max(result, left + right);
        //不把当前node当作转折点，取一个大的节点返回给根结点
        //相当于node.left + 1 || node.right + 1
        return Math.max(left, right);
    }
}
```
