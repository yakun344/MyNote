---
description: LeetCode 53
---

# Maximum Subarray

{% embed url="https://leetcode.com/problems/maximum-subarray/" %}

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return _its sum_.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Example 2:**

```
Input: nums = [1]
Output: 1
```

**Example 3:**

```
Input: nums = [5,4,-1,7,8]
Output: 23
```

**Constraints:**

* `1 <= nums.length <= 3 * 104`
* `-105 <= nums[i] <= 105`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

{% hint style="info" %}
Kadane's Algorithm

cur记录当前以i为index结尾的subarray和的最大值；

max记录全局subarray的最大值；

TC : O(N)

SC : O(1)
{% endhint %}

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int cur = nums[0];
        int max = nums[0];
        for (int i = 1; i < nums.length; ++i) {
            cur = Math.max(nums[i], cur + nums[i]);
            max = Math.max(max, cur);
        }
        return max;
    }
}
```

{% hint style="info" %}
1. 定义 maxAns 记录全局最大值，即结果；sum 记录当前子数组的和；
2. 初始化： maxAns=Integer.MIN\_VALUE; sum=0; 因为数组可以全为负数，因此maxAns不能直接初始化为0；
3. 遍历整数数组：
   * sum累加当前的值；
   * 若当前 sum>maxAns ，更新 maxAns=sum；
   * 若当前 sum<0 ，表示当前的子数组和已经为负，累加到后面会使和更小，因此令 sum=0，相当于放弃当前的子数组，重新开始；
{% endhint %}

* 时间复杂度：O(N),N为数组长度
  * 只需要遍历一遍数组就能找到答案；
* 空间复杂度：O(1)
  * 只需要用到maxAns和sum两个变量；

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 0 || nums == null) return -1;
        
        int maxSum = nums[0];
        int sum = 0;
        for (int i = 0; i < nums.length; ++i) {
            sum += nums[i];
            maxSum = Math.max(maxSum, sum);
            sum = Math.max(sum, 0);
        }
        
        return maxSum;
    }
}
```
