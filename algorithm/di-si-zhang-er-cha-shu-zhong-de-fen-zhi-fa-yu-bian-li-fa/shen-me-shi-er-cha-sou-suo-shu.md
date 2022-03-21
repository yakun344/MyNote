# 什么是二叉搜索树

#### 定义

二叉搜索树（Binary Search Tree，又名排序二叉树，二叉查找树，通常简写为BST）定义如下：\
**空树**或是**具有下列性质的二叉树**：\
（1）若左子树不空，则左子树上所有节点值均小于或等于它的根节点值；\
（2）若右子树不空，则右子树上所有节点值均大于根节点值；\
（3）左、右子树也为二叉搜索树；\
如图即为BST：\


![](<../../.gitbook/assets/image (1).png>)

#### BST 的特性

* 按照[中序遍历](http://www.jiuzhang.com/tutorial/algorithm/405#)（inorder traversal）打印各节点，会得到**由小到大**的顺序。
* 在BST中搜索某值的平均情况下复杂度为O(logN)，最坏情况下复杂度为O(N)，其中N为节点个数。将待寻值与节点值比较，若不相等，则**通过是小于还是大于，可断定该值只可能在左子树还是右子树，继续向该子树搜索**。
* 在balanced BST中查找某值的时间复杂度为O(logN)。

#### BST 的作用

* 通过中序遍历，可快速得到升序节点列表。
* 在BST中查找元素，平均情况下时间复杂度是O(logN)；插入新节点，保持BST特性平均情况下要耗时O（logN）。（[参考链接](http://www.jiuzhang.com/tutorial/algorithm/401)）。
* 和有序数组的对比：有序数组查找某元素可以用二分法，时间复杂度是O（logN）；但是插入新元素，维护数组有序性要耗时O（N）。

#### 常见的BST面试题

[http://www.lintcode.com/en/tag/binary-search-tree/](http://www.lintcode.com/en/tag/binary-search-tree/)

BST是一种**重要**且**基本**的结构，其相关题目也十分经典，并延伸出很多算法。\
在BST之上，有许多高级且有趣的变种,以解决各式各样的问题，例如:

* 用于数据库或各语言标准库中索引的[红黑树](https://baike.baidu.com/item/%E7%BA%A2%E9%BB%91%E6%A0%91/2413209?fr=aladdin)
* 提升二叉树性能底线的[伸展树](https://baike.baidu.com/item/%E4%BC%B8%E5%B1%95%E6%A0%91/7003945?fr=aladdin)
* 优化红黑树的[AA树](https://baike.baidu.com/item/AA%E6%A0%91/9246960?fr=aladdin)
* 随机插入的[树堆](https://baike.baidu.com/item/Treap?fromtitle=%E6%A0%91%E5%A0%86\&fromid=4478083)
* 机器学习kNN算法的高维快速搜索[k-d树](https://baike.baidu.com/item/kd-tree/2302515)\
  …………
