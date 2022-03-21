# 如何用链表实现队列

**定义**

1. 队列为一种先进先出的线性表
2. 只允许在表的一端进行入队，在另一端进行出队操作。在队列中，允许插入的一端叫队尾，允许删除的一端叫队头，即入队只能从队尾入，出队只能从队头出

**思路**

1. 需要两个节点，一个头部节点，也就是dummy节点，它是在加入的第一个元素的前面，也就是dummy.next=第一个元素，这样做是为了方便我们删除元素，还有一个尾部节点，也就是tail节点，表示的是最后一个元素的节点
2. 初始时，tail节点跟dummy节点重合
3. 当我们要加入一个元素时，也就是从队尾中加入一个元素，只需要新建一个值为val的node节点，然后tail.next=node，再移动tail节点到tail.next
4. 当我们需要删除队头元素时，只需要将dummy.next变为dummy.next.next，这样就删掉了第一个元素，这里需要注意的是，如果删掉的是队列中唯一的一个元素，那么需要将tail重新与dummy节点重合
5. 当我们需要得到队头元素而不删除这个元素时，只需要获得dummy.next.val就可以了

**示例代码**

Java:

```java
class QueueNode {
    public int val;
    public QueueNode next;
    public QueueNode(int value) {
        val = value;
    }
}

public class Queue {

    private QueueNode dummy, tail;

    public Queue() {
        dummy = new QueueNode(-1);
        tail = dummy;
    }

    public void enqueue(int val) {
        QueueNode node = new QueueNode(val);
        tail.next = node;
        tail = node;
    }

    public int dequeue() {
        int ele = dummy.next.val;
        dummy.next = dummy.next.next;

        if (dummy.next == null) {
            tail = dummy;// reset
        }
        return ele;
    }

    public int peek() {
        int ele = dummy.next.val;
        return ele;
    }

    public boolean isEmpty() {
        return dummy.next == null;
    }
}
```

Python:

```python
class QueueNode:
    def __init__(self, value):
        self.val = value
        self.next = None


class Queue:
    def __init__(self):
        self.dummy = QueueNode(-1)
        self.tail = self.dummy

    def enqueue(self, val):
        node = QueueNode(val)
        self.tail.next = node
        self.tail = node

    def dequeue(self):
        ele = self.dummy.next.val
        self.dummy.next = self.dummy.next.next

        if not self.dummy.next:
            self.tail = self.dummy
        return ele

    def peek(self):
        return self.dummy.next.val

    def isEmpty(self):
        return self.dummy.next == None
```
