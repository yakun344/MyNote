---
description: LeetCode 992
---

# Subarrays with K Different Integers

{% embed url="https://leetcode.com/problems/subarrays-with-k-different-integers/" %}



Given an array `nums` of positive integers, call a (contiguous, not necessarily distinct) subarray of `nums` _good_ if the number of different integers in that subarray is exactly `k`.

(For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.)

Return the number of good subarrays of `nums`.

**Example 1:**

```
Input: nums = [1,2,1,2,3], k = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
```

**Example 2:**

```
Input: nums = [1,2,1,3,4], k = 3
Output: 3
Explanation: Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].
```

**Note:**

1. `1 <= nums.length <= 20000`
2. `1 <= nums[i] <= nums.length`
3. `1 <= k <= nums.length`

{% hint style="info" %}
非常巧妙的做法：

exactly(K) = atMost(K) - atMost(K - 1)
{% endhint %}

```java
class Solution {
    public int subarraysWithKDistinct(int[] nums, int k) {
        return kmost(nums, k) - kmost(nums, k - 1);
    }
    
    private int kmost(int[] nums, int k) {
        int left = 0, right = 0, res = 0, count = 0;
        Map<Integer, Integer> map = new HashMap<>();
        while (right < nums.length) {
            int rn = nums[right];
            map.put(rn, map.getOrDefault(rn, 0) + 1);
            if (map.get(rn) == 1) {
                count++;
            }
            right++;
            
            while (count > k) {
                int ln = nums[left];
                map.put(ln, map.get(ln) - 1);
                if (map.get(ln) == 0) {
                    count--;
                }
                left++;
            }
            
            res += right - left;
        }
        return res;
    }
}
```

For anyone confused about `ret += j - i + 1`, here's how you can look at it:

_(Warning: This is a lengthy explanation -- TL;DR in comments)_

**Example: A = \[1,2,1,2,3], K = 2**\
We know that `ret` should return the **total** number of contiguous subarrays with **at most** `K` different integers.\
We also know that at every iteration, `j - i + 1` gives the **length of the contiguous subarray**.\
Given our example, let's see how this is handled in each valid iteration of this code (i.e. the "sliding window" part of this technique).

Here's what we get:

* `[1]` is **one** valid result of a contiguous subarray that has **at most** `K` different integers and has the length of **1**.
* `[1,2]` is **one** valid result of a contiguous subarray that has **at most** `K` different integers and has the length of **2**.
* `[1,2,1]` is **one** valid result of a contiguous subarray that has **at most** `K` different integers and has the length of **3**.
* `[1,2,1,2]` is **one** valid result of a contiguous subarray that has **at most** `K` different integers and has the length of **4**.

Then when we see the last element `3`, we will see that the only valid contiguous subarray with **at most** `K` (which is 2) different integers that can be created is:

* `[2,3]` is **one** valid result of a contiguous subarray that has **at most** `K` different integers and has the length of **2**.

So our function will return the sum of the **length** of all of those subarrays:

* **1 + 2 + 3 + 4** which is **10** different contiguous subarrays with **at most** `K` different integers for the subarray `[1,2,1,2]`.
* **2** which is **2** different contiguous subarrays with **at most** `K` different integers for the subarray `[2,3]`.

Here the answer is **10 + 2** which is **12** different contiguous subarrays with **at most** `K = 2` different integers.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

**Q: Now that we know visually what the code is doing, how do we relate that back to the actual subarrays? What exactly are those different contiguous subarrays?**\
Great, we have **12** different contiguous subarrays with **at most** `K = 2` different integers, but what does that actually look like logically?

`[1,2,1,2]` will produce a total of **10** different contiguous subarrays:

* `[1,2,1,2]` (**1** different contiguous subarrays with length 4)
* `[1,2,1]`, `[2,1,2]` (**2** different contiguous subarrays with length 3)
* `[1, 2]`, `[1,2]`, `[2,1]`(**3** different contiguous subarrays with length 2)
* `[1]`, `[2]`, `[1]`, `[2]` (**4** different contiguous subarrays with length 1)

`[2,3]` will produce a total of **3** different contiguous subarrays:

