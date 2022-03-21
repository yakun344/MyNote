---
description: LeetCode 76
---

# Minimum Window Substring

{% embed url="https://leetcode.com/problems/minimum-window-substring/" %}

Given two strings `s` and `t`, return _the minimum window in `s` which will contain all the characters in `t`_. If there is no such window in `s` that covers all characters in `t`, return _the empty string `""`_.

**Note** that If there is such a window, it is guaranteed that there will always be only one unique minimum window in `s`.

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
```

**Constraints:**

* `1 <= s.length, t.length <= 105`
* `s` and `t` consist of English letters.

&#x20;**Follow up:** Could you find an algorithm that runs in `O(n)` time?

{% hint style="info" %}
Sliding Window双指针，left和right从左往右移动，维持一个map来记录t里要使用的char应该出现的次数。

right先移动，同时更新map和source count，当source count为t count的时候，代表已经找到了left -> right中的substring包含t；然后left向右移动，缩小长度，找到最小长度的substring。
{% endhint %}

```java
class Solution {
    public String minWindow(String s, String t) {
        int tc = t.length();
        Map<Character, Integer> map = new HashMap<>();
        for (char c : t.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        
        int sc = 0;
        String res = "";
        int minLen = Integer.MAX_VALUE;
        
        int left = 0, right = 0;
        while (left < s.length()) {
            while (right < s.length() && sc < tc) {
                char ch = s.charAt(right);
                if (map.containsKey(ch)) {
                    //get(ch) > 0保证当前的ch要被放入window
                    //并且更新source count
                    //当get(ch) = 0时，当前的ch不需要计入window
                    if (map.get(ch) > 0) {
                        sc++;
                    }
                    map.put(ch, map.get(ch) - 1);
                }
                right++;
            }
            
            //这里right指针指向sliding window的下一个字母。
            //看👆right++；
            if (sc >= tc) {        //sc == tc更严格也可以ac
                if (minLen > right - left) {
                    minLen = right - left;
                    res = s.substring(left, right);
                }
            }
            
            char ch = s.charAt(left);
            if (map.containsKey(ch)) {
            
                // >= 0保证window里的ch是都要被使用的
                //如果 < 0 证明window里有多余的char
                //多余的这个char可以被删除(即map.get(ch) - 1)
                //此时不影响source count的大小
                if (map.get(ch) >= 0) {    //map.get(ch) == 0 也可以ac
                    sc--;
                }
                map.put(ch, map.get(ch) + 1);
            }
            
            left++;
        }
        
        return res;
    }
}
```
