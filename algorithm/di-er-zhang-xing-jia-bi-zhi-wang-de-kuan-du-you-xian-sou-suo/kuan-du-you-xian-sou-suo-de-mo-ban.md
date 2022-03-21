# 宽度优先搜索的模板

宽度优先搜索有很多种实现方法，这里为了大家记忆方便和教学的方便，我们只介绍最实用的一种方法，即使用一个队列的方法。\
这种方法也根据 BFS 时的需求不同，有两个版本，即需要分层遍历的版本和不需要分层遍历的版本。

**什么时候需要分层遍历？**

1. 如果问题需要你区分开不同层级的结果信息，如 [二叉树的分层遍历 Binary Tree Level Order Traversal](http://www.lintcode.com/problem/binary-tree-level-order-traversal/)
2. 简单图最短路径问题，如 [单词接龙 Word Ladder](http://www.lintcode.com/problem/word-ladder/)

## 无需分层遍历的宽度优先搜索

Java:

```java
// T 指代任何你希望存储的类型
Queue<T> queue = new LinkedList<>();
Set<T> set = new HashSet<>();

set.add(start);
queue.offer(start);
while (!queue.isEmpty()) {
    T head = queue.poll();
    for (T neighbor : head.neighbors) {
        if (!set.contains(neighbor)) {
            set.add(neighbor);
            queue.offer(neighbor);
        }
    }
}
```

Python:

```python
from collections import deque

queue = deque()
seen = set()  #等价于Java版本中的set

seen.add(start)
queue.append(start)
while len(queue):
    head = queue.popleft()
    for neighbor in head.neighbors:
        if neighbor not in seen:
            seen.add(neighbor)
            queue.append(neighbor)
```

C++:

```cpp
queue<T> que;
unordered_set<T, Hasher> seen;

seen.insert(start);
que.push(start);
while (!que.empty()) {
  const T &head = que.front();
  que.pop();
  for (const T &neighbor : head.neighbors) {
    if (!seen.count(neighbor)) {
      seen.insert(neighbor);
      que.push(neighbor);
    }
  }
}
```

上述代码中：

* neighbor 表示从某个点 head 出发，可以走到的下一层的节点。
* set/seen 存储已经访问过的节点（已经丢到 queue 里去过的节点）
* queue 存储等待被拓展到下一层的节点
* set/seen 与 queue 是一对好基友，无时无刻都一起出现，往 queue 里新增一个节点，就要同时丢到 set 里。

## 需要分层遍历的宽度搜先搜索



Java:

```java
// T 指代任何你希望存储的类型
Queue<T> queue = new LinkedList<>();
Set<T> set = new HashSet<>();

set.add(start);
queue.offer(start);
while (!queue.isEmpty()) {
    int size = queue.size();
    for (int i = 0; i < size; i++) {
        T head = queue.poll();
        for (T neighbor : head.neighbors) {
            if (!set.contains(neighbor)) {
                set.add(neighbor);
                queue.offer(neighbor);
            }
        }
    }
}
```

Python：

```python
from collections import deque

queue = deque()
seen = set()

seen.add(start)
queue.append(start)
while len(queue):
    size = len(queue)
    for _ in range(size):
        head = queue.popleft()
        for neighbor in head.neighbors:
            if neighbor not in seen:
                seen.add(neighbor)
                queue.append(neighbor)
```

C++:

```cpp
queue<T> que;
unordered_set<T, Hasher> seen;

seen.insert(start);
que.push(start);
while (!que.empty()) {
  int size = que.size();
  for (int i = 0; i < size; ++i) {
    const T &head = que.front();
    que.pop();
    for (const T &neighbor : head.neighbors) {
      if (!seen.count(neighbor)) {
        seen.insert(neighbor);
        que.push(neighbor);
      }
    }
  }
}
```

上述代码中：

* size = queue.size() 是一个必须的步骤。如果在 for 循环中使用 `for (int i = 0; i < queue.size(); i++)` 会出错，因为 queue.size() 是一个动态变化的值。所以必须先把当前层一共有多少个节点存在局部变量 size 中，才不会把下一层的节点也在当前层进行扩展。
