---
description: LeetCode 127
---

# Word Ladder

{% hint style="danger" %}
The constructor of String.
{% endhint %}

{% embed url="https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#String()" %}

{% embed url="https://leetcode.com/problems/word-ladder/" %}



Given two words (_beginWord_ and _endWord_), and a dictionary's word list, find the length of shortest transformation sequence from _beginWord_ to _endWord_, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.

**Note:**

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume _beginWord_ and _endWord_ are non-empty and are not the same.

**Example 1:**

```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

**Example 2:**

```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

{% hint style="info" %}
1. 处理特殊情况，wordList为空的情况和beginWord == endWord的情况
2. 需要把输入的wordList转为Set\<String>， 这样有助于判断当前string的neighbor在不在wordList里。
3. 维持一个visited set。
4. 用step记录层级遍历的层数。
5. 寻找string的neighbor的时候，要替换每一个字符，去掉重复，并且看替换的字符在不在wordList里。

注意⚠️：不需要把beginWord加入到wordList里。

Time Complexity: O({M}^2 \times N)O(M 2 ×N), where MM is the length of each word and NN is the total number of words in the input word list.
{% endhint %}

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (wordList == null || wordList.size() == 0) {
            return 0;
        }
        
        if (beginWord.equals(endWord)) {
            return 1;
        }
        
        int step = 1;
        Set<String> set = new HashSet<>(wordList);
        Set<String> visited = new HashSet<>();
        Deque<String> queue = new LinkedList<>();
        
        visited.add(beginWord);
        queue.addLast(beginWord);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                String str = queue.removeFirst();
                for (String neighbor : getNeighbor(str, set)) {
                    if (visited.contains(neighbor)) {
                        continue;
                    }
                    if (neighbor.equals(endWord)) {
                        return ++step;
                    }
                    visited.add(neighbor);
                    queue.addLast(neighbor);
                }
            }
            
            step++;
        }
        
        return 0;
    }
    
    private Set<String> getNeighbor(String str, Set<String> set) {
        Set<String> res = new HashSet<>();
        for (int i = 0; i < str.length(); ++i) {
            for (char c = 'a'; c <= 'z'; ++c) {
                if (c == str.charAt(i)) continue;
                String temp = replace(i, c, str);
                if (set.contains(temp)) {
                    res.add(temp);
                }
            }
        }
        
        return res;
    }
    
    private String replace(int i, char c, String str) {
        char[] arr = str.toCharArray();
        arr[i] = c;
        return new String(arr);
    }
}
```
