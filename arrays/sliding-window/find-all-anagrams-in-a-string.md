---
description: LeetCode 438
---

# Find All Anagrams in a String

{% embed url="https://leetcode.com/problems/find-all-anagrams-in-a-string/" %}



Given two strings `s` and `p`, return _an array of all the start indices of_ `p`_'s anagrams in_ `s`. You may return the answer in **any order**.

**Example 1:**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

**Constraints:**

* `1 <= s.length, p.length <= 3 * 104`
* `s` and `p` consist of lowercase English letters.

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int count = p.length();
        int[] nums = new int[26];
        for (char ch : p.toCharArray()) {
            nums[ch - 'a']++;
        }
        List<Integer> res = new ArrayList<>();
        int left = 0, right = 0;
        while (left < s.length()) {
            while (right < s.length() && right - left < p.length()) {
                char ch = s.charAt(right);
                if (nums[ch - 'a'] > 0) {
                    count--;
                }
                nums[ch - 'a']--;
                
                right++;
            }
            
            if (count == 0) {
                res.add(left);
            }
            
            char ch = s.charAt(left);
            if (nums[ch - 'a'] >= 0) {
                count++;
            }
            nums[ch - 'a']++;
            left++;
        }
        
        return res;
    }
}
```
