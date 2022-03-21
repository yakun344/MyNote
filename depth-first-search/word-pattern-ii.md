---
description: LeetCode 291 LintCode 829
---

# Word Pattern II

{% embed url="https://leetcode.com/problems/word-pattern-ii/" %}

{% embed url="https://www.lintcode.com/problem/word-pattern-ii/description" %}

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** substring in `str`.

**Example 1:**

```
Input: pattern = "abab", str = "redblueredblue"
Output: true
```

**Example 2:**

```
Input: pattern = pattern = "aaaa", str = "asdasdasdasd"
Output: true
```

**Example 3:**

```
Input: pattern = "aabb", str = "xyzabcxzyabc"
Output: false
```

**Notes:**\
You may assume both `pattern` and `str` contains only lowercase letters.

{% hint style="info" %}
用一个hash map记录pattern和s的mapping关系，用hash set记录重复出现的s。

遍历到pattern 和s 的长度都为0为止，这时候返回true。

每次从pattern里拿出一个字母，尝试着去匹配剩余s长度的string，如果能匹配到的话就返回true；如果匹配不到的话返回false。

如果map里已经有当前拿出的char，判断s里剩余的string是不是以已经mapping过char的string开头，如果不是的话就返回false；如果匹配成功，则返回char之后，string之后的匹配关系。

如果map里没有当前的char，证明当前的char是新的，之前没见过。

这种情况下，尝试着用当前的char去匹配s之后的所有情况， backtracking。

没找到的话return false。
{% endhint %}

```java
class Solution {
    public boolean wordPatternMatch(String pattern, String str) {
        Map<Character, String> map = new HashMap<>();
        Set<String> set = new HashSet<>();
        
        return search(pattern, str, map, set);
    }
    
    private boolean search(String pattern, String str, Map<Character, String> map, Set<String> set) {
        if (pattern.length() == 0) {
            return str.length() == 0;
        }
        
        Character ch = pattern.charAt(0);
        if (map.containsKey(ch)) {
            if (!str.startsWith(map.get(ch))) {
                return false;
            }
            
            return search(pattern.substring(1), str.substring(map.get(ch).length()), map, set);
        }
        for (int i = 0; i < str.length(); ++i) {
            String word = str.substring(0, i + 1);
            
            if (set.contains(word)) {
                continue;
            }
            
            map.put(ch, word);
            set.add(word);
            if (search(pattern.substring(1), str.substring(i + 1), map, set)) {
                return true;
            }
            map.remove(ch);
            set.remove(word);
        }
        
        return false;
    }
}
```

```java
class Solution {
    public boolean wordPatternMatch(String pattern, String s) {
        Map<Character, String> map = new HashMap<>();
        Set<String> set = new HashSet<>();
        
        return dfs(pattern, s, map, set);
    }
    
    private boolean dfs(String pattern,
                       String s,
                       Map<Character, String> map,
                       Set<String> set) {
        if (pattern.length() == 0) {
            return s.length() == 0;
        }
        
        char ch = pattern.charAt(0);
        if (map.containsKey(ch)) {
            if (!s.startsWith(map.get(ch))) {
                return false;
            }
            
            return dfs(pattern.substring(1), s.substring(map.get(ch).length()), map,set);
        }
        
        for (int i = 0; i < s.length(); ++i) {
            String str = s.substring(0, i + 1);
            
            if (set.contains(str)) continue;
            
            set.add(str);
            map.put(ch, str);
            if (dfs(pattern.substring(1), s.substring(i + 1), map, set)) {
                return true;
            }
            set.remove(str);
            map.remove(ch);
        }
        
        return false;
    }
}
```
