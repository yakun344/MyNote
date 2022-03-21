# 如何用两个队列实现一个栈？

#### 算法步骤

队列的知识请看：[http://www.jiuzhang.com/tutorial/algorithm/391](http://www.jiuzhang.com/tutorial/algorithm/391)\
两个队列实现一个栈，其实并没有什么优雅的办法。就看大家怎么去写这个东西了。

* 构造的时候，初始化两个队列，`queue1`，`queue2`。`queue1`主要用来存储，`queue2`则主要用来帮助`queue1`弹出元素以及访问栈顶。
* push：将元素推入`queue1`当中。
* pop：注意要弹出的元素在`queue1`末端，故将`queue1`中元素弹出，并直接推入`queue2`，当`queue1`只剩一个元素时，把它pop出来，并作为结果。而后交换两个队列。
* top：类似pop，不过不扔掉`queue1`中最后一个元素，而是把它也推入`queue2`当中。
* isEmpty：判断`queue1`是否为空即可。

#### 参考代码

Java:

```java
public class Stack {
    public Queue<Integer> queue1 = new LinkedList<Integer>();
    public Queue<Integer> queue2 = new LinkedList<Integer>();
    
    // 将queue1中元素移入queue2,留下最后一个。
    public void moveItems() {
        while (queue1.size() != 1) {
            queue2.offer(queue1.poll());
        }
    }
    
    // 交换两个队列
    public void swapQueues() {
        Queue<Integer> temp = queue1;
        queue1 = queue2;
        queue2 = temp;
    }

    public void push(int x) {
        queue1.offer(x);
    }

    public void pop() {
        moveItems();
        queue1.poll();
        swapQueues();
    }

    public int top() {
        moveItems();
        int item = queue1.poll();
        swapQueues();
        queue1.offer(item);
        return item;
    }

    public boolean isEmpty() {
        return queue1.isEmpty();
    }
}
```

Python:

```python
from collections import deque


class Stack:
    def __init__(self):
        self.queue1 = deque()
        self.queue2 = deque()

    # 将queue1中元素移入queue2,留下最后一个。
    def moveItems(self):
        while len(self.queue1) != 1:
            self.queue2.append(self.queue1.popleft())

    def swapQueues(self):
        self.queue1, self.queue2 = self.queue2, self.queue1

    def push(self, x):
        self.queue1.append(x)

    def pop(self):
        self.moveItems()
        self.queue1.popleft()
        self.swapQueues()

    def top(self):
        self.moveItems()
        item = self.queue1.popleft()
        self.swapQueues()
        self.queue1.append(item)
        return item

    def isEmpty(self):
        return len(self.queue1) == 0
```

#### 相关题目

[http://www.lintcode.com/zh-cn/problem/implement-stack-by-two-queues/](http://www.lintcode.com/zh-cn/problem/implement-stack-by-two-queues/)
