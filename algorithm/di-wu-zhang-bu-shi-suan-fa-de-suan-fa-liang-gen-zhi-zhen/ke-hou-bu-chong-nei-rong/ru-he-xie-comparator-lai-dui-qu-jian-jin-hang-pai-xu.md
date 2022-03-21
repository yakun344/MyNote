# 如何写 Comparator 来对区间进行排序？

对于数组、链表、堆等结构，标准库中排序方法，往往是对于基本类型的升序排序，有的时候，不一定能满足我们的要求。例如我们有一些特殊的顺序要求，或待排序的对象类型不是基本类型。此时，就需用到**自定义排序**。自定义排序可以用在很多地方，比如数组排序，堆的排序规则等。

### 一、Java实现自定义排序

Java实现自定义排序，主要有两种方法：

#### 1.实现Comparable接口：

以`Interval`区间为例，在定义该类时，让其实现Comparable，并重写其中的compareTo方法，使得Interval类可以进行大小比较，这样也可实现自定义的排序：

```java
class Interval implements Comparable<Interval> {
    int left, right;
    Interval(int left, int right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int compareTo(Interval o) {
        return this.left - o.left;
    }
}
```

这样，在其他地方就可以直接对Interval对象的大小进行比较。完整的测试方法如下：

```java
import java.util.ArrayList;
import java.util.List;
import static java.util.Collections.sort;

// Interval类如上

public class Main {
    public static void main(String[] args) {
        List<Interval> A = new ArrayList<>();
        A.add(new Interval(1, 7));
        A.add(new Interval(5, 6));
        A.add(new Interval(3, 4));
        System.out.println("Before sort:");
        for (Interval i : A)
            System.out.println("(" + i.left + ", " + i.right + ")");

        sort(A);
        System.out.println("After sort:");
        for (Interval i : A)
            System.out.println("(" + i.left + ", " + i.right + ")");
    }
}
```

输出可以自行观察，不再赘述。

#### 2.定义比较类：

该方法定义一个新的比较类，使其继承自Comparator，并完善其中的compare()方法。在调用时，使用该比较类进行比较。仍以`Interval`为例：

```java
class Interval { // 注意，这里没有继承自Comparable
    int left, right;
    Interval(int left, int right) {
        this.left = left;
        this.right = right;
    }
}

class MyCmp implements Comparator<Interval> {
    @Override
    public int compare(Interval o1, Interval o2) {
        return o1.left - o2.left;
    }
}
```

要对`List<Interval> A`进行排序时，调用如下方法，填入一个比较类的对象即可：

```java
A.sort(new MyCmp());
```

**大家可以对以上各种方法，进行输出调试，也可以修改比较方法，做各种尝试。**

### 二、Python实现自定义排序

Python中也有类似的两种实现方法：

#### 1. 实现`__lt__`方法:

以`Interval`区间为例，在定义该类时，重写其中的`__lt__`方法，使得Interval类可以进行大小比较，这样也可实现自定义的排序：

```python
class Interval:
    def __init__(self, left, right):
        self.left = left
        self.right = right

    # 以下为重写的__lt__方法
    def __lt__(self, other):
        # 当两个Interval比较大小时，直接比较它们的left属性
        return self.left < other.left
```

这样，在其他地方就可以直接对Interval对象的大小进行比较。完整的测试方法如下：

```python
# Interval类如上

if __name__ == "__main__":
    A = []
    A.append(Interval(1, 7))
    A.append(Interval(5, 6))
    A.append(Interval(3, 4))
    print("Before sort:")
    for i in A:
        print("({},{})".format(i.left, i.right))

    # 由于定义了__lt__，方法，此处可以直接调用sort方法进行升序排序
    A.sort()

    print("After sort:")
    for i in A:
        print("({},{})".format(i.left, i.right))
```

输出可以自行观察，不再赘述。

#### 2. 定义key函数

可以给sort方法传入一个key函数，表示按照什么标准来对元素进行排序，仍以上面的例子为例:

```python
# 要传给sort函数的key方法，表示按照interval.left进行排序
def IntervalKey(interval):
    return interval.left 

A = []
A.append(Interval(1, 7))
A.append(Interval(5, 6))
A.append(Interval(3, 4))
A.sort(key=IntervalKey)
```

如果要实现多关键字排序怎么办（比如left小的排前面，如果left相等的话，那么right小的排前面）?\
答案是返回一个tuple作为key。在Python中, 两个tuple比较大小时会先比较第一个元素，返回第一个元素较小的那个，如果第一个元素相等，再比较第二个元素，返回较小的那个，以此类推......\
我们可以利用这一点来实现多关键字排序，具体可以参考下面的例子：

```python
class Interval:
    def __init__(self, left, right):
        self.left, self.right = left, right
 
    # 打印函数，用于直接print一个Interval对象
    def __repr__(self):
        return "Interval(%d, %d)" % (self.left, self.right)


data = [(3, 2), (3, 1), (2, 7), (1, 5), (2, 6), (1, 7)]
intervals = [Interval(left, right) for left, right in data]

print(sorted(intervals, key=lambda i: (i.left, i.right)))  # 先按x从小到大排，再按y从小到大排
# 结果: [Interval(1, 5), Interval(1, 7), Interval(2, 6), Interval(2, 7), Interval(3, 1), Interval(3, 2)]

print(sorted(intervals, key=lambda i: (-i.left, i.right)))  # 先按x从大到小排，再按y从小到大排
# 结果: [Interval(3, 1), Interval(3, 2), Interval(2, 6), Interval(2, 7), Interval(1, 5), Interval(1, 7)]

print(sorted(intervals, key=lambda i: (i.right, i.left)))  # 先按y从小到大排，再按x从小到大排
# 结果: [Interval(3, 1), Interval(3, 2), Interval(1, 5), Interval(2, 6), Interval(1, 7), Interval(2, 7)]

print(sorted(intervals, key=lambda i: (-i.right, i.left)))  # 先按y从大到小排，再按x从小到大排
# 结果: [Interval(1, 7), Interval(2, 7), Interval(2, 6), Interval(1, 5), Interval(3, 2), Interval(3, 1)]
```
