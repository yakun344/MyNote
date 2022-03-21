---
description: LeetCode 272 LintCode 901
---

# Closest Binary Search Tree Value II

{% embed url="https://leetcode.com/problems/closest-binary-search-tree-value-ii/" %}

{% embed url="https://www.lintcode.com/problem/closest-binary-search-tree-value-ii/description" %}

{% hint style="danger" %}
Review
{% endhint %}

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

**Note:**

* Given target value is a floating point.
* You may assume k is always valid, that is: k ≤ total nodes.
* You are guaranteed to have only one unique set of k values in the BST that are closest to the target.

**Example:**

```
Input: root = [4,2,5,1,3], target = 3.714286, and k = 2

    4
   / \
  2   5
 / \
1   3

Output: [4,3]
```

**Follow up:**\
Assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

最优算法，时间复杂度 O(k + logn)，空间复杂度 O(logn)。

实现如下的子函数：

1. getStack() => 在假装插入 target 的时候，看看一路走过的节点都是哪些，放到 stack 里，用于 iterate
2. moveUpper(stack) => 根据 stack，挪动到 next node
3. moveLower(stack) => 根据 stack, 挪动到 prev node

有了这些函数之后，就可以把整个树当作一个数组一样来处理，只不过每次 i++ 的时候要用 moveUpper，i--的时候要用 moveLower

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
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> values = new ArrayList<>();
        
        if (k == 0 || root == null) {
            return values;
        }
        
        Stack<TreeNode> lowerStack = getStack(root, target);
        Stack<TreeNode> upperStack = new Stack<>();
        
        upperStack.addAll(lowerStack);
        
        if (target < lowerStack.peek().val) {
            moveLower(lowerStack);
        } else {
            moveUpper(upperStack);
        }
        
        for (int i = 0; i < k; ++i) {
            if (lowerStack.isEmpty() || !upperStack.isEmpty() && target - lowerStack.peek().val > upperStack.peek().val - target) {
                values.add(upperStack.peek().val);
                moveUpper(upperStack);
            } else {
                values.add(lowerStack.peek().val);
                moveLower(lowerStack);
            }
        }
        
        return values;
    }
    
    private Stack<TreeNode> getStack(TreeNode root, double target) {
        Stack<TreeNode> stack = new Stack<>();
        while (root != null) {
            stack.push(root);
            if (target < root.val) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return stack;
    }

    private void moveUpper(Stack<TreeNode> stack) {
        TreeNode node = stack.peek();
        if (node.right == null) {
            node = stack.pop();
            while (!stack.isEmpty() && stack.peek().right == node) {
                node = stack.pop();
            }
            return;
        }
        
        node = node.right;
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
    
    private void moveLower(Stack<TreeNode> stack) {
        TreeNode node = stack.peek();
        if (node.left == null) {
            node = stack.pop();
            while (!stack.isEmpty() && stack.peek().left == node) {
                node = stack.pop();
            }
            return;
        }
        
        node = node.left;
        while (node != null) {
            stack.push(node);
            node = node.right;
        }
    }
}
```
