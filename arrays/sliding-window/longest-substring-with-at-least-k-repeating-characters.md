---
description: LeetCode 395
---

# Longest Substring with At Least K Repeating Characters

{% embed url="https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters" %}

Given a string `s` and an integer `k`, return _the length of the longest substring of_ `s` _such that the frequency of each character in this substring is greater than or equal to_ `k`.

&#x20;

**Example 1:**

```
Input: s = "aaabb", k = 3
Output: 3
Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input: s = "ababbc", k = 2
Output: 5
Explanation: The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

&#x20;

**Constraints:**

* `1 <= s.length <= 104`
* `s` consists of only lowercase English letters.
* `1 <= k <= 105`

![](<../../.gitbook/assets/image (45).png>)

```java
public class Solution {
    public int longestSubstring(String s, int k) {
        char[] str = s.toCharArray();
        int[] countMap = new int[26];
        int maxUnique = getMaxUniqueLetters(s);
        int result = 0;
        for (int currUnique = 1; currUnique <= maxUnique; currUnique++) {
            // reset countMap
            Arrays.fill(countMap, 0);
            int windowStart = 0, windowEnd = 0, idx = 0, unique = 0, countAtLeastK = 0;
            while (windowEnd < str.length) {
                // expand the sliding window
                if (unique <= currUnique) {
                    idx = str[windowEnd] - 'a';
                    if (countMap[idx] == 0) unique++;
                    countMap[idx]++;
                    if (countMap[idx] == k) countAtLeastK++;
                    windowEnd++;
                }
                // shrink the sliding window
                else {
                    idx = str[windowStart] - 'a';
                    if (countMap[idx] == k) countAtLeastK--;
                    countMap[idx]--;
                    if (countMap[idx] == 0) unique--;
                    windowStart++;
                }
                if (unique == currUnique && unique == countAtLeastK)
                    result = Math.max(windowEnd - windowStart, result);
            }
        }

        return result;
    }

    // get the maximum number of unique letters in the string s
    public int getMaxUniqueLetters(String s) {
        boolean map[] = new boolean[26];
        int maxUnique = 0;
        for (int i = 0; i < s.length(); i++) {
            if (!map[s.charAt(i) - 'a']) {
                maxUnique++;
                map[s.charAt(i) - 'a'] = true;
            }
        }
        return maxUnique;
    }
}
```

```java
class Solution {
    public int longestSubstring(String s, int k) {
        int count = getUnique(s, k);
        int res = 0;
        for (int i = 1; i <= count; ++i) {
            int[] map = new int[26];
            int mostK = 0, cur = 0;
            int l = 0, r = 0;
            while (r < s.length()) {
                if (cur <= i) {
                    if (map[s.charAt(r) - 'a'] == 0) {
                        cur++;
                    }
                    map[s.charAt(r) - 'a']++;
                    
                    if (map[s.charAt(r) - 'a'] == k) {
                        mostK++;
                    }
                    
                    r++;
                } else {
                    if (map[s.charAt(l) - 'a'] == k) {
                        mostK--;
                    }
                    map[s.charAt(l) - 'a']--;
                    if (map[s.charAt(l) - 'a'] == 0) {
                        cur--;
                    }
                    l++;
                }
                
                if (cur == i && cur == mostK) {
                    res = Math.max(res, r - l);
                }
            }
        }
        return res;
    }
    
    private int getUnique(String s, int k) {
        boolean[] map = new boolean[26];
        int res = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (!map[s.charAt(i) - 'a']) {
                res++;
                map[s.charAt(i) - 'a'] = true;
            }
        }
        return res;
    }
}
```
