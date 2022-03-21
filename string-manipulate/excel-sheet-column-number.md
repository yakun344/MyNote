---
description: LeetCode 171
---

# Excel Sheet Column Number

{% embed url="https://leetcode.com/problems/excel-sheet-column-number" %}

Given a string `columnTitle` that represents the column title as appear in an Excel sheet, return _its corresponding column number_.

For example:

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

&#x20;

**Example 1:**

```
Input: columnTitle = "A"
Output: 1
```

**Example 2:**

```
Input: columnTitle = "AB"
Output: 28
```

**Example 3:**

```
Input: columnTitle = "ZY"
Output: 701
```

**Example 4:**

```
Input: columnTitle = "FXSHRXW"
Output: 2147483647
```

&#x20;

**Constraints:**

* `1 <= columnTitle.length <= 7`
* `columnTitle` consists only of uppercase English letters.
* `columnTitle` is in the range `["A", "FXSHRXW"]`.

```java
class Solution {
    public int titleToNumber(String s) {
        int result = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            result = result * 26;
            // In Java, subtracting characters is subtracting ASCII values of characters
            result += (s.charAt(i) - 'A' + 1);
        }
        return result;
    }
}
```
