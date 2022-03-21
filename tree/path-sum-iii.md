---
description: LeetCode 437
---

# Path Sum III

{% embed url="https://leetcode.com/problems/path-sum-iii/" %}

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

{% hint style="info" %}
在树上的prefix sum。

基本思想和[Subarray Sum Equals K](../arrays/prefix-sum-problems/subarray-sum-equals-k.md)类似

递归函数的定义：从当前节点出发，记录以当前节点为根，和为target组合的次数。

用map记录当前遍历点之前的所有和，并且更新它出现的次数，如果在map里找到了sum - target,那就加上它出现的所有情况。

记得backtrack。
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
    public int pathSum(TreeNode root, int targetSum) {
        Map<Integer, Integer> map = new HashMap<>();
        
        //初始化的map要加入(0, 1);
        //避免2,3,5 k = 5时，找cur - sum找不到
        //其实这个时候cur == sum,map应该有key为0的。
        map.put(0, 1);
        return dfs(root, 0, targetSum, map);
    }
    private int dfs(TreeNode root, int cur, int sum, Map<Integer, Integer> map) {
        if (root == null) {
            return 0;
        }
        
        cur += root.val;
        int res = map.getOrDefault(cur - sum, 0);
        
        
        map.put(cur, map.getOrDefault(cur, 0) + 1);
        
        res += dfs(root.left, cur, sum, map) +
            dfs(root.right, cur, sum, map);
        
        map.put(cur, map.get(cur) - 1);
        
        return res;
    }
}
```
