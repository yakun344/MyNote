# 第一章课后补充内容

## KMP算法的替代品：Rabin Karp

在解决 strstr 这个问题时，KMP 算法学起来比价费劲，不好理解，且对于其他的算法面试问题没有什么帮助。这里推荐大家学习一个替代品 —— Rabin Karp Algorithm。这个算法同样可以达到 O(n + m) 的线性时间复杂度，而且其利用到的哈希函数基本实现原理是必须掌握的面试算法知识点。

strstr-ii

## 最长回文子串标准算法 Manacher's Algorithm

网上已经有比较好的参考资料，不再赘述。该算法时间复杂度证明比较困难，如果有空的话，可以纯粹背下来代码就行。

* [中文版](https://segmentfault.com/a/1190000003914228)
* [英文版](https://www.geeksforgeeks.org/manachers-algorithm-linear-time-longest-palindromic-substring-part-1/)

## 简单位运算操作



#### 什么是位运算

程序中所有数在内存中都以**二进制**形式储存。**位运算**（bit operation）就是直接对整数在内存中的二进制位进行操作。使用的主要目的是节约内存，加速运行，以及对内存要求苛刻时使用。\
位运算在面试中的“初衷”是考察面试者的基本功，但不幸，位运算所考察的，大部分是知道就知道, 不知道不知道。

#### 按位与操作

主要讲解“按位与”（and）操作，操作符为`&`。\
将A和B的二进制表示的每一位进行与操作，只有两个对应的二进制位都为1时，结果位才为1，否则为0。

```
1 & 1 = 1
1 & 0 = 0
0 & 1 = 0
0 & 0 = 0
```

例如：\
下面 `(x)y`表示 `x` 是 `y` 进制。\
A = `(10)10` = `(001010)2` （注意高位全是0）\
B = `(44)10` = `(101100)2`\
A & B = `10 & 44` = `001010 & 101100` = `(001000)2` = `(8)10`

具体的Java程序如下：

```java
int a = 10 & 44; // a的值是8
```

Python版本类似:

```python
a = 10 & 44
```

#### 按位与相关题目

计算一个32位整数的二进制表示中有多少个`1`（[http://www.lintcode.com/zh-cn/problem/count-1-in-binary/](http://www.lintcode.com/zh-cn/problem/count-1-in-binary/)）。\
例如`32`(100000)，返回`1`；`5`(101)，返回`2`；`1023`(1111111111)，返回`10`。

**算法思路：**

用1不断左移（左移操作可参见下文**其他操作**，每次和num做**按位与**看是否为0，不是0的话说明这一位是1。左移32次后停止。代码如下：\
Java:

```java
public class Solution {
    public int countOnes(int num) {
        int count = 0;
        for(int i = 0 ; i < 32; i++) {
            if((num & (1<<i)) != 0) {
                count++;
            }
        }
        return count;
    }
}
```

Python:

```python
class Solution:
    def countOnes(self, num):
        count = 0
        for i in range(32):
            if (num & (1 << i)) != 0:
                count += 1
        return count
```

**Q：这方法有啥问题没有？**\
**A**：几乎没有，但是不管这个数是几，总要循环32次，稍微有点费时，而且看上去很蠢笨。

**更精妙的算法**

不断用num和num-1做按位与，结果直接赋给num。只要num不为0，就重复该过程。最后返回以上过程的次数即可。代码如下：\
Java:

```java
public class Solution {
    public int countOnes(int num) {
        int count = 0;
        while (num != 0) {
            num &= num - 1;
            count++;
        }
        return count;
    }
}
```

Python:

```python
class Solution:
    def countOnes(self, num):
        if num < 0:
            # Python的整数是无限长的, -1在Java/C++的32位整数中为: 11...11111 (32个1)
            # 但是在Python中为: ...1111111111111 (无限个1)
            # 因此在遇到负数时要先截断为32位
            num &= (1 << 32)-1
        count = 0
        while num != 0:
            num &= num - 1
            count += 1
        return count
```

**Q：这为啥可以？**\
**A**：其实原理很简单，先说结论：**每一次`num &= num - 1`会使得`num`最低位`1`变为`0`**。\
例如12，二进制表示为`1100`，减1后的二进制表示为`1011`。注意到了吗，减1后，最低位1变成了0，而最低位1后面的0全变成了1，高位不变。这样和原数按位与后，就只有最低位1发生了变化。所以该过程循环了多少次，就说明抹掉了多少个1。这对于其余正整数也是适用的。

但是要注意的是，Python中的整数是无限长的，负数的二进制表示中会有无限个前导1，因此要先将负数截断至32位。

#### 位运算其他操作

其他的位运算操作**同样重要**，可参考[九章算法——位运算入门教程](http://www.jiuzhang.com/tutorial/bit-manipulation/72)自行学习，这里附上各自链接：

* [左移操作 `A << B`](http://www.jiuzhang.com/tutorial/bit-manipulation/74)
* [右移操作 `A >> B`，`A >>> B`](http://www.jiuzhang.com/tutorial/bit-manipulation/75)
* [按位或操作 `A | B`](http://www.jiuzhang.com/tutorial/bit-manipulation/77)
* [按位非操作 `~A`](http://www.jiuzhang.com/tutorial/bit-manipulation/78)
* [按位异或操作 `A^B`](http://www.jiuzhang.com/tutorial/bit-manipulation/79)

## 子数组与前缀和
