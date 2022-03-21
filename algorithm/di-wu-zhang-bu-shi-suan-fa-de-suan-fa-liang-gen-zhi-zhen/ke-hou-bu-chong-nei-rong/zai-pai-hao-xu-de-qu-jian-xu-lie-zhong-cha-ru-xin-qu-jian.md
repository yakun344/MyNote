# 在排好序的区间序列中插入新区间

#### 问题描述

给一个排好序的区间序列，插入一段新区间。求插入之后的区间序列。要求输出的区间序列是没有重叠的。

LintCode 练习地址：[http://www.lintcode.com/problem/insert-interval/](http://www.lintcode.com/problem/insert-interval/)

#### 算法描述

1. 将该新区间按照**左端值**插入原区间中，使得原区间**左端值**是有序的。
2. 遍历原区间列表，并把它复制到一个新的`answer`区间列表当中，`answer`是最后要返回的结果。
3. 遍历时，要记录上一次访问的区间`last`。若当前区间**左端值**小于等于`last`区间的**右端值**，说明这两区间有重叠，此时仅更新`last`的**右端值**为这两区间**右端值**较大者；若当前区间**左端值**大于`last`的**右端值**，则可以直接加入`answer`。
4. 返回`answer`。

**F.A.Q**\
**Q：第三步有什么意义？**\
A：插入新区间后的原区间列表，仅能保证左端是有序的。而区间中是否存在重叠，右端是否有序，这些都是未知的。

**Q：时空复杂度多少？**\
A：都是O(N)。

**Q：有没有更高效的做法？**\
A：有！在查找左端新区见待插位置时，可以采用二分查找。原算法的的第三步，实际上是在查找右端的位置，也可以用二分查找，这样两次查找的复杂度都降为了O(logN)。但是，**完全没必要**，因为这个算法涉及到数组中间位置的移动，所以O(N)的时间复杂度是逃不开的，二分查找的改进对效率提升不明显，而且会增大编码难度。有兴趣的同学可以自己尝试\~

#### 参考代码

Java版本：

```java
public class Solution {
    /*
     * @param intervals: Sorted interval list.
     * @param newInterval: new interval.
     * @return: A new interval list.
     */
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        List<Interval> answer = new ArrayList<>();

        int index = 0;
        while (index < intervals.size() && intervals.get(index).start < newInterval.start) {
            index++;
        }
        intervals.add(index, newInterval);

        Interval last = null;
        for (Interval item : intervals) {
            if (last == null || last.end < item.start) {
                answer.add(item);
                last = item;
            } else {
                last.end = Math.max(last.end, item.end); // Modify the element already in list
            }
        }
        return answer;
    }
}
```

Python版本：

```python
class Solution:
      # @param intervals: Sorted interval list.
      # @param newInterval: new interval.
      # @return: A new interval list.
      def insert(self, intervals, new_interval):
          answer = []

          index = 0
          while index < len(intervals) and intervals[index].start < new_interval.start:
              index += 1
          intervals.insert(index, new_interval)

          last = None
          for item in intervals:
              if last == None or last.end < item.start:
                  answer.append(item)
                  last = item
              else:
                  last.end = max(last.end, item.end)
          return answer
```

#### 相关练习

[http://www.lintcode.com/problem/intersection-of-arrays/](http://www.lintcode.com/problem/intersection-of-arrays/)\
[http://www.lintcode.com/problem/merge-intervals/](http://www.lintcode.com/problem/merge-intervals/)
