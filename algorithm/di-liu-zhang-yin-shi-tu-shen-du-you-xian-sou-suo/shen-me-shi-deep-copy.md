# 什么是 Deep Copy？

在 Subsets 的Java实现中，我们用到了如下的代码记录每一个找到的集合\
Java:

```java
results.add(new ArrayList<Integer>(subset));
```

事实上，这句话是调用了 ArrayList 的一个构造函数（Constructor），这个构造函数可以接受另外一个 ArrayList 作为其初始化的状态。

这种方式，我们叫它`深度拷贝`（Deep Copy），又叫做硬拷贝（Hard Copy）或者克隆（Clone，名字多得老纸记不住啊）。与之对应的就有 软拷贝（Soft copy），又名引用拷贝（Reference Copy）。

在Python中，也有如下类似代码:\
Python:

```python
results.append(list(S))  # S is a set()
```

这新建了一个list，list的构造接受一个Iterable对象作为参数，并将该对象内的元素按顺序添加到新建的list中。这也是一次Deep Copy。

**不使用 Deep copy 会怎样呢？**

我们来看看不使用 Deep copy 会怎样：\
Java:

```java
List<Integer> subset = new ArrayList<>();
subset.add(1);  // 此时 subset 是 [1]

List<List<Integer>> results = new ArrayList<>();
results.add(subset);  // 此时 results 是 [[1]]

subset.add(2);  // 此时 subset 是 [1,2]
results.add(subset);  // 此时你以为 results 是 [[1], [1,2]] 而事实上他是[[1,2], [1,2]]

subset.add(3);  // 此时 results 里是 [[1,2,3], [1,2,3]]
```

Python:

```python
subset = []
subset.append(1) # 此时subset是[1]

results = []
results.append(subset)  # 此时results是[[1]]

subset.append(2)  # 此时subset是[1, 2]
results.append(subset)  # 此时你以为results是[[1], [1,2]]而事实上他是[[1,2], [1,2]]

subset.append(3)  # 此时results里是[[1,2,3], [1,2,3]]
```

我们看到由于每一次 results.add 都是加入了相同的变量 subset，因此如果 subset 有变化，那么 result 里的记录就会同步的发生变化。原因是 results.add(subset) 加入的是 subset 的 reference，也就是 subset 在内存中的地址。那么事实上，当 results 里有两个 subset 的时候，相当于存储的是两个内存地址，而这两个内存地址又是一样的，才会导致如果这个内存地址里存的东西发生了变化，results 看起来就每个元素都发生了变化。

**参数中引用传递**

来看这段代码\
Java:

```java
public void func(List<Integer> subset) {
    subset.add(1);
}
public void main() {
    List<Integer> subset = new ArrayList<>();
    // 此时 subset 是 []
    func(subset);
    // 此时 subset 就是 [1] 了
}
```

Python:

```python
def func(subset):  # subset is a list
    subset.append(1)

def main():
    subset = []
    # 此时subset是[]
    func(subset)
    # 此时subset就是[1]了
```

可能你会奇怪，不是说修改参数不会影响到函数之外的参数么？也就是：\
Java:

```java
public void func(int x) {
    x = x + 1;
}
public void main() {
    int x = 0;
    func(x);
    // 此时 x 仍然是 0
}
```

Python:

```python
def func(x):
    x = x+1
		
def main():
    int x = 0
    func(x)
    # 此时x仍然是0
```

上面两者的区别在于，人们习惯性的认为 `subset.add` 和 `x = x + 1` 都是对参数进行了`修改`。而事实上，x = x + 1 确实是对参数进行了修改，这个修改只在函数func的局部有效，出了func回到main就失效了。而 subset.add 并没有修改 subset 这个参数本身，而只是在 subset 所指向的内存空间中增加了一个新的元素，这个操作是永久性的，不是临时的，是全局有效的，不是局部有效的。那么怎么样才是对 subset 这个参数进行了修改呢？比如：\
Java:

```java
public void func(List<Integer> subset) {
    subset = new ArrayList<Integer>();
    subset.add(1);
}
public void main() {
    List<Integer> subset = new ArrayList<>();
    // 此时 subset 是 []
    func(subset);
    // 此时 subset 还是 []
}
```

Python:

```python
def func(subset):
    subset = list(subset)
    subset.append(1)

def main():
    subset = []
    # 此时 subset 是 []
    func(subset)
    # 此时 subset 还是 []
}
```

我们可以看到如果你的修改操作是 `参数x = ...` 那么这才是对参数x的修改，而 `参数x.call_method()` 并不是对参数 x 本身的修改。
