# 递归三要素

我们以《二叉树的最大深度》和《二叉树的前序遍历》两个题目为例子，来分析一下递归的`三要素`。

相关题目链接：

[http://www.lintcode.com/problem/maximum-depth-of-binary-tree/](http://www.lintcode.com/problem/maximum-depth-of-binary-tree/)\
[http://www.lintcode.com/problem/binary-tree-preorder-traversal/](http://www.lintcode.com/problem/binary-tree-preorder-traversal/)

**1. 递归的定义**

> 每一个递归函数，都需要有明确的定义，有了正确的定义以后，才能够对递归进行拆解。

例子：\
Java:

```java
int maxDepth(TreeNode root)
```

Python:

```python
def maxDepth(root):
```

代表 `以 root 开头的子树的最大深度是多少`。\
Java:

```java
void preorder(TreeNode root, List<TreeNode> result)
```

Python:

```python
def preorder(root, result):
```

代表 `将 root 开头的子树的前序遍历放到 result 里面`

**2. 递归的拆解**

> 一个`大问题`如何拆解为若干个`小问题`去解决。

例子：\
Java:

```java
int leftDepth = maxDepth(root.left);
int rightDepth = maxDepth(root.right);
return Math.max(leftDepth, rightDepth) + 1;
```

Python:

```python
leftDepth = maxDepth(root.left)
rightDepth = maxDepth(root.right)
return max(leftDepth, rightDepth) + 1
```

整棵树的最大深度，可以拆解为先计算左右子树深度，然后在左右子树深度中找到最大值+1来解决。\
Java:

```java
result.add(root);
preorder(root.left, result);
preorder(root.right, result);
```

Python:

```python
result.append(root)
preorder(root.left, result)
perorder(root.right, result)
```

一棵树的前序遍历可以拆解为3个部分：

1. 根节点自己（root）
2. 左子树的前序遍历
3. 右子树的前序遍历

所以对应的，我们把这个递归问题也拆分为三个部分来解决：

1. 先把 root 放到 result 里 --> result.add(root);
2. 再把左子树的前序遍历放到 result 里 --> preorder(root.left, result)。回想一下递归的定义，是不是正是如此？
3. 再把右子树的前序遍历放到 result 里 --> preorder(root.right, result)。

**3. 递归的出口**

> 什么时候可以直接知道答案，不用再拆解，直接 return

例子：\
Java:

```java
// 二叉树的最大深度
if (root == null) {
    return 0;
}
```

Python:

```python
# 二叉树的最大深度
if not root:
    return 0
```

一棵空的二叉树，可以认为是一个高度为`0`的二叉树。\
Java:

```java
// 二叉树的前序遍历
if (root == null) {
    return;
}
```

Python:

```python
if not root:
    return
```

一棵空的二叉树，自然不用往 result 里放任何的东西。
