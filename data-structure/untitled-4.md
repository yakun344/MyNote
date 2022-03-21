---
description: LeetCode 173
---

# Binary Search Tree Iterator

{% embed url="https://leetcode.com/problems/binary-search-tree-iterator/" %}

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

**Example:**

![](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```

**Note:**

* `next()` and `hasNext()` should run in average O(1) time and uses O(h) memory, where h is the height of the tree.
* You may assume that `next()` call will always be valid, that is, there will be at least a next smallest number in the BST when `next()` is called.

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
class BSTIterator {
    
    ArrayList<Integer> nodesSorted;
    int index;
    
    public BSTIterator(TreeNode root) {
        this.nodesSorted = new ArrayList<>();
        this.index = -1;
        this.inorder(root);
    }
    
    public void inorder(TreeNode root) {
        if (root == null) {
            return;
        }
        this.inorder(root.left);
        this.nodesSorted.add(root.val);
        this.inorder(root.right);
    }
    
    /** @return the next smallest number */
    public int next() {
        return this.nodesSorted.get(++this.index);
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return this.index +1 < this.nodesSorted.size();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```
