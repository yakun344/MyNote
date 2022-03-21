---
description: LeetCode 1448
---

# Count Good Nodes in Binary Tree

{% embed url="https://leetcode.com/problems/count-good-nodes-in-binary-tree" %}

Given a binary tree `root`, a node _X_ in the tree is named **good** if in the path from root to _X_ there are no nodes with a value _greater than_ X.

Return the number of **good** nodes in the binary tree.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/02/test\_sample\_1.png)

```
Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/02/test\_sample\_2.png)

```
Input: root = [3,3,null,4,2]
Output: 3
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.
```

**Example 3:**

```
Input: root = [1]
Output: 1
Explanation: Root is considered as good.
```

&#x20;

**Constraints:**

* The number of nodes in the binary tree is in the range `[1, 10^5]`.
* Each node's value is between `[-10^4, 10^4]`.

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
    public int count = 0;
    public int goodNodes(TreeNode root) {
        dfs(root, root.val);
        return count;
    }
    public void dfs(TreeNode root, int curMax) {
        if (root == null)  {
            return;
        }
        
        if (root.val >= curMax) {
            count++;
        }
        
        dfs(root.left, Math.max(root.val, curMax));
        dfs(root.right, Math.max(root.val, curMax));
    }
}
```

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
    class Pair {
        TreeNode node;
        int maxCur;
        public Pair(TreeNode node, int maxCur) {
            this.node = node;
            this.maxCur= maxCur;
        }
    }
    public int goodNodes(TreeNode root) {
        int count = 0;
        Deque<Pair> queue = new LinkedList<>();
        queue.addLast(new Pair(root, root.val));
        
        while (!queue.isEmpty()) {
            Pair cur = queue.removeFirst();
            if (cur.maxCur <= cur.node.val) {
                count++;
            }
            
            if (cur.node.left != null) {
                queue.addLast(new Pair(cur.node.left, Math.max(cur.node.val, cur.maxCur)));
            }
            if (cur.node.right != null) {
                queue.addLast(new Pair(cur.node.right, Math.max(cur.node.val, cur.maxCur)));
            }
        }
        
        return count;
    }
}
```
