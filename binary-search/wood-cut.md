---
description: LintCode 183
---

# Wood Cut

{% embed url="https://www.lintcode.com/problem/183/" %}

#### Description

Given n pieces of wood with length `L[i]` (integer array). Cut them into small pieces to guarantee you could have equal or more than k pieces with the same length. What is the longest length you can get from the n pieces of wood? Given L & k, return the maximum length of the small pieces.

You couldn't cut wood into float length.

If you couldn't get >= _k_ pieces, return `0`.

#### Example

**Example 1**

```
Input:
L = [232, 124, 456]
k = 7
Output: 114
Explanation: We can cut it into 7 pieces if any piece is 114cm long, however we can't cut it into 7 pieces if any piece is 115cm long.
```

**Example 2**

```
Input:
L = [1, 2, 3]
k = 7
Output: 0
Explanation: It is obvious we can't make it.
```

#### Challenge

O(n log Len), where Len is the longest length of the wood.

{% hint style="info" %}
二分答案

length : 1 2 3 4 ... 113 114 115 116 117 ...

K:           456 455 ...7   7       6     6      5    ...

&#x20;              xxxxxxxxxxxxx       ooooooooo...

要找到一种切法让木块的长度最大并且数量为k,做法是二分最长的数额，范围是从0到Max(L(i)),如果能切出>=k，就可以完成，start = mid;如果 <= k，就切不成，end = mid。

#### 注意⚠️ start的值从1开始，如果找不到就返回0
{% endhint %}

```java
public class Solution {
    /**
     * @param L: Given n pieces of wood with length L[i]
     * @param k: An integer
     * @return: The maximum length of the small pieces
     */
    public int woodCut(int[] L, int k) {
        // write your code here
        int start = 1, end = 0, result = 0;
        for (int i : L) {
            end = Math.max(end, i);
        }

        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (count(L, mid) >= k) {
                start = mid;
            } else {
                end = mid;
            }
        }


        if (count(L, end) >= k) {
            return end;
        } else if (count(L, start) >= k) {
            return start;
        } else {
            return 0;
        }
    }

    private int count(int[] L, int len) {
        int result = 0;
        for (int i : L) {
            result += i / len;
        }
        return result;

    }
}
```