* `[2,3]` (**1** different contiguous subarrays with length 2)
* `[2]`, `[3]` (**2** different contiguous subarrays with length 1)

_NOTE: Remember this for a bit later. Grouping it in terms of length will help us understand a point later_

Now you may notice that `[2,3]` produced a length of 2 earlier, but can actually create **3** different subarrays.\
`[2,3]` will only produce **2** _new_ different contiguous subarrays because `[2]` was already accounted for in `[1,2,1,2]`.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

**Q: Okay, I understood all that, but I still don't get why the number of contiguous subarrays is equal to the sum of lengths. Why does that work?**\
We want to find **all** valid contiguous subarrays that`[1,2,1,2,3]` would produce with **at most K** different integers.\
You will notice though -- that when we say **at most K** different integers -- we only use `K` to help us find the valid windows (ex. `[1,2,1,2]` and `[2,3]`)\
Once we have those valid windows though, we don't really care what `K` is (that's because **at most** means **0 to K** unique Integers, which literally means any contiguous subarray now). So, stop thinking about how `K` will affect the subarrays to understand the summation of lengths.

Okay, so now that we have our (let's call them "complete") valid subarrays `[1,2,1,2]` and `[2,3]` -- we can begin to understand why we take the summation of lengths to count all subarrays. Remember how we listed all the subarrays earlier and how I asked you to remember why we group subarrays by length?

Here's why:\
`[1,2,1,2]` will produce **1** subarrays of length 4, **2** subarrays of length 3, **3** subarrays of length 2, and **4** subarray of length 1 _(see above)_\
`[2,3]` will produce **1** subarrays of length 2, **2** subarray of length 1 _(see above)_

Do you notice anything?

`[1,2,1,2]` = **1 + 2 + 3 + 4** (sum of our **1** subarrays of length 4, **2** subarray of length 3, etc.) = **10**\
`[2,3]` = **1 + 2** (\*Special case: this creates **1** subarray of length 2, and **2** subarray of length 2, but since our `[2]` one was accounted for already, we only get **2** new subarrays so subtract **1**) **- 1** = **2**

When we summed the different lengths _(see above where we listed the iterations)_, we also get the same growth (i.e. **1 + 2 + 3 + 4**)!

So another way to understand this in the context of this problem is, that the code above will produce valid (sliding) window (like `[1,2]`, `[1,2,1]`, `[1,2,1,2]`).\
As we expand the length of that window, we can sum the length of those windows to get our different combinations because if our "complete" window was `[1,2,1,2]`,\
we could do **1 + 2 + 3 + 4** (or length of `[1]` + length of `[1,2]` + length of `[1,2,1]` + length of `[1,2,1,2]`).

We also noticed that for `[2,3]`, by only adding **2**, we were able to ignore that duplicate subarray. The sliding window did not return `[2]` because the window expanded to `[1,2,1,2,3]` -- invalidating the window and then compressed the window to `[2,3]` by moving `i` forward. This allowed us to skip those duplicate subarrays. You can expand this to other examples including where `K` is larger. The fact that our sliding window compresses by moving forward `i` will allow the lower summations to be ignored (i.e. our duplicate subarrays).

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

**Q: Woah, that was a lot to take in. Does learning this help me with other subarray counting?**\
You'll be happy to know that it does! This actually comes up frequently in other Leetcode questions that require us to count different combinations of subarrays.\
Grouping the subarrays in terms of length can really help us understand that summing up the lengths can give us the total subarrays.\
If you're still not convinced, write out a few examples of arrays of different lengths. The number of subarrays you can create can be related to the length.

Arrays of length 5 will produce **1 + 2 + 3 + 4 + 5** different contiguous subarrays.\
Arrays of length 6 will produce **1 + 2 + 3 + 4 + 5 + 6** different contiguous subarrays.\
_...and the list goes on_

**Alternative Formula:**\
If you're given an array of length `n`, it will produce `(n *(n+1))/2` total contiguous subarrays (same as doing the sum above).\
This is used often in other questions where we know our array size of `n` and so we don't need to add `[1...n]` incrementally like this problem.

This is just one way to look at this, I'm sure there are more ways to break down counting subarrays. Keep reading around until something resonates with you. Best of luck!
