# 如何用两个栈实现队列

我们已经知道，栈是一个先进后出的数据结构，而队列是一个先进先出的数据结构，那么如何用栈来实现队列呢？这里我们可以用两个栈来实现队列

**思路:**

1. 现在我们已经有了两个栈stack1和stack2，最暴力的做法，当需要往队列中加入元素时，可以往其中一个栈stack2中加入元素，当需要得到队头元素时，只需要将stack2中的元素倒入到stack1中，再取stack1的头元素就可以了，如果是需要删掉队头元素，那么直接pop stack1的栈顶元素就可以了，再将stack1中的元素再倒入到stack2中，以便下一次的加入元素
2. 上面的实现中，我们每取一次队头元素或者删掉队头元素，我们都需要将stack2中的元素先倒入到stack1中，再从stack1中倒回去，每次需要倒两边十分麻烦，那么是否有更加简便一些的方法呢？答案当然是有的，其实当我们将stack2中的元素倒入到stack1中的时候，我们发现stack1中的元素的顺序就是按照队列的先进先出顺序，那么我们不再将stack1中的元素倒入到stack2中，在获取队头元素或者删除队头元素的时候，我们先判断stack1是否为空，如果不为空，从stack1中取即可，如果为空，那么将stack2中的元素倒入到stack1中，每次加入元素的时候都是往stack2中加入元素。

**示例代码**

Java:

```java
public class MyQueue {
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;
    
    public MyQueue() {
        stack1 = new Stack<Integer>();
        stack2 = new Stack<Integer>();
    }
    
    private void stack2ToStack1() {
        while (! stack2.empty()) {
            stack1.push(stack2.peek());
            stack2.pop();
        }
    }
	
    public void push(int number) {
        stack2.push(number);
    }

    public int pop() {
        if (stack1.empty() == true) {
            this.stack2ToStack1();
        }
        return stack1.pop();
    }

    public int top() {
        if (stack1.empty() == true) {
            this.stack2ToStack1();
        }
        return stack1.peek();
    }
}
```

Python:

```python
class MyQueue:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def stack2ToStack1(self):
        while self.stack2:
            self.stack1.append(self.stack2.pop())

    def push(self, number):
        self.stack2.append(number)

    def pop(self):
        if not self.stack1:
            self.stack2ToStack1()
        return self.stack1.pop()

    def top(self):
        if not self.stack1:
            self.stack2ToStack1()
        return self.stack1[-1]
```
