---
description: LeetCode 295
---

# Find Median from Data Stream

{% embed url="https://leetcode.com/problems/find-median-from-data-stream" %}

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

* For example, for `arr = [2,3,4]`, the median is `3`.
* For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

* `MedianFinder()` initializes the `MedianFinder` object.
* `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
* `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

&#x20;

**Example 1:**

```
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

&#x20;

**Constraints:**

* `-105 <= num <= 105`
* There will be at least one element in the data structure before calling `findMedian`.
* At most `5 * 104` calls will be made to `addNum` and `findMedian`.

&#x20;

**Follow up:**

* If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
* If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

{% hint style="info" %}
用两个堆，左边最大堆，右边最小堆，把数字分为两个部分，左边的 <= leftmax.peek(),

右边的 >= rightmin.peek()。

如果堆的大小left 2k + 1, right 2k,中位数在左边；如果left 2k, right 2k，中位数在中间。

先把数加入到左边，然后左边的peek调整到右边，然后再检查两个堆的size()，使得相差 不大于1。

时间复杂度O(logn),空间复杂度O(n)
{% endhint %}

```java
class MedianFinder {
    public PriorityQueue<Integer> leftmax;
    public PriorityQueue<Integer> rightmin;
    public MedianFinder() {
        leftmax = new PriorityQueue<>((a, b) -> Integer.compare(b, a));
        rightmin = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        leftmax.offer(num);
        rightmin.offer(leftmax.poll());
        
        if (leftmax.size() < rightmin.size()) {
            leftmax.offer(rightmin.poll());
        }
    }
    
    public double findMedian() {
        if (leftmax.size() == rightmin.size()) {
            return (double)(leftmax.peek() + rightmin.peek()) / 2.0;
        } else {
            return leftmax.peek();
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```
