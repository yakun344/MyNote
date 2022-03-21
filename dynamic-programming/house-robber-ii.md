---
description: LeetCode 213
---

# House Robber II

{% embed url="https://leetcode.com/problems/house-robber-ii/" %}



You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**

```
Input: nums = [0]
Output: 0
```

**Constraints:**

* `1 <= nums.length <= 100`
* `0 <= nums[i] <= 1000`

``

Assume we have `nums` of `[7,4,1,9,3,8,6,5]` as shown in the figure. Since the start house and last house are adjacent to each other, if the thief decides to rob the start house `7`, they cannot rob the last house `5`. Similarly, if they select last house `5`, they have to start from a house with value `4`. Therefore, the final solution that we are looking for is to take the maximum value the thief can rob between houses of list `[7,4,1,9,3,8,6]` and `[4,1,9,3,8,6,5]`. For each of the lists, all we need to do is to figure the maximum value the thief can get using the approach in the original [House Robber Problem](https://leetcode.com/problems/house-robber/).

![alt text](https://leetcode.com/problems/house-robber-ii/Figures/213/213\_house\_robberII\_approach1\_slide01.png)

**Solving Original House Robber Problem with Dynamic Programming**

Trivial cases:

* If there is one house, the answer is the value of that house.
* If there are two houses, the answer is `max(house1, house2)`.
* If there are three houses, you can either pick the middle house or the sum of the first and the last house. Therefore, it boils down to `max(house3 + house1, house2)`.

To make the example more illustrative, imagine two thieves (`t1` and `t2`) coordinating a grand robbery. They are equipped with walkie-talkies to communicate the values of houses to each other.

* Before entering any of the houses, both `t1` and `t2` have values of zero.
* `t1`, enters the first house and record the value of the house. If that is the only house to rob, they can rob this house and be done with it.
* If there is more than one house, `t1` will leave a note of maximum value reaped until this point (which is just the value of the first house) and move to the next house while `t2` moves into the house `t1` was in. Now, `t1` and `t2` are going to communicate over the walkie-talkie to ask who has the most value. At this point, `t2` will read the note left by `t1` when the values are compared. If they have only two houses to rob, they would rob the house with the most value and be done with it.
* If there are three houses, `t1` will leave a note of the maximum value reaped until this point and move to the next house. Then `t1` will compare the value of the sum of the current house and the house which `t2` is in with the value of the house `t1` was in. The maximum value between those two will be chosen and `t2` will move into the house next to it.
* If there are four houses, `t1` will leave a note of the maximum value reaped until this point and move to the next house. Then `t1` will compare the value of the sum of the current house and the house which `t2` is in with the value of the house `t1` was in. The maximum value between those two will be chosen and `t2` will move into the house next to it.
* This procedure is done over and over again as long as there are houses in `nums`. If `t1` has reached to the end of `nums`, `t1` should have reaped the maximum amount obtainable from houses in `nums`.

{% hint style="info" %}
给定的nums是循环数组，所以按照rob的规则，有两种可能的最大值，返回最大的即可。

0 - nums.length - 2

1 - nums.length - 1

找当前数组最大值的时候，用t1记录当前最优解， t2记录t1前一个的最优解，那么t1 = Math.max(t1, t2 + current)。移动temp指针。循环结束，t1记录的就是这个数组的最优解。
{% endhint %}

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        if (nums.length == 1) {
            return nums[0];
        }
        
        int max1 = dp(nums, 0, nums.length - 2);
        int max2 = dp(nums, 1, nums.length - 1);
        
        return Math.max(max1, max2);
    }
    
    private int dp(int[] nums, int start, int end) {
        int t1 = 0;
        int t2 = 0;
        
        for (int i = start; i <= end; ++i) {
            int temp = t1;
            int current = nums[i];
            t1 = Math.max(t1, t2 + current);
            t2 = temp;
        }
        
        return t1;
    }
}
```
