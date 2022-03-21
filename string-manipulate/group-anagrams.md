---
description: LeetCode 49
---

# Group Anagrams

{% embed url="https://leetcode.com/problems/group-anagrams" %}

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

&#x20;

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**

```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**

```
Input: strs = ["a"]
Output: [["a"]]
```

&#x20;

**Constraints:**

* `1 <= strs.length <= 104`
* `0 <= strs[i].length <= 100`
* `strs[i]` consists of lowercase English letters.

{% hint style="info" %}
anagram的要求是word中每个字母出现的频率一样，就可以是anagram。

用hashmap统计每个单词的频率计作key，如果在map中出现过，那么当前单词就是anagram，把当前单词放到key对应的value里。

结果返回map中所有的value
{% endhint %}

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) {
            return new ArrayList<>();
        }
        
        Map<String, List<String>> map = new HashMap<>();
        int[] count = new int[26];
        
        for (String str : strs) {
            Arrays.fill(count, 0);
            for (char ch : str.toCharArray()) {
                count[ch - 'a']++;
            }
            
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; ++i) {
                sb.append(".");
                sb.append(count[i]);
            }
            
            String key = sb.toString();
            if (!map.containsKey(key)) {
                map.put(key, new ArrayList<>());
            }
            
            map.get(key).add(str);
        }
        
        return new ArrayList(map.values());
    }
}
```
