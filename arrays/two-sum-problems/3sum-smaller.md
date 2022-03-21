---
description: LeetCode 259
---

# 3Sum Smaller

{% embed url="https://leetcode.com/problems/3sum-smaller/" %}

Given an array of n integers nums and a target, find the number of index triplets `i, j, k` with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

**Example:**

```
Input: nums = [-2,0,1,3], and target = 2
Output: 2 
Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
```

**Follow up:** Could you solve it in O(n2) runtime?

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        int count = 0;
        if (nums == null || nums.length < 3) {
            return count;
        }
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; ++i) {
            int j = i + 1, k = nums.length - 1;
            while (j < k) {
                if (nums[i] + nums[j] + nums[k] < target) {
                    count += k - j;
                    j++;
                } else {
                    k--;
                }
            }
        }
        
        return count;
    }
}
```

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        int count = 0;
        if (nums == null || nums.length < 3) {
            return count;
        }
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; ++i) {
            count += twoSum(nums, i + 1, target - nums[i]);
        }
        
        return count;
    }
    private int twoSum(int[] nums, int left, int target) {
        int sum = 0;
        int right = nums.length - 1;
        while (left < right) {
            if (nums[left] + nums[right] < target) {
                sum += right - left;
                left++;
            } else {
                right--;
            }
        }
        return sum;
    }
}
```
