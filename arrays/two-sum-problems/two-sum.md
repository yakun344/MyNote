---
description: LeetCode 1
---

# Two Sum

{% embed url="https://leetcode.com/problems/two-sum/" %}

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have _**exactly**_ one solution, and you may not use the _same_ element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return null;
        }
        
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; ++i) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; ++i) {
            int sec = target - nums[i];
            if (map.containsKey(sec) && map.get(sec) != i) {
                return new int[]{map.get(sec), i};
            }
        }
        return null;
    }
}
```
