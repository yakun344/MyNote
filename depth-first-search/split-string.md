---
description: LintCode 680
---

# Split String

{% embed url="https://www.lintcode.com/problem/split-string/description" %}

Give a string, you can choose to split the string after one character or two adjacent characters, and make the string to be composed of only one character or two characters. Output all possible results.

#### Example

**Example1**

```
Input: "123"
Output: [["1","2","3"],["12","3"],["1","23"]]
```

**Example2**

```
Input: "12345"
Output: [["1","23","45"],["12","3","45"],["12","34","5"],["1","2","3","45"],["1","2","34","5"],["1","23","4","5"],["12","3","4","5"],["1","2","3","4","5"]]
```

Time Complexity: O(2^n)

```java
public class Solution {
    /*
     * @param : a string to be split
     * @return: all possible split string array
     */
    public List<List<String>> splitString(String s) {
        // write your code here
        List<List<String>> results = new ArrayList<>();
        if (s == null) return results;
        List<String> sub = new ArrayList<>();
        dfs(s, 0, sub, results);
        
        return results;
    }
    
    private void dfs(String s, int index, List<String> sub, List<List<String>> results) {
        if (index == s.length()) {
            results.add(new ArrayList<>(sub));
            return;
        }
        
        //1st option
        sub.add(String.valueOf(s.charAt(index)));
        dfs(s, index + 1, sub, results);
        sub.remove(sub.size() - 1);
        
        //2nd option(remember how to consider the edge case)
        if (index < s.length() - 1) {
            sub.add(s.substring(index, index + 2));
            dfs(s, index + 2, sub, results);
            
            //this string's size is 1, consist of two chars;
            sub.remove(sub.size() - 1);
        }
    }
}
```
