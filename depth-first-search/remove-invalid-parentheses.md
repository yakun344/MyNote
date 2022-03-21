---
description: LeetCode 301
---

# Remove Invalid Parentheses

{% embed url="https://leetcode.com/problems/remove-invalid-parentheses/" %}



Given a string `s` that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.

Return _all the possible results_. You may return the answer in **any order**.

**Example 1:**

```
Input: s = "()())()"
Output: ["(())()","()()()"]
```

**Example 2:**

```
Input: s = "(a)())()"
Output: ["(a())()","(a)()()"]
```

**Example 3:**

```
Input: s = ")("
Output: [""]
```

**Constraints:**

* `1 <= s.length <= 25`
* `s` consists of lowercase English letters and parentheses `'('` and `')'`.
* There will be at most `20` parentheses in `s`.

#### BFS Approach

{% hint style="info" %}
删除最小数目的无效括号，使得输入字符串有效，那么我们考虑一个个地去删除字符，直到出现有效字符串

* 用队列维护bfs
* 每次取队头进行check
* 如果队头元素是有效字符串，那么没必要去搜索更短的串，用一个flag标记continue，检查剩余相同长度的string，把合法的string加入到res
* 每次for循环，都尝试着删除一个字符，加入到queue中。
* 如果依然无效字符串，对于这个字符串，删除其中一位压入队列
* 用一个set做剪枝，对于已经check过的字符串，直接跳过，
* check函数用来判断是否有效，用一个count记录'（'和'）'的个数遇到'('则count++反之则count--，若中途出现count小于0的情况，则说明字符串无效，若最后count！=0则字符串无效

**复杂度分析**

* 时间复杂度`O(2^n)`
  * n为字符串长度，最坏情况复杂度为m^2
* 空间复杂度`O(n^2)`
  * n为字符串长度
{% endhint %}

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> res = new ArrayList<>();
        
        if (s == null) {
            return res;
        }
        if (s.length() == 0) {
            res.add("");
            return res;
        }
        
        boolean reached = false;
        Deque<String> queue = new LinkedList<>();
        Set<String> set = new HashSet<>();
        
        queue.addLast(s);
        set.add(s);
        while (!queue.isEmpty()) {
            String curStr = queue.removeFirst();
            
            if (isValid(curStr)) {
                res.add(curStr);
                reached = true;
            }
            
            if (reached) {
                continue;
            }
            
            for (int i = 0; i < curStr.length(); ++i) {
                if (curStr.charAt(i) != '(' && curStr.charAt(i) != ')') {
                    continue;
                }
                
                String str1 = curStr.substring(0, i);
                String str2 = curStr.substring(i + 1);
                String str3 = str1 + str2;
                
                if (!set.contains(str3)) {
                    set.add(str3);
                    queue.addLast(str3);
                }
            }
        }
        
        return res;
    }
    
    private boolean isValid(String str) {
        if (str == null || str.length() == 0) {
            return true;
        }
        int count = 0;
        for (int i = 0; i < str.length(); ++i) {
            if (str.charAt(i) == '(') {
                count++;
            }
            
            if (str.charAt(i) == ')') {
                count--;
            }
            
            //if判断语句保证左括号'('一定要在前面，如果是')'在前，那么不valid
            //例如：)((),直接return false
            //     ()()，return true
            if (count < 0) {
                return false;
            }
        }
        
        return count == 0;
    }
}
```

#### DFS Approach

{% hint style="info" %}
1.从左到右遍历String获得左括号和右括号的计数

2.从左到右遍历String，如果碰到左括号或右括号，就删除左括号或右括号，更新计数，然后把剩下的String放入DFS递归，注意当有连续的左括号或者右括号的时候，必须先删前面的

3.DFS退出的条件是左括号和右括号的计数均为0

注意⚠️记录count的时候，一定要先匹配左括号，比如)((())，如果没匹配左括号那么

count数组就是\[0, 0]；如果先匹配括号，那么count数组就是\[1, 1]。循环退出条件是count数组为\[0, 0]。
{% endhint %}

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        if (s == null || s.length() == 0) {
            return new ArrayList<>();
        }
        
        List<String> res = new ArrayList<>();
        int[] count = getCount(s);
        
        dfs(res, s, count[0], count[1], 0);
        
        return res;
    }
    
    private void dfs(List<String> res,
                    String s,
                    int left,
                    int right,
                    int startIndex) {
        if (left == 0 && right == 0 && isValid(s)) {
            res.add(s);
            return;
        }
        
        for (int i = startIndex; i < s.length(); ++i) {
            if (i > startIndex && s.charAt(i) == s.charAt(i - 1)) {
                continue;
            }
            String str1 = s.substring(0, i);
            String str2 = s.substring(i + 1);
            String str3 = str1 + str2;
            
            if (left > 0 && s.charAt(i) == '(') {
                dfs(res, str3, left - 1, right, i);    
            }
            if (right > 0 && s.charAt(i) == ')') {
                dfs(res, str3, left, right - 1, i);
            }
        }
    }
    
    private int[] getCount(String s) {
        int[] count = new int[2];
        for (char ch : s.toCharArray()) {
            if (ch == '(') {
                count[0]++;
            }
            if (ch == ')') {
                if (count[0] > 0) {
                    count[0]--;
                } else {
                    count[1]++;
                }
            }
        }
        return count;
    }
    
    private boolean isValid(String s) {
        int[] count = getCount(s);
        return count[0] == 0 && count[1] == 0;
    }
}
```
