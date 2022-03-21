---
description: LeetCode 31
---

# Next Permutation

{% embed url="https://leetcode.com/problems/next-permutation/" %}

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be [**in place**](http://en.wikipedia.org/wiki/In-place\_algorithm) and use only constant extra memory.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [1,3,2]
```

**Example 2:**

```
Input: nums = [3,2,1]
Output: [1,2,3]
```

**Example 3:**

```
Input: nums = [1,1,5]
Output: [1,5,1]
```

**Example 4:**

```
Input: nums = [1]
Output: [1]
```

**Constraints:**

* `1 <= nums.length <= 100`
* `0 <= nums[i] <= 100`

1. Find the first nums\[i] > nums\[i - 1] from end of nums, record this i - 1 as index.
2. Find the first num just bigger than nums\[i - 1], record this index as next\_index.
3. swap(nums\[index], nums\[next\_index])
4. reverse(nums\[index+1], end)

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int index = -1;
        for (int i = nums.length - 1; i > 0; i--) {
            if (nums[i] > nums[i - 1]) {
                index = i - 1;
                break;
            }
        }
        
        if (index == -1) {
            reverse(nums, 0);
            return;
        }
        
        int next_index = -1;
        for (int i = nums.length - 1; i > 0; --i) {
            if (nums[i] > nums[index]) {
                next_index = i;
                swap(nums, index, next_index);
                break;
            }
        }
        
        reverse(nums, index + 1);
        return;
    }
    private void reverse(int[] nums, int index) {
        int start = index, end = nums.length - 1;
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
        return;
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
        return;
    }
}
```
