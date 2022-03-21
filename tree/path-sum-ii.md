---
description: LeetCode 113
---

# Path Sum II

{% embed url="https://leetcode.com/problems/path-sum-ii/" %}



Given the `root` of a binary tree and an integer `targetSum`, return all **root-to-leaf** paths where each path's sum equals `targetSum`.

A **leaf** is a node with no children.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: []
```

**Example 3:**

```
Input: root = [1,2], targetSum = 0
Output: []
```

**Constraints:**

* The number of nodes in the tree is in the range `[0, 5000]`.
* `-1000 <= Node.val <= 1000`
* `-1000 <= targetSum <= 1000`

#### DFS Approach

{% hint style="info" %}
Similar with [Subset Problems](../depth-first-search/subset-problems/)
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
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        dfs(root, targetSum, result, temp);
        return result;
    }
    
    private void dfs(TreeNode root, int curSum, List<List<Integer>> result, List<Integer> temp) {
        if (root == null) {
            return;
        }
        
        curSum -= root.val;
        
        if (root.left == null && root.right == null && curSum == 0) {
            temp.add(root.val);
            result.add(new ArrayList<>(temp));
            temp.remove(temp.size() - 1);
            return;
        }
        
        temp.add(root.val);
        dfs(root.left, curSum, result, temp);
        dfs(root.right, curSum, result, temp);
        temp.remove(temp.size() - 1);
        return;
    }
}
```
