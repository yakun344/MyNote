---
description: LeetCode 159
---

# Longest Substring with At Most Two Distinct Characters

{% embed url="https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/" %}



Given a string `s`, return _the length of the longest substring that contains at most **two distinct characters**_.

**Example 1:**

```
Input: s = "eceba"
Output: 3
Explanation: The substring is "ece" which its length is 3.
```

**Example 2:**

```
Input: s = "ccaabbb"
Output: 5
Explanation: The substring is "aabbb" which its length is 5.
```

**Constraints:**

* `1 <= s.length <= 104`
* `s` consists of English letters.

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        Map<Character, Integer> map = new HashMap<>();
        
        int left = 0, right = 0;
        int len = Integer.MIN_VALUE;
        int count = 0;
        while (right < s.length()) {
            char ch = s.charAt(right);
            map.put(ch, map.getOrDefault(ch, 0) + 1);
            if (map.get(ch) == 1) {
                count++;
            }
            
            right++;
            
            while (count > 2) {
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
