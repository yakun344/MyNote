---
description: LeetCode 137
---

# Single Number II

{% embed url="https://leetcode.com/problems/single-number-ii/" %}



Given an integer array `nums` where every element appears **three times** except for one, which appears **exactly once**. _Find the single element and return it_.

**Example 1:**

```
Input: nums = [2,2,3,2]
Output: 3
```

**Example 2:**

```
Input: nums = [0,1,0,1,0,1,99]
Output: 99
```

**Constraints:**

* `1 <= nums.length <= 3 * 104`
* `-231 <= nums[i] <= 231 - 1`
* Each element in `nums` appears exactly **three times** except for one element which appears **once**.

**Follow up:** Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?



**Bitwise Operators : NOT, AND and XOR**

**Intuition**

Now let's discuss \mathcal{O}(1)O(1) space solution by using three [bitwise operators](https://wiki.python.org/moin/BitwiseOperators)

\sim x \qquad \textrm{that means} \qquad \textrm{bitwise NOT}∼xthat meansbitwise NOT

x \\& y \qquad \textrm{that means} \qquad \textrm{bitwise AND}x\&ythat meansbitwise AND

x \oplus y \qquad \textrm{that means} \qquad \textrm{bitwise XOR}x⊕ythat meansbitwise XOR

**XOR**

Let's start from XOR operator which could be used to detect the bit which appears odd number of times: 1, 3, 5, etc.

XOR of zero and a bit results in that bit

0 \oplus x = x0⊕x=x

XOR of two equal bits (even if they are zeros) results in a zero

x \oplus x = 0x⊕x=0

and so on and so forth, i.e. one could see the bit in a bitmask only if it appears odd number of times.

![fig](https://leetcode.com/problems/single-number-ii/Figures/137/xor.png)

That's already great, so one could detect the bit which appears once, and the bit which appears three times. The problem is to distinguish between these two situations.

**AND and NOT**

To separate number that appears once from a number that appears three times let's use two bitmasks instead of one: `seen_once` and `seen_twice`.

The idea is to

* change `seen_once` only if `seen_twice` is unchanged
* change `seen_twice` only if `seen_once` is unchanged

![fig](https://leetcode.com/problems/single-number-ii/Figures/137/three.png)

This way bitmask `seen_once` will keep only the number which appears once and not the numbers which appear three times.\
\


```java
class Solution {
  public int singleNumber(int[] nums) {
    int seenOnce = 0, seenTwice = 0;

    for (int num : nums) {
      // first appearence: 
      // add num to seen_once 
      // don't add to seen_twice because of presence in seen_once

      // second appearance: 
      // remove num from seen_once 
      // add num to seen_twice

      // third appearance: 
      // don't add to seen_once because of presence in seen_twice
      // remove num from seen_twice
      seenOnce = ~seenTwice & (seenOnce ^ num);
      seenTwice = ~seenOnce & (seenTwice ^ num);
    }

    return seenOnce;
  }
}
```
