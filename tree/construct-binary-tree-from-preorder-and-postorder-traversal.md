---
description: LeetCode 889
---

# Construct Binary Tree from Preorder and Postorder Traversal

{% embed url="https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/" %}



Return any binary tree that matches the given preorder and postorder traversals.

Values in the traversals `pre` and `post` are distinct positive integers.

**Example 1:**

```
Input: pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]
```

**Note:**

* `1 <= pre.length == post.length <= 30`
* `pre[]` and `post[]` are both permutations of `1, 2, ..., pre.length`.
* It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.

**Intuition**

A preorder traversal is:

* `(root node) (preorder of left branch) (preorder of right branch)`

While a postorder traversal is:

* `(postorder of left branch) (postorder of right branch) (root node)`

For example, if the final binary tree is `[1, 2, 3, 4, 5, 6, 7]` (serialized), then the preorder traversal is `[1] + [2, 4, 5] + [3, 6, 7]`, while the postorder traversal is `[4, 5, 2] + [6, 7, 3] + [1]`.

If we knew how many nodes the left branch had, we could partition these arrays as such, and use recursion to generate each branch of the tree.

**Algorithm**

Let's say the left branch has LL nodes. We know the head node of that left branch is `pre[1]`, but it also occurs last in the postorder representation of the left branch. So `pre[1] = post[L-1]` (because of uniqueness of the node values.) Hence, `L = post.indexOf(pre[1]) + 1`.

Now in our recursion step, the left branch is represnted by `pre[1 : L+1]` and `post[0 : L]`, while the right branch is represented by `pre[L+1 : N]` and `post[L : N-1]`.

{% hint style="info" %}
**解决这类题的关键就是在preorder inorder postorder里找到root**

递归出口

找到左子树的数量，来分割pre和post。左子树的根是left.pre的第一个， 也是post里的left children的最后一个，index 在left.post + L + 1

pre  =  {1, 2, 4, 5, 3, 6, 7}

post = {4, 5, 2, 6, 7, 3, 1}

递归过程，以node 1为根的左子树pre = {2, 3, 4} , post = {4, 5, 2}

右子树pre = {3, 6, 7} , post = {6, 7, 3}.

注意⚠️indices一定要分清楚。
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
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        int N = pre.length;
        if (N == 0) return null;
        TreeNode root = new TreeNode(pre[0]);
        if (N == 1) return root;
        
        //L is the number of left children;
        int L = 0;
        for (int i = 0; i < N; ++i) {
            if (pre[1] == post[i]) {
                L = i + 1;
            }
        }
        
        root.left = constructFromPrePost(Arrays.copyOfRange(pre, 1, L + 1),
                                        Arrays.copyOfRange(post, 0, L));
        root.right = constructFromPrePost(Arrays.copyOfRange(pre, L + 1, N),
                                         Arrays.copyOfRange(post, L, N - 1));
        return root;
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
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        int N = pre.length;
        if (N == 0) {
            return null;
        }
        
        TreeNode root = new TreeNode(pre[0]);
        if (N == 1) {
            return root;
        }
        
        //leftR is the root of left tree
        int leftR = 0;
        for (int i = 0; i < N; ++i) {
            if (pre[1] == post[i]) {
                leftR = i;
            }
        }
        
        root.left = constructFromPrePost(Arrays.copyOfRange(pre, 1, leftR + 2),
                                        Arrays.copyOfRange(post, 0, leftR + 1));
        root.right = constructFromPrePost(Arrays.copyOfRange(pre, leftR + 2, N),
                                         Arrays.copyOfRange(post, leftR + 1, N - 1));
        return root;
    }
}
```
