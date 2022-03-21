---
description: LintCode 45
---

# Maximum Subarray Difference

{% embed url="https://www.lintcode.com/problem/45/?_from=collection&fromId=164" %}

Description

Given an array with integers.

Find two _non-overlapping_ subarrays `A` and `B`, which |SUM(A) - SUM(B)|∣SUM(A)−SUM(B)∣ is the largest.

Return the largest difference.

The subarray should contain at least one numberExample

**Example 1:**

Input:

```
array = [1, 2, -3, 1]
```

Output:

```
6
```

Explanation:

The subarray are \[1,2] and \[-3].So the answer is 6.

**Example 2:**

Input:

```
array = [0,-1]
```

Output:

```
1
```

Explanation:

The subarray are \[0] and \[-1].So the answer is 1.Challenge

O(n) time and O(n) space.

{% hint style="info" %}
### 算法：贪心

### 思路

* 要找到两个不重叠的子数组，使二者求和的差值最大，那么显然一个子数组在左边，一个子数组在右边。只要我们分别求出每个元素 左边最大/最小的子数组 和 右边最大/最小的子数组，就能计算出差值，取 左边最大值与右边最小值的差值 和 左边最小值与右边最大值的差值 的最大值。
* 因此，假设下标从0开始，对于下标为i的元素x，我们要想办法求出`[0, i]`范围内的左边最大/最小的子数组和，还有右边最大/最小的子数组和，那么右边数组的范围是多少呢？因为两个子数组是不重叠的，因此这时右边数组的范围为`[i + 1, n - 1]`。
* 令`lMax[i]`, `lMin[i]`分别表示从左到i的范围内的子数组最大/最小和，令`rMax[i]`, `rMin[i]`分别表示从右到i的范围内的子数组最大/最小和。然后依次用贪心的思想求出每个数组。这样我们就能很容易的求出最大子数组差了，遍历一次整个数组，取
* max(abs(`lMax[i] - rMin[i + 1]`), abs(`lMin[i] - rMax[i + 1]`))
* 用贪心的思想求出每个数组，以 从左到i的最大值rMax\[] 为例:
* 我们从左到右遍历整个数组，依次选取最优答案，那么当我们处理到`rMax[i]`的时候，`rMax[i - 1]`即为前(i - 1)个元素部分的最大子数组和。我们定义now记录当前最优值，即必须选择当前元素的最大子数组的和。然后我们比较当前最优值和之前的最优值，哪个比较大就选择哪个作为当前最优值`rMax[i]`。
* 即：
* `now = max(nums[i], now + nums[i])`
* `lMax[i] = max(lMax[i - 1], now)`

### 复杂度

* 假设数组大小为n；
* 空间复杂度为O(n);
* 时间复杂度为O(n)。

TestCase: \[1, 2, -3, 1]

left min========  1, 1, -3, -3

left max======== 1, 3, 3, 3

right min======= -3, -3, -3, 1

right max======= 3, 3, 1, 1
{% endhint %}

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: An integer indicate the value of maximum difference between two substrings
     */
    public int maxDiffSubArrays(int[] nums) {
        // write your code here
        int n = nums.length;
        if (n < 2) {
            return 0;
        }

        int[] lmin = new int[n];
        int[] lmax = new int[n];
        int[] rmin = new int[n];
        int[] rmax = new int[n];

        lmin[0] = nums[0];
        for (int i = 1, now = nums[0]; i < n; ++i) {
            now = Math.min(nums[i], now + nums[i]);
            lmin[i] = Math.min(lmin[i - 1], now);
            System.out.print(lmin[i] + ",");
        }
        System.out.println(" ");

        lmax[0] = nums[0];
        for (int i = 1, now = nums[0]; i < n; ++i) {
            now = Math.max(nums[i], now + nums[i]);
            lmax[i] = Math.max(lmax[i - 1], now);
            System.out.print(lmax[i] + ",");
        }
        System.out.println(" ");

        rmin[n - 1] = nums[n - 1];
        for (int i = n - 2, now = nums[n - 1]; i >= 0; --i) {
            now = Math.min(nums[i], now + nums[i]);
            rmin[i] = Math.min(rmin[i + 1], now);
            System.out.print(rmin[i] + ",");
        }
        System.out.println(" ");

        rmax[n - 1] = nums[n - 1];
        for (int i = n - 2, now = nums[n - 1]; i >= 0; --i) {
            now = Math.max(nums[i], now + nums[i]);
            rmax[i] = Math.max(rmax[i + 1], now);
            System.out.print(rmax[i] + ",");
        }

        int res = Integer.MIN_VALUE;
        for (int i = 0; i < n - 1; ++i) {
            res = Math.max(res, Math.max(Math.abs(lmax[i] - rmin[i + 1]), Math.abs(lmin[i] - rmax[i + 1])));
        }

        return res;
    }
}
```
