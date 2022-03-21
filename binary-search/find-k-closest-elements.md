---
description: LeetCode 658
---

# Find K Closest Elements

{% embed url="https://leetcode.com/problems/find-k-closest-elements/" %}

Given a **sorted** integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.

An integer `a` is closer to `x` than an integer `b` if:

* `|a - x| < |b - x|`, or
* `|a - x| == |b - x|` and `a < b`

**Example 1:**

```
Input: arr = [1,2,3,4,5], k = 4, x = 3
Output: [1,2,3,4]
```

**Example 2:**

```
Input: arr = [1,2,3,4,5], k = 4, x = -1
Output: [1,2,3,4]
```

**Constraints:**

* `1 <= k <= arr.length`
* `1 <= arr.length <= 104`
* Absolute value of elements in the array and `x` will not exceed `104`

{% hint style="info" %}
先二分法找到离x距离最近的点设为mid，从mid出发，左右两指针相背而行，找到k个数，再排序结果就可以了。
{% endhint %}

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int mid = binarySearch(arr, x);
        List<Integer> result = new ArrayList<>();
        result.add(arr[mid]);
        k--;
        int left = mid - 1, right = mid + 1;
        while (k > 0) {
            if (left < 0) {
                result.add(arr[right++]);
            } else if (right >= arr.length) {
                result.add(arr[left--]);
            } else if (Math.abs(arr[left] - x) <= Math.abs(arr[right] - x)) {
                result.add(arr[left--]);
            } else {
                result.add(arr[right++]);
            }
            k--;
        }
        Collections.sort(result);
        return result;
    }
    private int binarySearch(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] < target) {
                left = mid;
            } else {
                right = mid;
            }
            
        }
        if (Math.abs(arr[left] - target) <= Math.abs(arr[right] - target)) {
            return left;
        } else {
            return right;
        }
    }
}
```
