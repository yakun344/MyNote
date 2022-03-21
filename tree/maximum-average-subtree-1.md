---
description: LeetCode 1120
---

# Maximum Average Subtree

{% embed url="https://leetcode.com/problems/maximum-average-subtree/" %}

Given the `root` of a binary tree, find the maximum average value of any subtree of that tree.

(A subtree of a tree is any node of that tree plus all its descendants. The average value of a tree is the sum of its values, divided by the number of nodes.)

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/09/1308\_example\_1.png)

```
Input: [5,6,1]
Output: 6.00000
Explanation: 
For the node with value = 5 we have an average of (5 + 6 + 1) / 3 = 4.
For the node with value = 6 we have an average of 6 / 1 = 6.
For the node with value = 1 we have an average of 1 / 1 = 1.
So the answer is 6 which is the maximum.
```

**Note:**

1. The number of nodes in the tree is between `1` and `5000`.
2. Each node will have a value between `0` and `100000`.
3. Answers will be accepted as correct if they are within `10^-5` of the correct answer.

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
    public class ResultType{
        int sum, size;
        public ResultType(int sum, int size) {
            this.sum = sum;
            this.size = size;
        }
    }
    private ResultType subTreeResult = null;
    private TreeNode subTree;
    public double maximumAverageSubtree(TreeNode root) {
        divideConquer(root);
        return (double)(subTreeResult.sum) / (subTreeResult.size);
    }
    
    private ResultType divideConquer(TreeNode node) {
        if (node == null) {
            return new ResultType(0, 0);
        }
        
        ResultType left = divideConquer(node.left);
        ResultType right = divideConquer(node.right);
        
        ResultType result = new ResultType(
            left.sum + right.sum + node.val,
            left.size + right.size + 1);
        
        //rs.sum / rs.size > sub.sum / sub.size
        if (subTree == null ||
           result.sum * subTreeResult.size > result.size * subTreeResult.sum) {
            subTree = node;
            subTreeResult = result;
        }
        return result;
    }
}
```
