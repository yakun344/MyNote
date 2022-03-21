# 第四章课后补充内容

二叉树相关有一些内容，如果学有余力可以掌握一下，可以提升自信心（因为知道了别人不知道的东西），更有底气去面试（虽然考到的概率很低）：

1. 用 Morris 算法实现 O(1) 额外空间对二叉树进行先序遍历
2. 用非递归（Non-recursion / Iteration）的方式实现二叉树的前序遍历，中序遍历和后序遍历
3. BST 的增删查改
4. 平衡排序二叉树（Balanced Binary Search Tree）及 TreeSet / TreeMap 的使用

## 非递归的方式实现二叉树遍历

#### 先序遍历

**思路**

遍历顺序为**根**、**左**、**右**

1. 如果根节点非空，将根节点加入到栈中。
2. 如果栈不空，弹出出栈顶节点，将其值加加入到数组中。
   1. 如果该节点的右子树不为空，将右子节点加入栈中。
   2. 如果左子节点不为空，将左子节点加入栈中。
3. 重复第二步，直到栈空。

**代码实现**

Java:

```java
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        List<Integer> preorder = new ArrayList<Integer>();
        
        if (root == null) {
            return preorder;
        }
        
        stack.push(root);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            preorder.add(node.val);
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        
        return preorder;
    }
}
```

Python:

```python
class Solution:
    """
    @param root: A Tree
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        stack = []
        preorder = []

        if not root:
            return preorder

        stack.append(root)
        while len(stack) > 0:
            node = stack.pop()
            preorder.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        
        return preorder
```

**练习**

