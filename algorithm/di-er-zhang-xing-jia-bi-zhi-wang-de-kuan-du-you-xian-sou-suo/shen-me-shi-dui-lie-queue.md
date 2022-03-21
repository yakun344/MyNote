# 什么是队列（Queue）

#### 什么是队列（Queue）？

队列（queue）是一种采用**先进先出**（FIFO，first in first out）策略的抽象数据结构。比如生活中排队，总是按照先来的先服务，后来的后服务。队列在数据结构中举足轻重，其在算法中应用广泛，**最常用的就是在宽度优先搜索(BFS）中，记录待扩展的节点**。

队列内部存储元素的方式，一般有两种，**数组**（array）和**链表**（linked list）。两者的最主要区别是：

* 数组对**随机访问**有较好性能。
* 链表对**插入**和**删除**元素有较好性能。

在各语言的标准库中：

* Java常用的队列包括如下几种：\
  `ArrayDeque`：数组存储。实现Deque接口，而Deque是Queue接口的子接口，代表**双端队列**（double-ended queue）。\
  `LinkedList`：链表存储。实现List接口和Duque接口，不仅可做队列，还可以作为双端队列，或栈（stack）来使用。
* C++中，使用\<queue>中的`queue`模板类，模板需两个参数，元素类型和容器类型，元素类型必要，而容器类型可选，默认`deque`，可改用`list`（链表）类型。
* Python中，使用`collections.deque`，双端队列。

#### 如何自己用数组实现一个队列？

队列的主要操作有：

* `add()`队尾追加元素
* `poll()`弹出队首元素
* `size()`返回队列长度
* `empty()`判断队列为空

下面利用Java的`ArrayList`和一个头指针实现一个简单的队列。（注意：为了将重点放在实现队列上，做了适当简化。该队列仅支持整数类型，若想实现泛型，可用反射机制和object对象传参；此外，可多做安全检查并抛出异常）\
Java:

```java
class MyQueue {
    private ArrayList<Integer> elements;  // 用ArrayList存储队列内部元素
    private int pointer;  // 表示队头的位置

    // 队列初始化
    public MyQueue() {
        this.elements = new ArrayList<>();
        pointer = 0;
    }

    // 获取队列中元素个数
    public int size() {
        return this.elements.size() - pointer;
    }

    // 判断队列是否为空
    public boolean empty() {
        return this.size() == 0;
    }

    // 在队尾添加一个元素
    public void add(Integer e) {
        this.elements.add(e);
    }

    // 弹出队首元素，如果为空则返回null
    public Integer poll() {
        if (this.empty()) {
            return null;
        }
        return this.elements.get(pointer++);
    }
}
```

Python:

```python
class MyQueue:
    # 队列初始化
    def __init__(self):
        self.elements = []  # 用list存储队列元素
        self.pointer = 0    # 队头位置

    # 获取队列中元素个数
    def size(self):
        return len(self.elements)-pointer
    
    # 判断队列是否为空
    def empty(self):
        return self.size() == 0

    # 在队尾添加一个元素
    def add(self, e):
        self.elements.append(e)

    # 弹出队首元素，如果为空则返回None
    def poll(self):
        if self.empty():
            return None
        pointer += 1
        return self.elements[pointer-1]
```

#### 队列在工业界的应用

队列可用于实现消息队列（message queue），以完成异步（asynchronous）任务。

“消息”是计算机间传送的数据，可以只包含文本；也可复杂到包含嵌入对象。当消息“生产”和“消费”的速度不一致时，就需要消息队列，临时保存那些已经发送而并未接收的消息。例如集体打包调度，服务器繁忙时的任务处理，事件驱动等等。

常用的消息队列实现包括[RabbitMQ](http://www.rabbitmq.com)，[ZeroMQ](http://zeromq.org)等等。

更多消息队列的参考资料：\
[为什么需要消息队列，及使用消息队列的好处？](https://blog.csdn.net/qq\_39470733/article/details/80576013)\
[RabbitMQ的应用场景以及基本原理介绍](http://blog.csdn.net/whoamiyang/article/details/54954780)
