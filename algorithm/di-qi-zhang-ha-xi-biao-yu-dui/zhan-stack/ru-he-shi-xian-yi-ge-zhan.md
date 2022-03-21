# 如何实现一个栈？

#### 什么是栈（Stack）？

栈（stack）是一种采用**后进先出**（LIFO，last in first out）策略的抽象数据结构。比如物流装车，后装的货物先卸，先转的货物后卸。栈在数据结构中的地位很重要，在算法中的应用也很多，比如用于非递归的遍历二叉树，计算逆波兰表达式，等等。

栈一般用一个存储结构（常用数组，偶见链表），存储元素。并用一个指针记录**栈顶位置**。**栈底位置**则是指栈中元素数量为0时的栈顶位置，也即栈开始的位置。\
栈的主要操作：

* `push()`，将新的元素压入栈顶，同时栈顶上升。
* `pop()`，将新的元素弹出栈顶，同时栈顶下降。
* `empty()`，栈是否为空。
* `peek()`，返回栈顶元素。

在各语言的标准库中：

* Java，使用`java.util.Stack`，它是扩展自`Vector`类，支持`push()`，`pop()`，`peek()`，`empty()`，`search()`等操作。
* C++，使用`<stack>`中的`stack`即可，方法类似Java，只不过C++中`peek()`叫做`top()`，而且`pop()`时，返回值为空。
* Python，直接使用`list`，查看栈顶用`[-1]`这样的切片操作，弹出栈顶时用`list.pop()`，压栈时用`list.append()`。

#### 如何自己实现一个栈？

**参见问题：**[**http://www.lintcode.com/en/problem/implement-stack/**](http://www.lintcode.com/en/problem/implement-stack/)\
这里给出一种用`ArrayList`的通用实现方法。（注意：为了将重点放在栈结构上，做了适当简化。该栈仅支持整数类型，若想实现泛型，可用反射机制和object对象传参；此外，可多做安全检查并抛出异常）\
Java:

```java
public class Stack {
    
    List<Integer> array = new ArrayList<Integer>();

    // 压入新元素
    public void push(int x) {
        array.add(x);
    }

    // 栈顶元素弹出
    public void pop() {
        if (!isEmpty()) {
            array.remove(array.size() - 1);
        }
    }

    // 返回栈顶元素
    public int top() {
        return array.get(array.size() - 1);
    }

    // 判断是否是空栈
    public boolean isEmpty() {
        return array.size() == 0;
    }
}

```

Python

```python
class Stack:
    def __init__(self):
        self.array = []
				
    # 压入新元素
    def push(self, x):
        self.array.append(x)
    
    # 栈顶元素出栈
    def pop(self):
        if not self.isEmpty():
            self.array.pop()
	
    # 返回栈顶元素
    def top(self):
        return self.array[-1]

    # 判断是否是空栈
    def isEmpty(self):
        return len(self.array) == 0
```

#### 栈在计算机内存当中的应用

我们在程序运行时，常说的内存中的堆栈，其实就是栈空间。这一段空间存放着程序运行时，产生的各种临时变量、函数调用，一旦这些内容失去其作用域，就会被自动销毁。\
函数调用其实是栈的很好的例子，后调用的函数先结束，所以为了调用函数，所需要的内存结构，栈是再合适不过了。在内存当中，**栈从高地址不断向低地址扩展**，随着程序运行的层层深入，栈顶指针不断指向内存中更低的地址。

相关参考资料：\
[https://blog.csdn.net/liu\_yude/article/details/45058687](https://blog.csdn.net/liu\_yude/article/details/45058687)
