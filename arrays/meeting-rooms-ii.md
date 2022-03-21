---
description: LeetCode 253
---

# Meeting Rooms II

{% embed url="https://leetcode.com/problems/meeting-rooms-ii/" %}

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` (si < ei), find the minimum number of conference rooms required.

**Example 1:**

```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

**Example 2:**

```
Input: [[7,10],[2,4]]
Output: 1
```

{% hint style="info" %}
对每个会议的开始和结束**排序**，记录到两个数组里；

如果当前的start\<end，说明当前的会议没有结束，需要开新的房间。

否则当前会议结束，就可以用旧的房间。
{% endhint %}

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        if(intervals == null || intervals.length == 0) {
            return 0;
        }
        int[] start = new int[intervals.length];
        int[] end = new int[intervals.length];
        for (int i = 0; i < intervals.length; ++i) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        
        Arrays.sort(start);
        Arrays.sort(end);
        
        int num = 0;
        int i = 0, j = 0;
        while (i < start.length) {
            if (start[i] < end[j]) {
                num++;
                i++;
            } else {
                i++;
                j++;
            }
        }
        
        return num;
    }
}
```

{% hint style="info" %}
根据开始时间的大小对intervals排序。

在heap中，push结束时间，如果当前的会议开始时间小于等于堆顶的结束时间，那么堆顶的元素pop出去，加入当前的会议，否则直接加入当前会议。

遍历整个intervals之后，堆的size就是需要的会议室数量。
{% endhint %}

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        for (int[] interval : intervals) {
            if (heap.isEmpty()) {
                heap.offer(interval[1]);
            } else if (heap.peek() <= interval[0]) {
                heap.offer(interval[1]);
                heap.poll();
            } else {
                heap.offer(interval[1]);
            }
        }
        
        return heap.size();
    }
}

```
