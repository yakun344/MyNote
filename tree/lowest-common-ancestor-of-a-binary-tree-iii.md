---
description: LeetCode 1650
---

# Lowest Common Ancestor of a Binary Tree III

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iii/" %}

Given two nodes of a binary tree `p` and `q`, return _their lowest common ancestor (LCA)_.

Each node will have a reference to its parent node. The definition for `Node` is below:

```
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}
```

According to the [**definition of LCA on Wikipedia**](https://en.wikipedia.org/wiki/Lowest\_common\_ancestor): "The lowest common ancestor of two nodes p and q in a tree T is the lowest node that has both p and q as descendants (where we allow **a node to be a descendant of itself**)."

**Example 1:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5 since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

**Constraints:**

* The number of nodes in the tree is in the range `[2, 105]`.
* `-109 <= Node.val <= 109`
* All `Node.val` are **unique**.
* `p != q`
* `p` and `q` exist in the tree.

{% hint style="info" %}
Node中有parent指针，用set记录从p节点到root节点的所有parent节点。从q再往root的方向走，第一次发现set里有的节点，这个节点就是LCA。

实现细节：用cur记录当前遍历的node
{% endhint %}

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
};
*/

class Solution {
    public Node lowestCommonAncestor(Node p, Node q) {
        Set<Node> set = new HashSet();
        Node cur = p;
        while (cur != null) {
            set.add(cur);
            cur = cur.parent;
        }
        
        cur = q;
        while (cur != null) {
            if (set.contains(cur)) {
                return cur;
            }
            cur = cur.parent;
        }
        return null;
    }
}
```
