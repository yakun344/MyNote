---
description: LeetCode 17 LintCode 425
---

# Letter Combinations of a Phone Number

{% embed url="https://leetcode.com/problems/letter-combinations-of-a-phone-number/" %}

{% embed url="https://www.lintcode.com/problem/letter-combinations-of-a-phone-number/description" %}

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.

{% hint style="info" %}
||语句的先后顺序，空字符串代表的是长度为0的字符串，字符串==null代表reference没有指向内存的地方。
{% endhint %}

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        String[] phone = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        
        List<String> results = new ArrayList<>();
        if (digits.length() == 0 || digits == null) {
            return results;
        }
        
        //base case;
        dfs(0, digits.length(), "", digits, phone, results);
        
        return results;
    }
    
    private void dfs(int start,
                    int len,
                    String cur,
                    String digits,
                    String[] phone,
                    List<String> results) {
        if (start == len) {
            results.add(cur);
            return;
        }
        
        int distance = digits.charAt(start) - '0';
        for (char c : phone[distance].toCharArray()) {
            dfs(start + 1, len, cur + c, digits, phone, results);
        }
    }
}
```

```java
public class Solution {
    /**
     * @param digits: A digital string
     * @return: all posible letter combinations
     */
    public List<String> letterCombinations(String digits) {
        // write your code here
        if (digits == null | digits.length() == 0) {
            return new ArrayList<>();
        }

        String[] phone = new String[]{"",
                                    "",
                                    "abc",
                                    "def",
                                    "ghi",
                                    "jkl",
                                    "mno",
                                    "pqrs",
                                    "tuv",
                                    "wxyz"};
        List<String> res = new ArrayList<>();
        StringBuilder sb = new StringBuilder();

        dfs(res, sb, phone, digits, 0);

        return res;
    }

    private void dfs(List<String> res,
                    StringBuilder sb,
                    String[] phone,
                    String digits,
                    int index) {
        if (index == digits.length()) {
            res.add(sb.toString());
            return;
        }

        int distance = digits.charAt(index) - '0';
        for (char ch : phone[distance].toCharArray()) {
            sb.append(ch);
            dfs(res, sb, phone, digits, index + 1);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```
