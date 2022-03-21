---
description: LeetCode 267
---

# Palindrome Permutation II

{% embed url="https://leetcode.com/problems/palindrome-permutation-ii/" %}

Given a string s, return _all the palindromic permutations (without duplicates) of it_.

You may return the answer in **any order**. If `s` has no palindromic permutation, return an empty list.

**Example 1:**

```
Input: s = "aabb"
Output: ["abba","baab"]
```

**Example 2:**

```
Input: s = "abc"
Output: []
```

**Constraints:**

* `1 <= s.length <= 16`
* `s` consists of only lowercase English letters.

{% hint style="info" %}
用HashMap统计所有character出现的频率，如果频率为odd的character大于1，那么返回🔙空，如果等于1或者小于1，那么就可以得到palindrome。

如果odd == 1，通过map找到odd == 1的character，设为mid

想要找到palindrome的permutation，只需要找到前一半的permutation，然后加上中间的character，后一半的permutation可以通过前一半的reverse得到。所以要遍历map，要permutation的部分是一半，把这一半加入到list中，通过递归找到permutation的集合。再做字符串拼接。
{% endhint %}

```java
class Solution {
    public List<String> generatePalindromes(String s) {
        if (s == null || s.length() == 0) {
            return new ArrayList<>();
        }
        
        String mid = "";
        int odd = 0;
        Map<Character, Integer> map = new HashMap<>();
        boolean[] visited = new boolean[s.length()];
        StringBuilder sb = new StringBuilder();
        List<Character> list = new ArrayList<>();
        List<String> result = new ArrayList<>();
        
        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
            if (map.get(ch) % 2 != 0) {
                odd++;
            } else {
                odd--;
            }
        }
        
        if (odd > 1) {
            return result;
        }
        
        for (Map.Entry<Character, Integer> entry : map.entrySet()) {
            char key = entry.getKey();
            int val = entry.getValue();
            if (val % 2 != 0) {
                mid += key;
            }
            for (int i = 0; i < val / 2; ++i) {
                list.add(key);
            }
        }
        dfs(mid, sb, visited, list, result);
        return result;
    }
    
    private void dfs(String mid,
                    StringBuilder sb,
                    boolean[] visited,
                    List<Character> list,
                    List<String> result) {
        if (sb.length() == list.size()) {
            result.add(sb.toString() + mid + sb.reverse().toString());
            sb.reverse();
            return;
        }
        
        for (int i = 0; i < list.size(); ++i) {
            if (visited[i]) continue;
            if (i > 0 && visited[i - 1] && list.get(i) == list.get(i - 1)) continue;
            visited[i] = true;
            sb.append(list.get(i));
            dfs(mid, sb, visited, list, result);
            visited[i] = false;
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```
