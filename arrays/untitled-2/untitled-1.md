---
description: LintCode 604
---

# Window Sum

{% embed url="https://www.lintcode.com/problem/window-sum/description" %}

#### Description

Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the `sum` of the element inside the window at each moving.

#### Example

**Example 1**

```
Input：array = [1,2,7,8,5], k = 3
Output：[10,17,20]
Explanation：
1 + 2 + 7 = 10
2 + 7 + 8 = 17
7 + 8 + 5 = 20
```

```java
public class Solution{
    public int[] winSum(int[] nums, int k) {
            
            if (nums == null || nums.lenth == 0 || k <= 0) {
                return new int[]{};
            }
            
            int[] sum = new int[nums.length - k + 1];
            //计算前k个数之和，赋值给sum[0],算出第一个sum[]的值;
            for (int i = 0; i < k; ++i) {
                sum[0] += nums[i];
            }
            //减去头部，加上尾巴；
            for (int i = 0; i < sum.length; ++i) {
                sum[i] = sum[i - 1] - nums[i - 1] + nums[i + k - 1];
            }
            
            return sum;
        }
    }
```
