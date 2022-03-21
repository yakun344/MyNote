# 相向双指针

相向双指针，指的是在算法的一开始，两根指针分别位于数组/字符串的两端，并相向行走。如我们在小学的时候经常遇到的问题：

> 小明和小红分别在铁轨A站和B站相向而行，小红的速度为 1m/s, 小明的速度为 2m/s，A站和B站相距 1km。\
> 请问 ... 他们什么时候被火车撞死？（逃

一个典型的相向双指针问题就是翻转字符串的问题。在第二节课中我们学到的三步翻转法，就是一个典型的例子。

**用 while 循环的写法：**

Java:

```java
public void reverse(char[] s) {
    int left = 0, right = s.length - 1;
    while (left < right) {
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--; 
    }
}
```

Python:

```python
"""
@param s: a list of characters
"""
def reverse(s):
    left, right = 0, len(s)-1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

**用 for 循环的写法：**

Java:

```java
public void reverse(char[] s) {
    for (int i = 0, j = s.length - 1; i < j; i++, j--) {
        char temp = s[i];
        s[i] = s[j];
        s[j] = temp;
    }
}
```

Python: 无\
没有掌握三步翻转法的同学请回到第二章节的课后补充内容中进行学习。
