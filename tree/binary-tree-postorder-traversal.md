---
description: LeetCode 145
---

# Binary Tree Postorder Traversal

{% embed url="https://leetcode.com/problems/binary-tree-postorder-traversal/" %}

Given a binary tree, return the _postorder_traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

#### Traversal Approach

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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> postorder = new ArrayList<>();
        if (root == null) {
            return postorder;
        }
        
        traverse(root, postorder);
        
        return postorder;
    }
    
    private void traverse(TreeNode root, List<Integer> postorder) {
        if (root == null) {
            return;
        }
        
        traverse(root.left, postorder);
        traverse(root.right, postorder);
        postorder.add(root.val);
    }
}
```

#### Divide and Conquer Approach

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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> postorder = new ArrayList<>();
        if (root == null) {
            return postorder;
        }
        
        List<Integer> left = postorderTraversal(root.left);
        List<Integer> right = postorderTraversal(root.right);
        
        postorder.addAll(left);
        postorder.addAll(right);
        postorder.add(root.val);
        
        return postorder;
    }
}
```

#### Iterative Approach

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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> postorder = new ArrayList<>();
        if (root == null) {
            return postorder;
        }
        
        TreeNode prev = null;
        
        
        //?????????????????????????????????????????????push???stack???
        Stack<TreeNode> stack = new Stack<>();
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
        
        while (!stack.isEmpty()) {
            TreeNode curr = stack.peek();
            
            //????????????curr????????????????????????????????????visited?????????visit curr???
            //??????prev
            if (curr.right == null || curr.right == prev) {
                postorder.add(curr.val);
                prev = curr;
                stack.pop();
            } else {
            
            //??????????????????????????????visited??????????????????????????????push
                curr = curr.right;
                while (curr != null) {
                    stack.push(curr);
                    curr = curr.left;
                }
            }
        }
        
        return postorder;
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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> postorder = new ArrayList<>();
        if (root == null) {
            return postorder;
        }
        
        TreeNode prev = null;
        TreeNode curr = root;
        
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            curr = stack.peek();
            
            //????????????push???stack????????????????????????visit??????
            if (prev == null || prev.left == curr || prev.right == curr) {
                if (curr.left != null) {
                    stack.push(curr.left);
                } else if (curr.right != null) {
                    stack.push(curr.right);
                }
                
            //???????????????????????????visited?????????push??????????????????stack???
            } else if (curr.left == prev) {
                if (curr.right != null) {
                    stack.push(curr.right);
                }
                
            //????????????????????????????????????postorder????????????pop???????????????
            } else {
                postorder.add(curr.val);
                stack.pop();
            }
            prev = curr;
        }
        
        return postorder;
    }
}
```
