# 什么是平衡二叉搜索树

#### 定义

平衡二叉搜索树（Balanced Binary Search Tree，又称为AVL树，**有别于AVL算法**）是二叉树中的一种特殊的形态。二叉树当且仅当满足如下两个条件之一，是平衡二叉树：

* 空树。
* **左右子树高度差绝对值不超过1**且**左右子树都是平衡二叉树**。

![A](<../../.gitbook/assets/image (2).png>)

![B](<../../.gitbook/assets/image (4).png>)

如图（图片来自网络），节点旁边的数字表示左右两子树高度差。(a)是AVL树，(b)不是，(b)中5节点不满足AVL树，故4节点，3节点都不再是AVL树。

#### AVL树的高度为 O(logN)

当AVL树有N个节点时，高度为O(logN)。为何？\
试想一棵满二叉树，每个节点左右子树高度相同，随着树高的增加，叶子容量指数暴增，故树高一定是O(logN)。而相比于满二叉树，**AVL树仅放宽一个条件，允许左右两子树高度差1**，当树高足够大时，可以把1忽略。如图是高度为9的最小AVL树，若节点更少，树高绝不会超过8，也即为何AVL树高会被限制到O(logN)，因为**树不可能太稀疏**。严格的数学证明复杂,略去。\
![图片](http://media.jiuzhang.com/markdown/images/3/13/d4b9b288-268a-11e8-99b6-0242ac110002.jpg)

为何普通二叉树不是O(logN)？这里给出最坏的单枝树，若单枝扩展，则树高为O(N)：\
![图片](http://media.jiuzhang.com/markdown/images/3/13/23657f56-268c-11e8-b3fe-0242ac110002.jpg)

#### AVL树有什么用？

最大作用是保证查找的**最坏**时间复杂度为O(logN)。而且较浅的树对插入和删除等操作也更快。

#### AVL树的相关练习题

判断一棵树是否为平衡树\
[http://www.lintcode.com/problem/balanced-binary-tree/](http://www.lintcode.com/problem/balanced-binary-tree/)\
提示：可以自下而上递归判断每个节点是否平衡。若平衡将当前节点高度返回，供父节点判断;否则该树一定不平衡。
