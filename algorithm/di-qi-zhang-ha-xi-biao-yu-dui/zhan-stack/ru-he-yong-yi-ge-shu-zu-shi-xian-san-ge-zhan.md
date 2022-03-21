# 如何用一个数组实现三个栈？

#### 题目描述

用一个数组实现三个栈。你可以假设这三个栈都一样大并且足够大。你不需要担心如果一个栈满了之后怎么办。\
详见：[http://www.lintcode.com/zh-cn/problem/implement-three-stacks-by-single-array/](http://www.lintcode.com/zh-cn/problem/implement-three-stacks-by-single-array/)

#### 算法描述

这道题的本质是把**数组索引**当作地址，用**链表**来实现栈。数组`buffer`中的每一个元素，并不能单单是简单的int类型，而是一个链表中的节点，它包含值`value`，栈中向栈底方向的之前元素索引`prev`，向栈顶方向的后来元素索引`next`。\
在该三栈数据结构中，要记录三个栈顶指针`stackPointer`，也就是三个栈顶所在的数组索引，通过这三个栈顶节点，能够用`prev`找到整串栈。\
此外还要用`indexUsed`记录整个数组中的所用的索引数。其实也就是下一次`push`的时候，向数组的`indexUsed`位置存储。

**具体操作**：

* 构造：要初始化`stackPointer`为`3个-1`,表示没有;`indexUsed=0`;`buffer`为一个长度为三倍栈大小的数组。
* push：要把新结点new在`buffer[indexUsed]`，同时修改该栈的`stackPointer`，`indexUsed`自增。注意修改当前栈顶结点`prev`和之前栈顶结点的`next`索引。
* peek：只需要返回`buffer`中对应的`stackPointer`即可。
* isEmpty：只需判断`stackPointer`是否为-1。
* pop：pop的操作较为复杂，因为有三个栈，所以栈顶不一定在数组尾端，pop掉栈顶之后，**数组中可能存在空洞**。而这个空洞又很难`push`入元素。所以，解决方法是，当要`pop`的元素不在数组尾端（即`indexUsed-1`）时，交换这两个元素。**不过一定要注意，交换的时候，要注意修改这两个元素之前、之后结点的`prev`和`next`指针，使得链表仍然是正确的**，事实上这就是结点中`next`的作用——为了找到之后结点并修改它的`prev`。在交换时，一种很特殊的情况是栈顶节点刚好是数组尾端元素的后继节点，这时需要做特殊处理。在交换完成后，就可以删掉数组尾端元素，并修改相应的`stackPointer`、`indexUsed`和新栈顶的`next`。

#### 参考代码

**pop操作确实非常复杂，编写时如履薄冰。这里给出一种比较好的写法。**\
Java:

```java
public class ThreeStacks {
    public int stackSize;
    public int indexUsed;
    public int[] stackPointer;
    public StackNode[] buffer;

    public ThreeStacks(int size) {
        // do intialization if necessary
        stackSize = size;
        stackPointer = new int[3];
        for (int i = 0; i < 3; ++i)
            stackPointer[i] = -1;
        indexUsed = 0;
        buffer = new StackNode[stackSize * 3];
    }

    public void push(int stackNum, int value) {
        // Write your code here
        // Push value into stackNum stack
        int lastIndex = stackPointer[stackNum];
        stackPointer[stackNum] = indexUsed;
        indexUsed++;
        buffer[stackPointer[stackNum]] = new StackNode(lastIndex, value, -1);
        if (lastIndex != -1) {
            buffer[lastIndex].next = stackPointer[stackNum];
        }
    }

    public int pop(int stackNum) {
        // Write your code here
        // Pop and return the top element from stackNum stack
        int value = buffer[stackPointer[stackNum]].value;
        int lastIndex = stackPointer[stackNum];
        if (lastIndex != indexUsed - 1)
            swap(lastIndex, indexUsed - 1, stackNum);

        stackPointer[stackNum] = buffer[stackPointer[stackNum]].prev;
        if (stackPointer[stackNum] != -1)
            buffer[stackPointer[stackNum]].next = -1;

        buffer[indexUsed-1] = null;
        indexUsed --;
        return value;
    }

    public int peek(int stackNum) {
        // Write your code here
        // Return the top element
        return buffer[stackPointer[stackNum]].value;
    }

    public boolean isEmpty(int stackNum) {
        // Write your code here
        return stackPointer[stackNum] == -1;
    }

    public void swap(int lastIndex, int topIndex, int stackNum) {
        if (buffer[lastIndex].prev == topIndex) {
            int tmp = buffer[lastIndex].value;
            buffer[lastIndex].value = buffer[topIndex].value;
            buffer[topIndex].value = tmp;
            int tp = buffer[topIndex].prev;
            if (tp != -1) {
                buffer[tp].next = lastIndex;
            }
            buffer[lastIndex].prev = tp;
            buffer[lastIndex].next = topIndex;
            buffer[topIndex].prev = lastIndex;
            buffer[topIndex].next = -1;
            stackPointer[stackNum] = topIndex;
            return;
        }

        int lp = buffer[lastIndex].prev;
        if (lp != -1)
            buffer[lp].next = topIndex;

        int tp = buffer[topIndex].prev;
        if (tp != -1)
            buffer[tp].next = lastIndex;

        int tn = buffer[topIndex].next;
        if (tn != -1)
            buffer[tn].prev = lastIndex;
        else {
            for (int i = 0; i < 3; ++i)
                if (stackPointer[i] == topIndex)
                    stackPointer[i] = lastIndex;
        }

        StackNode tmp = buffer[lastIndex];
        buffer[lastIndex] = buffer[topIndex];
        buffer[topIndex] = tmp;
        stackPointer[stackNum] = topIndex;
    }
}

class StackNode {
    public int prev, next;
    public int value;
    public StackNode(int p, int v, int n) {
        value = v;
        prev = p;
        next = n;
    }
}
```

Python:

```python
class ThreeStacks:
    def __init__(self, size):
        self.stackSize = size
        self.stackPointer = [-1, -1, -1]
        self.indexUsed = 0
        self.buffer = [StackNode(-1, -1, -1) for _ in range(size*3)]

    def push(self, stackNum, value):
        lastIndex = self.stackPointer[stackNum]
        self.stackPointer[stackNum] = self.indexUsed
        self.indexUsed += 1
        self.buffer[self.stackPointer[stackNum]] = StackNode(lastIndex, value, -1)
        if lastIndex != -1:
            self.buffer[lastIndex].next = self.stackPointer[stackNum]

    def pop(self, stackNum):
        value = self.buffer[self.stackPointer[stackNum]].value
        lastIndex = self.stackPointer[stackNum]
        if lastIndex != self.indexUsed - 1:
            self.swap(lastIndex, self.indexUsed-1, stackNum)

        self.stackPointer[stackNum] = self.buffer[self.stackPointer[stackNum]].prev
        if self.stackPointer[stackNum] != -1:
            self.buffer[self.stackPointer[stackNum]].next = -1

        self.buffer[self.indexUsed-1] = None
        self.indexUsed -= 1
        return value

    def peek(self, stackNum):
        return self.buffer[self.stackPointer[stackNum]].value

    def isEmpty(self, stackNum):
        return self.stackPointer[stackNum] == -1

    def swap(self, lastIndex, topIndex, stackNum):
        if self.buffer[lastIndex].prev == topIndex:
            self.buffer[lastIndex].value, self.buffer[topIndex].value = self.buffer[topIndex].value, self.buffer[lastIndex].value
            tp = self.buffer[topIndex].prev
            if tp != -1:
                self.buffer[tp].next = lastIndex
            self.buffer[lastIndex].prev = tp
            self.buffer[lastIndex].next = topIndex
            self.buffer[topIndex].prev = lastIndex
            self.buffer[topIndex].next = -1
            self.stackPointer[stackNum] = topIndex
            return

        lp = self.buffer[lastIndex].prev
        if lp != -1:
            self.buffer[lp].next = topIndex

        tp = self.buffer[topIndex].prev
        if tp != -1:
            self.buffer[tp].next = lastIndex

        tn = self.buffer[topIndex].next
        if tn != -1:
            self.buffer[tn].prev = lastIndex
        else:
            for i in range(3):
                if self.stackPointer[i] == topIndex:
                    self.stackPointer[i] = lastIndex

        self.buffer[lastIndex], self.buffer[topIndex] = self.buffer[topIndex], self.buffer[lastIndex]
        self.stackPointer[stackNum] = topIndex


class StackNode:
    def __init__(self, p, v, n):
        self.value = v
        self.prev = p
        self.next = n
```
