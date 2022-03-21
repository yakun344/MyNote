---
description: LeetCode 1647
---

# Minimum Deletions to Make Character Frequencies Unique

{% embed url="https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique" %}



A string `s` is called **good** if there are no two different characters in `s` that have the same **frequency**.

Given a string `s`, return _the **minimum** number of characters you need to delete to make_ `s` _ **good**._

The **frequency** of a character in a string is the number of times it appears in the string. For example, in the string `"aab"`, the **frequency** of `'a'` is `2`, while the **frequency** of `'b'` is `1`.

&#x20;

**Example 1:**

```
Input: s = "aab"
Output: 0
Explanation: s is already good.
```

**Example 2:**

```
Input: s = "aaabbbcc"
Output: 2
Explanation: You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".
```

**Example 3:**

```
Input: s = "ceabaacb"
Output: 2
Explanation: You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
```

&#x20;

**Constraints:**

* `1 <= s.length <= 105`
* `s` contains only lowercase English letters.

{% hint style="info" %}
统计每个字母出现的频率。

扫一遍数组，当前频率能不能加入set中，不能的话一直递减，同时更新res。
{% endhint %}

```java
class Solution {
    public int minDeletions(String s) {
        int[] arr = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            arr[s.charAt(i) - 'a']++;
        }
        
        int res = 0;
        Set<Integer> used = new HashSet<>();
        for (int i = 0; i < 26; ++i) {
            while (arr[i] > 0 && !used.add(arr[i])) {
                arr[i]--;
                res++;
            }
        }
        
        return res;
    }
}
```
