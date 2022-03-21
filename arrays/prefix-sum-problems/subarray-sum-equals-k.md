---
description: LeetCode 560
---

# Subarray Sum Equals K

{% embed url="https://leetcode.com/problems/subarray-sum-equals-k/" %}

Given an array of integers `nums` and an integer `k`, return _the total number of continuous subarrays whose sum equals to `k`_.

**Example 1:**

```
Input: nums = [1,1,1], k = 2
Output: 2
```

**Example 2:**

```
Input: nums = [1,2,3], k = 3
Output: 2
```

**Constraints:**

* `1 <= nums.length <= 2 * 104`
* `-1000 <= nums[i] <= 1000`
* `-107 <= k <= 107`

#### Using HashMap

{% hint style="info" %}
记录prefix sum，判断是不是map里有sum - k，如果有，count就加上map里sum - k出现的次数，然后更新map里sum的。如果没有，就直接更新map里的sum。初始化map要加入(0, 1)。

eg: sum = { ........6.........6........} k = 6.

count += map.get(sum - k). 后面这一部分是2.

注意⚠️count的值一定要是当前的count加上符合条件sum - k的个数，要不就会漏掉解。

eg: sum = {................2..............2..............2...........7........} k = 5;

如果不加上sum - k的个数，那么就会少两组解。这也是统计sum出现次数的原因。

时间复杂度为$$O(n)$$ ，空间复杂度为$$O(n)$$&#x20;
{% endhint %}

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, sum = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for (int i = 0; i < nums.length; ++i) {
            sum += nums[i];
            if (map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
//[1, -1, 0],k = 0;
//
```

#### Two Pointer

{% hint style="info" %}
prefix sum，时间复杂度为$$O(n^2)$$&#x20;
{% endhint %}

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int i = 0; i < nums.length; ++i) {
            int sum = 0;
            for (int j = i; j < nums.length; ++j) {
                sum += nums[j];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
