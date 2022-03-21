# 两数之差问题

#### 问题描述

在一个数组中，求出满足两个数之差等于 target 的那一对数。返回他们的下标。

LintCode 练习地址：\
[http://www.lintcode.com/problem/two-sum-difference-equals-to-target/](http://www.lintcode.com/problem/two-sum-difference-equals-to-target/)

#### 问题分析

作为两数之和的一个 Follow up 问题，在两数之和被问烂了以后，两数之差是经常出现的一个面试问题。\
我们可以先尝试一下两数之和的方法，发现并不奏效，因为即便在数组已经排好序的前提下，nums\[i] - nums\[j] 与 target 之间的关系并不能决定我们淘汰掉 nums\[i] 或者 nums\[j]。

那么我们尝试一下将两根指针同向前进而不是相向而行，在 i 指针指向 nums\[i] 的时候，j 指针指向第一个使得 nums\[j] - nums\[i] >= |target| 的下标 j：

1. 如果 nums\[j] - nums\[i] == |target|，那么就找到答案
2. 否则的话，我们就尝试挪动 i，让 i 向右挪动一位 => i++
3. 此时我们也同时将 j 向右挪动，直到 nums\[j] - nums\[i] >= |target|

可以知道，由于 j 的挪动不会从头开始，而是一直递增的往下挪动，那么这个时候，i 和 j 之间的两个循环的就不是累乘关系而是叠加关系。

#### 核心代码

Java:

```java
Arrays.sort(nums);
target = Math.abs(target)

// 下面这个部分的代码是 O(n) 的
int j = 1;
for (int i = 0; i < nums.length; i++) {
    while (j < nums.length && nums[j] - nums[i] < target) {
        j++;
    }
    if (nums[j] - nums[i] == target) {
        // 找到答案!
    }
}
```

Python:

```python
nums.sort()
target = abs(target)

j = 1
for i in range(len(nums)):
    while j < len(nums) and nums[j]-nums[i] < target:
        j += 1
    if nums[j]-nums[i] == target:
        # 找到答案
```

[完整参考代码](http://www.jiuzhang.com/solution/two-sum-difference-equals-to-target)

#### 相似问题

G家的一个相似问题：找到一个数组中有多少对二元组，他们的平方差 < target（target 为正整数）。\
我们可以用类似放的方法来解决，首先将数组的每个数进行平方，那么问题就变成了有多少对两数之差 < target。\
然后走一遍上面的这个流程，当找到一对 nums\[j] - nums\[i] >= target 的时候，就相当于一口气发现了：

```
nums[i + 1] - nums[i]
nums[i + 2] - nums[i]
...
nums[j - 1] - nums[i]
```

一共 `j - i - 1` 对满足要求的二元组。累加这个计数，然后挪动 i 的位置 +1 即可。
