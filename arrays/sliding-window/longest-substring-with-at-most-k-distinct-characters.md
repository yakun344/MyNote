---
description: LeetCode 340
---

# Longest Substring with At Most K Distinct Characters

{% embed url="https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/" %}



Given a string `s` and an integer `k`, return _the length of the longest substring of_ `s` _that contains at most_ `k` _**distinct** characters_.

**Example 1:**

```
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.
```

**Example 2:**

```
Input: s = "aa", k = 1
Output: 2
Explanation: The substring is "aa" with length 2.
```

**Constraints:**

* `1 <= s.length <= 5 * 104`
* `0 <= k <= 50`

{% hint style="info" %}
Sliding Window.

map中存char -> frequency

右边指针向前移动，并且更新map。

当窗口大小大于k的时候移动left指针，并且更新map和count，直到count <= k。

更新长度的大小。
{% endhint %}

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        int len = Integer.MIN_VALUE;
        int left = 0, right = 0;
        int count = 0;
        Map<Character, Integer> map = new HashMap<>();
        while (right < s.length()) {
            char ch = s.charAt(right);
            right++;
            map.put(ch, map.getOrDefault(ch, 0) + 1);
            if (map.get(ch) == 1) {
                count++;
            }
            
            while (count > k) {
                char cur = s.charAt(left);
                map.put(cur, map.get(cur) - 1);
                if (map.get(cur) == 0) {
                    count--;
                }
                left++;
            }
            
            len = Math.max(len, right - left);
            
        }
        return len;
    }
}
```
