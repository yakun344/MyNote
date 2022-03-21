---
description: LeetCode 93 LintCode 426
---

# Restore IP Addresses

{% embed url="https://leetcode.com/problems/restore-ip-addresses/" %}

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

**Example:**

```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```

**Complexity Analysis**

* Time complexity : as discussed above, there is not more than `27` combinations to check.
* Space complexity : constant space to keep the solutions, not more than `19` valid IP addresses.

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> results = new ArrayList<>();
        if (s.length() < 4 || s.length() > 12) {
            return results;
        }
        List<String> path = new ArrayList<>();
        
        search(0, s, path, results);
        return results;
    }
    
    private void search(int index, String s, List<String> path, List<String> results) {
        //退出条件是path集齐4个字符串段
        if (path.size() == 4) {
            if (index != s.length()) {
                return;
            }
            
            StringBuffer sb = new StringBuffer();
            for (String str : path) {
                sb.append(str);
                sb.append(".");
            }
            sb.deleteCharAt(sb.length() - 1);
            results.add(sb.toString());
            return;
        }
        
        //每次循环字符串中的3个char
        for (int i = index; i < s.length() && i < index + 3; ++i) {
            String str = s.substring(index, i + 1);
            if (isValid(str)) {
                path.add(str);
                search(i + 1, s, path, results);
                path.remove(path.size() - 1);
            }
        }
    }
    
    private boolean isValid(String s) {
        //这种情况避免00，10的存在
        if (s.charAt(0) == '0') {
            return s.equals("0");
        }
        
        //检查是否是合法的ip
        int digit = Integer.valueOf(s);
        return digit >= 0 && digit <= 255;
    }
}
```

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        if (s.length() < 4 || s.length() > 12) {
            return new ArrayList<>();
        }
        
        List<String> res = new ArrayList<>();
        List<String> paths = new ArrayList<>();
        
        dfs(res, paths, s, 0);
        
        return res;
    }
    
    private void dfs(List<String> res,
                    List<String> paths,
                    String s,
                    int start) {
        if (paths.size() == 4) {
            if (start != s.length()) {
                return;
            }
            
            StringBuilder sb = new StringBuilder();
            for (String str : paths) {
                sb.append(str);
                sb.append(".");
            }
            
            sb.deleteCharAt(sb.length() - 1);
            res.add(sb.toString());
            
            return;
        }
        
        for (int i = start; i < start + 3 && i < s.length(); ++i) {
            String str = s.substring(start, i + 1);
            if (isValid(str)) {
                paths.add(str);
                dfs(res, paths, s, i + 1);
                paths.remove(paths.size() - 1);
            }
        }
    }
    
    private boolean isValid(String s) {
        if (s.startsWith("0")) {
            return s.equals("0");
        }
        
        int num = Integer.valueOf(s);
        return num >= 0 && num <= 255;
    }
}
```
