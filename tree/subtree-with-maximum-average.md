---
description: LintCode 597
---

# Subtree with Maximum Average

{% embed url="https://www.lintcode.com/problem/subtree-with-maximum-average/description" %}

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

**Example 1**

```
Input：
{1,-5,11,1,2,4,-2}
Output：11
Explanation:
The tree is look like this:
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2 
The average of subtree of 11 is 4.3333, is the maximun.
```

**Example 2**

```
Input：
{1,-5,11}
Output：11
Explanation:
     1
   /   \
 -5     11
The average of subtree of 1,-5,11 is 2.333,-5,11. So the subtree of 11 is the maximun.
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
class ResultType{
    public int sum;
    public int size;
    public ResultType(int sum, int size) {
        this.sum = sum;
        this.size = size;
    }
}
public class Solution {
    private TreeNode subtree;
    private ResultType subtreeResult;
    /**
     * @param root: the root of binary tree
     * @return: the root of the maximum average of subtree
     */
    public TreeNode findSubtree2(TreeNode root) {
        // write your code here
        if (root == null) {
            return null;
        }
        
        helper(root);
        return subtree;
    }
    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0);
        }
        
        //Calculate the average of the two trees sepseparately 
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        
        //Using result to record current result of subtree
        ResultType result = new ResultType(left.sum + right.sum + root.val, left.size + right.size + 1);
        
        //Compare current subtree with previous one
        if (subtree == null || result.sum * subtreeResult.size > subtreeResult.sum * result.size) {
            subtree = root;
            subtreeResult = result;
        }
        
        return result;
    }
}
```
