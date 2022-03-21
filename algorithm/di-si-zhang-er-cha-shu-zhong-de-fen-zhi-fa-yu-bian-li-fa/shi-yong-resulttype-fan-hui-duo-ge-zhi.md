# 使用 ResultType 返回多个值

**什么是 ResultType**

通常是我们定义在某个文件内部使用的一个类。比如：\
Java:

```java
class ResultType {
    int maxValue, minValue;
    public ResultType(int maxValue, int minValue) {
        this.maxValue = maxValue;
        this.minValue = minValue;
    }
}
```

**什么时候需要 ResultType**

当我们定义的函数需要返回多个值供调用者计算时，就需要使用 ResultType了。\
所以如果你只是返回一个值就够用的话，就不需要。

**其他语言需要 ResulType 么？**

不是所有的语言都需要自定义 ResultType。\
像 Python 这样的语言，天生支持你返回多个值作为函数的 return value，所以是不需要的。
