---
description: LeetCode 230 & LintCode 902
---

# Kth Smallest Element in a BST

{% embed url="https://leetcode.com/problems/kth-smallest-element-in-a-bst/" %}

{% embed url="https://www.lintcode.com/problem/kth-smallest-element-in-a-bst/description" %}

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

**Note:**\
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example 1:**

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Example 2:**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

**Follow up:**\
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

#### Divide and Conquer Approach

* Time complexity O(n)
* If we want to look up k several times, it's wise to collect child number of every node.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Map<TreeNode, Integer> numOfChild = new HashMap<>();
        countNum(root, numOfChild);
        
        return quickSelect(root, k, numOfChild);
    }
    
    private int countNum(TreeNode root, Map<TreeNode, Integer> numOfChild) {
        if (root == null) {
            return 0;
        }
        
        int leftNum = countNum(root.left, numOfChild);
        int rightNum = countNum(root.right, numOfChild);
        
        numOfChild.put(root, leftNum + rightNum + 1);
        
        return leftNum + rightNum + 1;
    }
    
    private int quickSelect(TreeNode root, int k, Map<TreeNode, Integer> numOfChild) {
        if (root == null) {
            return -1;
        }
        
        int left = root.left == null ? 0 : numOfChild.get(root.left);
        
        if (left >= k) {
            return quickSelect(root.left, k, numOfChild);
        }
        
        if (left + 1 == k) {
            return root.val;
        }
        
        return quickSelect(root.right, k - left - 1, numOfChild);
    }
}
```

#### Iterator Approach

* Time complexity is O(h + k);

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
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
        
        for (int i = 0; i < k - 1; i++) {
            TreeNode node = stack.pop();
            TreeNode right = node.right;
            while (right != null) {
                stack.push(right);
                right = right.left;
            }
        }
        return stack.peek().val;
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
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> inorder = traverse(root, new ArrayList<>());
        return inorder.get(k - 1);
    }
    
    private List<Integer> traverse(TreeNode root, List<Integer> arr) {
        if (root == null) {
            return new ArrayList<>();
        }
        
        traverse(root.left, arr);
        arr.add(root.val);
        traverse(root.right, arr);
        
        return arr;
    }
}
```
