# 图上的宽度优先搜索

BFS 大部分的时候是在图上进行的。

#### 什么是图（Graph）

图在离线数据中的表示方法为 `<E, V>`，E表示 Edge，V 表示 Vertex。也就是说，图是顶点（Vertex）和边（Edge）的集合。

图分为：

* 有向图（Directed Graph）
* 无向图（Undirected Graph）

见下图\
![图片](http://media.jiuzhang.com/markdown/images/3/27/cce99416-316f-11e8-b86d-0242ac110002.jpg)

BFS 在两种图上都适用。另外，树（Tree）也是一种特殊的图。

## 二叉树的BFS vs 图的BFS

二叉树中进行 BFS 和图中进行 BFS 最大的区别就是二叉树中无需使用 HashSet（C++: unordered\_map, Python: dict) 来存储访问过的节点（丢进过 queue 里的节点） 因为二叉树这种数据结构，上下层关系分明，没有环（circle），所以不可能出现一个节点的儿子的儿子是自己的情况。 但是在图中，一个节点的邻居的邻居就可能是自己了。

## 如何定义一个图的数据结构？

有很多种方法可以存储一个图，最常用的莫过于：

1. 邻接矩阵
2. 邻接表

而邻接矩阵因为耗费空间过大，我们通常在工程中都是使用邻接表作为图的存储结构。

### 邻接矩阵

邻接矩阵 Adjacency Matrix

```
[
  [1,0,0,1],
  [0,1,1,0],
  [0,1,1,0],
  [1,0,0,1]
]
```

例如上图表示0号点和3号点有连边。1号点和2号点有连边。\
当然，每个点和自己也是默认有连边的。\
图中的 `0` 表示不连通，`1` 表示连通。\
我们也可以用一个更具体的整数值来表示连边的长度。\
邻接矩阵我们可以直接用一个二维数组表示，如`int[][] matrix;`。这种数据结构因为耗费 O(n^2) 的空间，所以在稀疏图上浪费很大，因此并不常用。

### 邻接表

邻接表 (Adjacency List)

```
[
  [1],
  [0,2,3],
  [1],
  [1]
]
```

这个图表示 0 和 1 之间有连边，1 和 2 之间有连边，1 和 3 之间有连边。即每个点上存储自己有哪些邻居（有哪些连通的点）。\
这种方式下，空间耗费和边数成正比，可以记做 O(m)，m代表边数。m最坏情况下虽然也是 O(n^2)，但是邻接表的存储方式大部分情况下会比邻接矩阵更省空间。

**自定义邻接表**

可以用自定义的类来实现邻接表\
Java:

```java
class DirectedGraphNode {
    int label;
    List<DirectedGraphNode> neighbors;
    ...
}
```

Python:

```python
def DirectedGraphNode:
    def __init__(self, label):
        self.label = label
        self.neighbors = []  # a list of DirectedGraphNode's
		...
```

其中 neighbors 表示和该点连通的点有哪些。

**使用 Map 和 Set（面试时）**

也可以使用 HashMap 和 HashSet 搭配的方式来存储邻接表\
Java:

```java
Map<T, Set<T>> = new HashMap<Integer, HashSet<Integer>>();
```

Python:

```python
# 假设nodes为节点标签的列表:

# 使用了Python中的dictionary comprehension语法
adjacency_list = {x:set() for x in nodes}

# 另一种写法
adjacency_list = {}
for x in nodes:
    adjacency_list[x] = set()
```

其中 T 代表节点类型。通常可能是整数(Integer)。\
这种方式虽然没有上面的方式更加直观和容易理解，但是在面试中比较节约代码量。\
而自定义的方法，更加工程化，所以在面试中如果时间不紧张题目不难的情况下，推荐使用自定义邻接表的方式。
