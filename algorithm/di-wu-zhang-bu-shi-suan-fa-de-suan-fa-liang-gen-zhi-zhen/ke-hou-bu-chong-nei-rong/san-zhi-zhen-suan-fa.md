# 三指针算法

#### 题目描述

将包含0，1，2三种颜色代码的数组按照颜色代码的大小排序。如 `[1,0,1,0,2]` => `[0,0,1,1,2]`。

LintCode 提交地址：[http://www.lintcode.com/problem/sort-colors/](http://www.lintcode.com/problem/sort-colors/)

#### 解法分析

在颜色排序（Sort Color）这个问题中，传统的双指针算法可以这么做：

1. 先用 partition 的方式区分开 0 和 1, 2
2. 再在右半部分区分开 1 和 2

这个算法不可避免的要使用两次 Parition，写两个循环。许多面试官会要求你，能否只 partition 一次，也就是只用一个循环。

用一个循环的方法如下：

[http://www.jiuzhang.com/solution/sort-colors](http://www.jiuzhang.com/solution/sort-colors)

分析一下核心代码部分：\
Java:

```java
public void sortColors(int[] a) {
    if (a == null || a.length <= 1) {
        return;
    }
    
    int pl = 0;
    int pr = a.length - 1;
    int i = 0;
    while (i <= pr) {
        if (a[i] == 0) {
            swap(a, pl, i);
            pl++;
            i++;
        } else if(a[i] == 1) {
            i++;
        } else {
            swap(a, pr, i);
            pr--;
        }
    }
}
```

Python:

```python
def sortColors(a):
    if not a:
        return
    
    pl, pr = 0, len(a)-1
    i = 0
    while i <= pr:
        if a[i] == 0:
            a[pl], a[i] = a[i], a[pl]
            pl += 1
            i += 1
        elif a[i] == 1:
            i += 1
        else:
            a[pr], a[i] = a[i], a[pr]
            pr -= 1
```

pl 和 pr 是传统的双指针，分别代表 0\~pl-1 都已经是 0 了，pr+1\~a.length - 1 都已经是 2 了。\
另一个角度说就是，如果你发现了一个 0 ，就可以和 pl 上的数交换，pl 就可以 ++；如果你发现了一个 2 就可以和 pr 上的数交换 pr 就可以 --。

这样，我们用第三根指针 i 来循环整个数组。如果发现 0，就丢到左边（和 pl 交换，pl++），如果发现 2，就丢到右边（和 pr 交换，pr--），如果发现 1，就不管（i++）

这就是三根指针的算法，两根指针在两边，一根指针扫描所有的数。

这里有一个实现上的小细节，当发现一个 0 丢到左边的时候，i需要++，但是发现一个2 丢到右边的时候，i不用++。原因是，从pr 换过来的数有可能是0, 1或者2，需要继续判断丢到左边还是右边。而从 pl 换过来的数，要么是0要么是1，不需要再往右边丢了。因此这里 i 指针还有一个角度可以理解为，i指针的左侧，都是0和1。

**类似的题**

G家问过一个类似的题：给出 low, high 和一个数组，将数组分为三个部分，< low, >= low & <= high, > high。解法和本题一模一样
