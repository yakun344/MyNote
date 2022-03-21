---
description: LeetCode 410
---

# Split Array Largest Sum

{% embed url="https://leetcode.com/problems/split-array-largest-sum/" %}

Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these msubarrays.

**Note:**\
If n is the length of array, assume the following constraints are satisfied:

* 1 ≤ n ≤ 1000
* 1 ≤ m ≤ min(50, n)

**Examples:**&#x20;

```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        int max = 0, sum = 0;
        for (int i = 0; i < nums.length; ++i) {
            sum += nums[i];
            max = Math.max(max, nums[i]);
        }
        
        int start = max, end = sum;
        
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (canSplit (nums, m, mid)) {
                end = mid;
            }
            else {
                start = mid;
            }
        }
        if (canSplit(nums, m, start)) return start;
        else return end;
    }
    
    //以mid为最大值，看能不能分成m份
    private boolean canSplit(int[] nums, int m, int maxSum) {
        int sum = 0;
        for (int i = 0; i < nums.length; ++i) {
            sum += nums[i];
            if (sum > maxSum) {
                sum = nums[i];
                m--;
            }
            if (m == 0) return false;
        }
        return true;
    }
}
```
