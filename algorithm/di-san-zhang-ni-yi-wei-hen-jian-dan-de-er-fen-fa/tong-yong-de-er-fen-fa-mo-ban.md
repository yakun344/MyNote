# 通用的二分法模板

## 模板代码剖析

#### Java 版本

```java
public class Solution {
    /**
     * @param A an integer array sorted in ascending order
     * @param target an integer
     * @return an integer
     */
    public int findPosition(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        // 要点1: start + 1 < end
        while (start + 1 < end) {
	    // 要点2：start + (end - start) / 2
            int mid = start + (end - start) / 2;
            // 要点3：=, <, > 分开讨论，mid 不+1也不-1
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        // 要点4: 循环结束后，单独处理start和end
        if (nums[start] == target) {
            return start;
        }
        if (nums[end] == target) {
            return end;
        }
        return -1;
    }
}
```

其他语言的参考代码请见：\
[http://www.jiuzhang.com/solutions/binary-search/\
](http://www.jiuzhang.com/solutions/binary-search/)

#### 常见问题

**Q: 为什么要用 start + 1 < end？而不是 start < end 或者 start <= end？**

A: 为了避免死循环。二分法的模板中，整个程序架构分为两个部分：

1. 通过 while 循环，将区间范围从 n 缩小到 2 （只有 start 和 end 两个点）。
2. 在 start 和 end 中判断是否有解。

start < end 或者 start <= end 在寻找目标最后一次出现的位置的时候，出现死循环。

**Q: 为什么明明可以 start = mid + 1 偏偏要写成 start = mid?**

A: 大部分时候，mid 是可以 +1 和 -1 的。在一些特殊情况下，比如寻找目标的最后一次出现的位置时，当 target 与 nums\[mid] 相等的时候，是不能够使用 mid + 1 或者 mid - 1 的。因为会导致漏掉解。那么为了节省`脑力`，统一写成 start = mid / end = mid 并不会造成任何解的丢失，并且也不会损失效率——log(n) 和 log(n+1) 没有区别。

## 为什么 start < end 容易出现死循环



许多同学在写二分法的时候，都比较习惯性的写 `while (start < end)` 这样的循环条件。这样的写法及其容易出现死循环，导致 LintCode 上的测试“超时”（Time Limit Exceeded）。

**什么情况下会出现死循环？**

在做 `last position of target` 这种模型下的二分法时，使用 while (start < end) 就容易出现超时。

在线练习：\
[http://www.lintcode.com/problem/last-position-of-target/](http://www.lintcode.com/problem/last-position-of-target/)

我们来看看会超时的代码：\
Java版本：

```java
int start = 0, end = nums.length - 1;
while (start < end) {
    int mid = start + (end - start) / 2;
    if (nums[mid] == target) {
        start = mid;
    } else if (nums[mid] < target) {
        start = mid + 1;
    } else {
        end = mid - 1;
    }
}
```

Python版本：

```python
start, end = 0, len(nums) - 1
while start <end:
    mid = start + (end - start) // 2
    if nums[mid] == target:
        start = mid
    else if nums[mid] < target:
        start = mid + 1
    else:
        end = mid - 1
```

C++版本：

```cpp
int start = 0, end = nums.size() - 1; // nums 是 vector<int>
while (start < end) {
    int mid = start + (end - start) / 2;
    if (nums[mid] == target) {
        start = mid;
    } else if (nums[mid] < target) {
        start = mid + 1;
    } else {
        end = mid - 1;
    }
}

上面这份代码是大部分同学的实现方式。看上去似乎没有太大问题。我们来注意一下 `nums[mid] == target` 时候的处理。这个时候，因为 mid 这个位置上的数有可能是最后一个出现的target，所以不能写成 start = mid + 1（那样就跳过了正确解）。而如果是这样写的话，下面这组数据将出现超时(TLE)：

```

nums = \[1,1], target = 1

```

将数据带入过一下代码：
```

`start = 0, end = 1`\
`while (0 < 1) {`\
`mid = 0 + (1 - 0) / 2 = 0`\
`if (nums[0] == 1) {`\
`start = 0;`\
`}`\
`...`\
`}`

我们发现，start 将始终是 \`0\`。出现这个问题的主要原因是，mid = start + (end - start) / 2 这种写法是偏向start的。也就是说 mid 是中间偏左的位置。这样导致如果 start 和 end 已经是相邻关系，会导致 start 有可能在一轮循环之后保持不变。或许你会说，那么我改成 mid = (start + end + 1) / 2 是否能解决问题呢？没错，确实可以解决 last position of target 的问题，但是这样之后 first position of target 就超时了。我们比较希望能够有一个理想的模板，无论是 first position of target 还是 last position of target，代码的区别尽可能的小和容易记住。