[http://www.lintcode.com/problem/binary-tree-preorder-traversal/](http://www.lintcode.com/problem/binary-tree-preorder-traversal/)

#### 中序遍历

**思路**

遍历顺序为**左**、**根**、**右**

1. 如果根节点非空，将根节点加入到栈中。
2. 如果栈不空，取栈顶元素（暂时不弹出），
   1. 如果左子树已访问过，或者左子树为空，则弹出栈顶节点，将其值加入数组，如有右子树，将右子节点加入栈中。
   2. 如果左子树不为空，则将左子节点加入栈中。
3. 重复第二步，直到栈空。

**代码实现**

Java:

```python
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
//推荐，这个比较好理解
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> inorder = new ArrayList<>();
        if (root == null) {
            return inorder;
        }
        
        Stack<TreeNode> stack = new Stack<>();
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            inorder.add(node.val);
            
            if (node.right != null) {
                node = node.right;
                while (node != null) {
                    stack.push(node);
                    node = node.left;
                }
            }
        }
        
        return inorder;
    }
}
```

```java
/**
如果根节点非空，将根节点加入到栈中。
如果栈不空，取栈顶元素（暂时不弹出），
    如果左子树已访问过，或者左子树为空，则弹出栈顶节点，将其值加入数组，如有右子树，将右子节点加入栈中。
    如果左子树不为空，则将左子节点加入栈中。
重复第二步，直到栈空。
*/
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: Inorder in ArrayList which contains node values.
     */
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        ArrayList<Integer> result = new ArrayList<>();
        
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    
        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            result.add(node.val);
            
            if (node.right == null) {
                node = stack.pop();
                while (!stack.isEmpty() && stack.peek().right == node) {
                    node = stack.pop();
                }
            } else {
                node = node.right;
                while (node != null) {
                    stack.push(node);
                    node = node.left;
                }
            }
        }
        return result;
    }
}
```

Python

```python
class Solution:
    """
    @param root: A Tree
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        stack = []
        result = []

        while root:
            stack.append(root)
            root = root.left

        while len(stack) > 0:
            node = stack[-1]
            result.append(node.val)

            if not node.right:
                node = stack.pop()
                while len(stack) > 0 and stack[-1].right == node:
                    node = stack.pop()
            else:
                node = node.right
                while node:
                    stack.append(node)
                    node = node.left
        
        return result
```

**练习**

[http://www.lintcode.com/problem/binary-tree-inorder-traversal/](http://www.lintcode.com/problem/binary-tree-inorder-traversal/)

#### 后序遍历

**思路**

遍历顺序为**左**、**右**、**根**

1. 如果根节点非空，将根节点加入到栈中。
2. 如果栈不空，取栈顶元素（暂时不弹出），
   1. 如果（左子树已访问过或者左子树为空），且（右子树已访问过或右子树为空），则弹出栈顶节点，将其值加入数组，
   2. 如果左子树不为空，且未访问过，则将左子节点加入栈中，并标左子树已访问过。
   3. 如果右子树不为空，且未访问过，则将右子节点加入栈中，并标右子树已访问过。
3. 重复第二步，直到栈空。

**代码实现**

Java:

```java
public ArrayList<Integer> postorderTraversal(TreeNode root) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode prev = null; // previously traversed node
    TreeNode curr = root;

    if (root == null) {
        return result;
    }

    stack.push(root);
    while (!stack.empty()) {
        curr = stack.peek();
        if (prev == null || prev.left == curr || prev.right == curr) { // traverse down the tree
            if (curr.left != null) {
                stack.push(curr.left);
            } else if (curr.right != null) {
                stack.push(curr.right);
            }
        } else if (curr.left == prev) { // traverse up the tree from the left
            if (curr.right != null) {
                stack.push(curr.right);
            }
        } else { // traverse up the tree from the right
            result.add(curr.val);
            stack.pop();
        }
        prev = curr;
    }

    return result;
}
```

Python

```python
class Solution:
    """
    @param root: A Tree
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        result = []
        stack = []
        prev, curr = None, root

        if not root:
            return result

        stack.append(root)
        while len(stack) > 0:
            curr = stack[-1]
            if not prev or prev.left == curr or prev.right == curr:  # traverse down the tree
                if curr.left:
                    stack.append(curr.left)
                elif curr.right:
                    stack.append(curr.right)
            elif curr.left == prev:  # traverse up the tree from the left
                if curr.right:
                    stack.append(curr.right)
            else:  # traverse up the tree from the right
                result.append(curr.val)
                stack.pop()
            prev = curr

        return result
```

**练习**

[http://www.lintcode.com/problem/binary-tree-postorder-traversal/](http://www.lintcode.com/problem/binary-tree-postorder-traversal/)

## 用 Morris 算法实现 O(1) 额外空间遍历二叉树

#### 什么是 Morris 算法

与递归和使用栈空间遍历的思想不同，Morris 算法使用二叉树中的叶节点的right指针来保存后面将要访问的节点的信息，当这个right指针使用完成之后，再将它置为null，但是在访问过程中有些节点会访问两次，所以与递归的空间换时间的思路不同，Morris则是使用时间换空间的思想。

#### 节点定义

Java:

```java
class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;
    pubic TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}
```

Python:

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
```

#### 用 Morris 算法进行中序遍历(Inorder Traversal)

#### **思路**

```
1. 如果当前节点的左孩子为空，则输出当前节点并将其右孩子作为当前节点。
2. 如果当前节点的左孩子不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点。
    1. 如果前驱节点的右孩子为空，将它的右孩子设置为当前节点。当前节点更新为当前节点的左孩子。
    2. 如果前驱节点的右孩子为当前节点，将它的右孩子重新设为空（恢复树的形状）。输出当前节点。当前节点更新为当前节点的右孩子。
3. 重复1、2两步直到当前节点为空。
```

**图示**

下图为每一步迭代的结果（从左至右，从上到下），cur代表当前节点，深色节点表示该节点已输出。\
![InOrder](http://media.jiuzhang.com/markdown/images/3/14/6784d3e2-2765-11e8-96a4-0242ac110002.jpg)

**示例代码**

Java:

```java
public class Solution {
    /**
     * @param root: A Tree
     * @return: Inorder in ArrayList which contains node values.
     */

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> nums = new ArrayList<>();
        TreeNode cur = null;

        while (root != null) {
            if (root.left != null) {
                cur = root.left;
                while (cur.right != null && cur.right != root) {
                    cur = cur.right;
                }

                if (cur.right == root) {
                    nums.add(root.val);
                    cur.right = null;
                    root = root.right;
                } else {
                    cur.right = root;
                    root = root.left;
                }               
            } else {
                nums.add(root.val);
                root = root.right;
            }
        }

        return nums;
    }
}
```

Python:

```python
class Solution:
    """
    @param root: A Tree
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        nums = []
        cur = None
    
        while root:
            if root.left:
                cur = root.left
                while cur.right and cur.right != root:
                    cur = cur.right
                
                if cur.right == root:
                    nums.append(root.val)
                    cur.right = None
                    root = root.right
                else:
                    cur.right = root
                    root = root.left
            else:
                nums.append(root.val)
                root = root.right
                
        return nums
```

**LintCode 练习**

[http://www.lintcode.com/problem/binary-tree-inorder-traversal/](http://www.lintcode.com/problem/binary-tree-inorder-traversal/)

#### 用 Morris 算法实现先序遍历(Preorder Traversal)

**思路**

```
1. 如果当前节点的左孩子为空，则输出当前节点并将其右孩子作为当前节点。
2. 如果当前节点的左孩子不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点。
    1. 如果前驱节点的右孩子为空，将它的右孩子设置为当前节点。**输出当前节点**（与中序遍历唯一一点不同）。当前节点更新为当前节点的左孩子。
    2. 如果前驱节点的右孩子为当前节点，将它的右孩子重新设为空。当前节点更新为当前节点的右孩子。
3. 重复1、2两步直到当前节点为空。
```

**图示**

![preorder](http://media.jiuzhang.com/markdown/images/3/14/d6667ad8-2768-11e8-96a4-0242ac110002.jpg)

**示例代码**

Java:

```java
public class Solution {
    /**
     * @param root: A Tree
     * @return: Preorder in ArrayList which contains node values.
     */
    public List<Integer> preorderTraversal(TreeNode root) {
        // morris traversal
        List<Integer> nums = new ArrayList<>();
        TreeNode cur = null;
        while (root != null) {
            if (root.left != null) {
                cur = root.left;
                // find the predecessor of root node
                while (cur.right != null && cur.right != root) {
                    cur = cur.right;
                }
                if (cur.right == root) {
                    cur.right = null;
                    root = root.right;
                } else {
                    nums.add(root.val);
                    cur.right = root;
                    root = root.left;
                }
            } else {
                nums.add(root.val);   
                root = root.right;
            }
        }
        return nums;
    } 
}
```

Python:

```python
class Solution:
    """
    @param root: A Tree
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        nums = []
        cur = None
        
        while root:
            if root.left:
                cur = root.left
                while cur.right and cur.right != root:
                    cur = cur.right
                if cur.right == root:
                    cur.right = None
                    root = root.right
                else:
                    nums.append(root.val)
                    cur.right = root
                    root = root.left
            else:
                nums.append(root.val)
                root = root.right
                
        return nums
```

**LintCode 练习**

[http://www.lintcode.com/problem/binary-tree-preorder-traversal/](http://www.lintcode.com/problem/binary-tree-preorder-traversal/)

#### 用 Morris 算法实现后序遍历(Postorder Traversal)

**思路**

```
* 后序遍历其实可以看作是和前序遍历左右对称的，此处，我们同样可以利用这个性质，基于前序遍历的算法，可以很快得到后序遍历的结果。我们只需要将前序遍历中所有的左孩子和右孩子进行交换就可以了。
```

**示例代码**

Java:

```java
public class Solution {
    /**
     * @param root: A Tree
     * @return: Postorder in ArrayList which contains node values.
     */
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> nums = new ArrayList<>();
        TreeNode cur = null;
        while (root != null) {
            if (root.right != null) {
                cur = root.right;
                while (cur.left != null && cur.left != root) {
                    cur = cur.left;
                }
                if (cur.left == root) {
                    cur.left = null;
                    root = root.left;
                } else {
                    nums.add(root.val);
                    cur.left = root;
                    root = root.right;
                }
            } else {
                nums.add(root.val);
                root = root.left;
            }
        }
        Collections.reverse(nums);
        return nums;
    } 
}
```

Python:

```python
class Solution:
    """
    @param root: A Tree
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        nums = []
        cur = None

        while root:
            if root.right != None:
                cur = root.right
                while cur.left and cur.left != root:
                    cur = cur.left
                if cur.left == root:
                    cur.left = None
                    root = root.left
                else:
                    nums.append(root.val)
                    cur.left = root
                    root = root.right
            else:
                nums.append(root.val)
                root = root.left
                
        nums.reverse()
        return nums
```

**LintCode 练习**

[http://www.lintcode.com/problem/binary-tree-postorder-traversal/](http://www.lintcode.com/problem/binary-tree-postorder-traversal/)

## BST 的增删查改

#### 什么是二叉搜索树(Binary Search Tree)

**二叉搜索树**可以是一棵空树或者是一棵满足下列条件的[二叉树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%A0%91):

* 如果它的左子树不空，则左子树上所有节点值`均小于`它的根节点值。
* 如果它的右子树不空，则右子树上所有节点值`均大于`它的根节点值。
* 它的左右子树均为二叉搜索树(BST)。
* 严格定义下BST中是没有值相等的节点的(No duplicate nodes)。\
  根据上述特性，我们可以得到一个结论：BST**中序遍历**得到的序列是**升序**的。如下述BST的中序序列为：\[1,3,4,6,7,8,10,13,14]

![BST](http://media.jiuzhang.com/markdown/images/3/14/64328624-2749-11e8-b863-0242ac110002.jpg)

#### BST基本操作——增删改查(CRUD)

对于树节点的定义如下：\
Java:

```java
class TreeNode{
	int val;
	TreeNode left;
	TreeNode right;
	pubic TreeNode(int val) {
		this.val = val;
		this.left = this.right = null;
	}
}
```

Python:

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
```

**基本操作之查找(Retrieve)**

* 思路
  * 查找值为**val**的节点，如果val小于根节点则在左子树中查找，反之在右子树中查找
* 代码实现

Java:

```java
public TreeNode searchBST(TreeNode root, int val) {
	if (root == null) {
		return null;
	}// 未找到值为val的节点
	if (val < root.val) {
		return searchBST(root.left, val);//val小于根节点值，在左子树中查找
	} else if (val > root.val) {
		return searchBST(root.right, val);//val大于根节点值，在右子树中查找
	} else {
		return root;//找到了
	}
}
```

Python:

```python
def searchBST(root, val):
    if not root:
        return None # 未找到值为val的节点
    if val < root.val:
        return searchBST(root.left, val) # val小于根节点值，在左子树中查找哦
    elif val > root.val:
        return searchBST(root.right, val) # val大于根节点值，在右子树中查找
    else:
        return root
```

* 实战
  * [http://www.lintcode.com/en/problem/search-range-in-binary-search-tree/](http://www.lintcode.com/en/problem/search-range-in-binary-search-tree/)
  * [http://www.lintcode.com/en/problem/two-sum-bst-edtion/](http://www.lintcode.com/en/problem/two-sum-bst-edtion/)
  * [http://www.lintcode.com/en/problem/closest-binary-search-tree-value/](http://www.lintcode.com/en/problem/closest-binary-search-tree-value/)
  * [http://www.lintcode.com/en/problem/closest-binary-search-tree-value-ii/](http://www.lintcode.com/en/problem/closest-binary-search-tree-value-ii/)
  * [http://www.lintcode.com/en/problem/trim-binary-search-tree/](http://www.lintcode.com/en/problem/trim-binary-search-tree/)
  * [http://www.lintcode.com/en/problem/bst-swapped-nodes/](http://www.lintcode.com/en/problem/bst-swapped-nodes/)

**基本操作之修改(Update)**

* 思路
  * 修改仅仅需要在查找到需要修改的节点之后，更新这个节点的值就可以了
* 代码实现

Java:

```java
public void updateBST(TreeNode root, int target, int val) {
	if (root == null) {
		return;
	}// 未找到target节点
	if (target < root.val) {
		updateBST(root.left, target, val);//target小于根节点值，在左子树中查找
	} else if (target > root.val) {
		updateBST(root.right, target, val);//target大于根节点值，在右子树中查找
	} else { //找到了
		root.val = val;
	}
}
```

Python:

```python
def updateBSTBST(root, target, val):
    if not root:
        return  # 未找到target节点
    if target < root.val:
        updateBST(root.left, target, val) # target小于根节点值，在左子树中查找哦
    elif target > root.val:
        updateBST(root.right, target, val) # target大于根节点值，在右子树中查找
    else:  # 找到了
        root.val = val
```

* 实战
  * [http://www.lintcode.com/en/problem/bst-swapped-nodes/](http://www.lintcode.com/en/problem/bst-swapped-nodes/)

**基本操作之增加(Create)**

* 思路
  * 根节点为空，则待添加的节点为根节点
  * 如果待添加的节点值小于根节点，则在左子树中添加
  * 如果待添加的节点值大于根节点，则在右子树中添加
  * 我们统一在树的叶子节点(Leaf Node)后添加
* 代码实现

Java:

```java
public TreeNode insertNode(TreeNode root, TreeNode node) {
    if (root == null) {
        return node;
    }
    if (root.val > node.val) {
        root.left = insertNode(root.left, node);
    } else {
        root.right = insertNode(root.right, node);
    }
    return root;
}
```

Python:

```python
def insertNode(root, node):
    if not root:
        return node
    if root.val > node.val:
        root.left = insertNode(root.left, node)
    else:
        root.right = insertNode(root.right, node)
    return root
```

* 实战
  * [http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/](http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/)

**基本操作之删除(Delete)**

* 思路(最为复杂)
  * 考虑待删除的节点为叶子节点，可以直接删除并修改父亲节点(Parent Node)的指针，需要区分待删节点是否为根节点
  * 考虑待删除的节点为单支节点(只有一棵子树——左子树 or 右子树)，与删除链表节点操作类似，同样的需要区分待删节点是否为根节点
  * 考虑待删节点有两棵子树，可以将待删节点与左子树中的最大节点进行交换，由于左子树中的最大节点一定为叶子节点，所以这时再删除待删的节点可以参考第一条
  * 详细的解释可以看 [http://www.algolist.net/Data\_structures/Binary\_search\_tree/Removal](http://www.algolist.net/Data\_structures/Binary\_search\_tree/Removal)
* 代码实现

Java:

```java
public TreeNode removeNode(TreeNode root, int value) {
    TreeNode dummy = new TreeNode(0);
    dummy.left = root;
    TreeNode parent = findNode(dummy, root, value);
    TreeNode node;
    if (parent.left != null && parent.left.val == value) {
        node = parent.left;
    } else if (parent.right != null && parent.right.val == value) {
        node = parent.right;
    } else {
        return dummy.left;
    }
    deleteNode(parent, node);
    return dummy.left;
}

private TreeNode findNode(TreeNode parent, TreeNode node, int value) {
    if (node == null) {
        return parent;
    }
    if (node.val == value) {
        return parent;
    }
    if (value < node.val) {
        return findNode(node, node.left, value);
    } else {
        return findNode(node, node.right, value);
    }
}

private void deleteNode(TreeNode parent, TreeNode node) {
    if (node.right == null) {
        if (parent.left == node) {
            parent.left = node.left;
        } else {
            parent.right = node.left;
        }
    } else {
        TreeNode temp = node.right;
        TreeNode father = node;
        while (temp.left != null) {
            father = temp;
            temp = temp.left;
        }
        if (father.left == temp) {
            father.left = temp.right;
        } else {
            father.right = temp.right;
        }
        if (parent.left == node) {
            parent.left = temp;
        } else {
            parent.right = temp;
        }
        temp.left = node.left;
        temp.right = node.right;
    }
}
```

Python:

```python
def removeNode(root, value):
    dummy = TreeNode(0)
    dummy.left = root
    parent = findNode(dummy, root, value)
    node = None
    if parent.left and parent.left.val == value:
        node = parent.left
    elif parent.right and parent.right.val == value:
        node = parent.right
    else:
        return dummy.left
    deleteNode(parent, node)
    return dummy.left

def findNode(parent, node, value):
    if not node:
        return parent
    if node.val == value:
        return parent
    if value < node.val:
        return findNode(node,node.left, value)
    else:
        return findNode(node, node.right, value)

def deleteNode(parent, node):
    if not node.right:
        if parent.left == node:
            parent.left = node.left
        else:
            parent.right = node.left
    else:
        temp = node.right
        father = node
        while temp.left:
            father = temp
            temp = temp.left
        if father.left == temp:
            father.left = temp.right
        else:
            father.right = temp.right
        if parent.left == node:
            parent.left = temp
        else:
            parent.right = temp
        temp.left = node.left
        temp.right = node.right
```

* 实战
  * [http://www.lintcode.com/en/problem/remove-node-in-binary-search-tree/](http://www.lintcode.com/en/problem/remove-node-in-binary-search-tree/)
  * [http://www.lintcode.com/en/problem/trim-binary-search-tree/](http://www.lintcode.com/en/problem/trim-binary-search-tree/)

## 平衡排序二叉树

#### 平衡排序二叉树(Self-balancing Binary Search Tree)

**定义**

平衡二叉搜索树又被称为AVL树（有别于AVL算法），且具有以下性质：

* 它是一棵空树或它的左右两个子树的高度差的绝对值不超过1
* 左右两棵子树都是一棵平衡二叉搜索树
* 平衡二叉搜索树必定是二叉搜索树，反之则不一定。

**平衡排序二叉树 与 二叉搜索树 对比**

也许因为输入值不够随机，也许因为输入顺序的原因，还或许一些插入、删除操作，会使得二叉搜索树失去平衡，造成搜索效率低落的情况。\
![AVL](http://media.jiuzhang.com/markdown/images/3/15/7c52db9e-27ff-11e8-af91-0242ac110002.jpg)\
比如上面两个树，在平衡树上寻找15就只要2次查找，在非平衡树上却要5次查找方能找到，效率明显下降。

**平衡排序二叉树节点定义**

Java:

```java
class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;
    pubic TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}
```

Python:

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
```

**常用的实现办法**

* AVL树 --> [https://en.wikipedia.org/wiki/AVL\_tree](https://en.wikipedia.org/wiki/AVL\_tree)
* 红黑树(Red Black Tree) --> [http://blog.csdn.net/v\_july\_v/article/details/6105630](http://blog.csdn.net/v\_july\_v/article/details/6105630)

#### Java中的 [TreeSet](https://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html) / [TreeMap](https://docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html)

TreeSet / TreeMap 是底层运用了[红黑树](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)的数据结构

**对比 HashSet / HashMap**

* HashSet / HashMap 存取的时间复杂度为**O(1)**,而 TreeSet / TreeMap 存取的时间复杂度为 **O(logn)** 所以在存取上并不占优。
* HashSet / HashMap 内元素是无序的，而TreeSet / TreeMap 内部是有序的(可以是按自然顺序排列也可以自定义排序)。
* TreeSet / TreeMap 还提供了类似 [lowerBound](http://www.cplusplus.com/reference/algorithm/lower\_bound/) 和 [upperBound](http://www.cplusplus.com/reference/algorithm/upper\_bound/) 这两个其他数据结构没有的方法
  * 对于 TreeSet, 实现上述两个方法的方法为：
    * **lowerBound**
      * **public E lower(E e)** --> 返回set中**严格小于**给出元素的**最大元素**，如果没有满足条件的元素则返回 **null**。
      * **public E floor(E e)** --> 返回set中**不大于**给出元素的**最大元素**，如果没有满足条件的元素则返回 **null**。
    * **upperBound**
      * **public E higher(E e)** --> 返回set中**严格大于**给出元素的**最小元素**，如果没有满足条件的元素则返回 **null**。
      * **public E ceiling(E e)** --> 返回set中**不小于**给出元素的**最小元素**，如果没有满足条件的元素则返回 **null**。
  * 对于 TreeMap, 实现上述两个方法的方法为：
    * **lowerBound**
      * **public Map.Entry\<K,V> lowerEntry(K key)** --> 返回map中**严格小于**给出的key值的**最大key**对应的**key-value对**，如果没有满足条件的key则返回 **null**。
      * **public K lowerKey(K key)** --> 返回map中**严格小于**给出的key值的**最大key**，如果没有满足条件的key则返回 **null**。
      * **public Map.Entry\<K,V> floorEntry(K key)** --> 返回map中**不大于**给出的key值的**最大key**对应的**key-value对**，如果没有满足条件的key则返回 **null**。
      * **public K floorKey(K key)** --> 返回map中**不大于**给出的key值的**最大key**，如果没有满足条件的key则返回 **null**。
    * **upperBound**
      * **public Map.Entry\<K,V> higherEntry(K key)** --> 返回map中**严格大于**给出的key值的**最小key**对应的**key-value对**，如果没有满足条件的key则返回 **null**。
      * **public K higherKey(K key)** --> 返回map中**严格大于**给出的key值的**最小key**，如果没有满足条件的key则返回 **null**。
      * **public Map.Entry\<K,V> ceilingEntry(K key)** --> 返回map中**不小于**给出的key值的**最小key**对应的**key-value对**，如果没有满足条件的key则返回 **null**。
      * **public K ceilingKey(K key)** --> 返回map中**不小于**给出的key值的**最小key**，如果没有满足条件的key则返回 **null**。
  * lowerBound 与 upperBound 均为二分查找(因此要求有序)，时间复杂度为**O(logn)**.

**对比 PriorityQueue(Heap)**

**PriorityQueue**是基于Heap实现的，它可以保证队头元素是优先级最高的元素，但其余元素是不保证有序的。

* 方法时间复杂度对比：
  * 添加元素 add() / offer()
    * TreeSet: O(logn)
    * PriorityQueue: O(logn)
  * 删除元素 poll() / remove()
    * TreeSet: O(logn)
    * PriorityQueue: O(n)
  * 查找 contains()
    * TreeSet: O(logn)
    * PriorityQueue: O(n)
  * 取最小值 first() / peek()
    * TreeSet: O(logn)
    * PriorityQueue: O(1)

**常见用法**

比如滑动窗口需要保证有序，那么这时可以用到TreeSet,因为TreeSet是有序的，并且不需要每次移动窗口都重新排序，只需要插入和删除(O(logn))就可以了。

注：在 C++ 中类似的结构为 set / map。在Python中没有内置的TreeSet、TreeMap，需要使用第三方库或者自己实现。

**练习**

[http://www.lintcode.com/problem/consistent-hashing-ii/](http://www.lintcode.com/problem/consistent-hashing-ii/)

### 练习：链表转平衡排序二叉树

#### 题目描述

将有序链表转换为平衡的排序二叉树。

LintCode 练习地址：\
[http://www.lintcode.com/en/problem/convert-sorted-list-to-balanced-bst/](http://www.lintcode.com/en/problem/convert-sorted-list-to-balanced-bst/)

#### 粗暴的算法

可以十分容易想到一个一个 O(nlogn) 的分治算法，以链表作为参数，二叉树作为返回值：\
Java:

```java
TreeNode convert(ListNode head) {
    if (head == null) {
        return null;
    }

    // find the following three nodes in the linked list
    // .... prev -> mid -> next ...
    // prev.next = null; // break the connect between prev & mid
    
    TreeNode root = new TreeNode(mid.val);
    root.left = convertListBBT(head);
    root.right = convertListBBT(next);
    return root;
}
```

Python:

```python
def convert(head):
    if not head:
        return null
				
    # find the following three nodes in the linked list
    # .... prev -> mid -> next ...
    # prev.next = null; // break the connect between prev & mid
		
		root = TreeNode(mid.val)
    root.left = convertListBBT(head)
    root.right = convertListBBT(next)
    return root
```

算法的大致思路就是，找到链表中点和他前后的点，然后左边的部分递归生成一棵左子树，右边的部分递归生成一棵右子树，再和中间的点拼接起来就好了。

这个算法我们不难发现他的时间复杂度是 O(nlogn)O(nlogn) 的，因为找到中点的时间复杂度是 O(n)O(n)，因此可以用 T 函数推算法来进行推算：

```
T(n) = 2 * T(n/2) + O(n) = O(nlogn)
```

#### 优化的算法

为了优化这个算法，我们给分治函数带上了一个参数 n 代表目前打算去转换 head 开始，长度为 n 那么多个节点，让其变为 Balanced Binary Tree。递归函数接口如下：\
Java:

```java
TreeNode convert(ListNode head, int n)
```

Python

```python
"""
Returns a TreeNode.
"""
def convert(head, n):
```

这样，我们不用真正把链表从 prev 和 mid 之间断开。可以利用对第二个参数的大小控制来让处理规模缩小。

但是虽然我们可以很快的调用 convert(head, n / 2)，让链表的一半变成二叉树。但是如何很快知道链表的中点呢？这里的办法是，如果我们把 head 放在参数里，那么就无法利用 convert 函数对 head 进行挪动了，所以我们把 head 挪出来，放到全局，作为一个全局变量。这样之后函数的接口改为：\
Java:

```java
public class Solution {
    private ListNode current;

    private TreeNode convert(int n) {
        // ...
    }

    // the entry point to public
    public TreeNode sortedListToBST(ListNode head) {
        current = head;
        convert(getLength(head));
    }
```

Python:

```python
class Solution:
    def __init(self):
        self.current = None

    def convert(self, n):
        # ...

    def sortedListToBST(self, head):
        self.current = head
        self.convert(getLength(head));
```

这里我们在全局放了一个 current 指针，这个指针会指向当前还没有被变成 Tree 的下一个 List 上的节点。因此如果我们把左子树变成 Tree 以后，current 就要让他指向 List 上的下一个点，也就是中间的这个点了。

算法有一些绕，建议使用几个小数据模拟整个算法的执行过程。\
完整参考程序见：\
[http://www.jiuzhang.com/solution/convert-sorted-list-to-balanced-bst/](http://www.jiuzhang.com/solution/convert-sorted-list-to-balanced-bst/)

**完整算法描述如下：**

1. 首先求得整个list的长度 O(n)
2. 利用 helper 函数进行递归，helper(head, len) 表示把从 head 开始的，长度为len的链表，转换为一个bst并且return。与此同时，把global variable的指针挪到head开始的第 len + 1个listnode上。

那么 convert(head, len) 就可以分为，三个步骤：

1. 把head开头的长度为 len/2的先变成bst，也就是我们的左子树，convert(head, len / 2)。这个时候他顺便会把global variable 挪到第len / 2 + 1的那个node，这个就是我们的root。
2. 然后得到了root之后，把global variable 往下挪一个挪到 第 len/2 + 2个点，也就是右子树开头的那个点，然后调用 convert(global variable, len - len/2 -1)，构造出右子树。
3. 然后把root，左子树，右子树，拼接在一起，return

这个题算法框架就是这样，如果不是很明白的话，建议模拟一个小数据，比如 5个节点的情况。模拟几个数据结合算法的思路来分析，就应该可以明白。这个题的这种解法背下来就好了。
