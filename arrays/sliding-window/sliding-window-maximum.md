---
description: LeetCode 239
---

# Sliding Window Maximum

{% embed url="https://leetcode.com/problems/sliding-window-maximum/" %}



You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

**Example 3:**

```
Input: nums = [1,-1], k = 1
Output: [1,-1]
```

**Example 4:**

```
Input: nums = [9,11], k = 2
Output: [11]
```

**Example 5:**

```
Input: nums = [4,-2], k = 2
Output: [4]
```

**Constraints:**

* `1 <= nums.length <= 105`
* `-104 <= nums[i] <= 104`
* `1 <= k <= nums.length`

{% hint style="info" %}
* 单调队列中的元素是严格单调的。我们在求解这个问题的时候需要维护他的单调性。
* 队首元素即为当前位置的最大值。假设要求滑动窗口中的最大值。我们就需要确保滑动窗口中的元素从队首到队尾是递减的。
* 每滑动一次就判断当前元素和队尾元素的关系，如果放入队尾满足单调递减，那么放入即可；如果放入不满足，就需要删除队尾元素直到放入当前元素之后满足队列单调递减。同时要确保已经出窗口的最大值（队首元素）被删除掉。

注意⚠️  往单调队列里压入的是元素对应的下标，因为我们要通过下标来判断该元素有没有离开了滑动窗口，如果离开了滑动窗口就得从队列里给删除了。
{% endhint %}

![](<../../.gitbook/assets/image (24).png>)

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> queue = new LinkedList<>();
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; ++i) {
        
            //维护单调性，当前数字比队尾的大，就一直remove，直到等于或小于队尾数字
            while (!queue.isEmpty() && nums[i] > nums[queue.peekLast()]) {
                queue.removeLast();
            }
            
            //当前窗口大小超过k，删掉超过的部分
            if (!queue.isEmpty() && i - queue.peekFirst() >= k) {
                queue.removeFirst();
            }
            
            queue.addLast(i);
            
            //统计答案
            if (i >= k - 1) {
                list.add(nums[queue.peekFirst()]);
            }
        }
        int[] res = new int[list.size()];
        for (int i = 0; i < list.size(); ++i) {
            res[i] = list.get(i);
        }
        
        return res;
    }
}
```

```java
// Author: Huahua
// Running time: 19 ms
class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    if (k == 0) return nums;
    
    int[] ans = new int[nums.length - k + 1];
    Deque<Integer> indices = new LinkedList<>();
    
    for (int i = 0; i < nums.length; ++i) {
      while (indices.size() > 0 && nums[i] >= nums[indices.getLast()])
        indices.removeLast();
      
      indices.addLast(i);
      if (i - k + 1 >= 0) ans[i - k + 1] = nums[indices.getFirst()];
      if (i - k + 1 >= indices.getFirst()) indices.removeFirst();
    }
             
    return ans;
  }
}
```
