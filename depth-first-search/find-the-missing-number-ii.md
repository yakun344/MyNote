---
description: LintCode 570
---

# Find the Missing Number II

{% embed url="https://www.lintcode.com/problem/570/" %}



Description

To give a random string sequence composed of 1 - `n` integers, in which an integer is lost, please find it.

n < 100\
Data guarantees have only one solution.\
if the list that you've found has more than one missing numbers, which could be that you didn't find the correct way to split the string.Example

**Example1**

```
Input: n = 20 and str = 19201234567891011121314151618
Output: 17
Explanation:
19'20'1'2'3'4'5'6'7'8'9'10'11'12'13'14'15'16'18
```

**Example2**

```
Input: n = 6 and str = 56412
Output: 3
Explanation:
5'6'4'1'2
```

```java
public class Solution {
    /**
     * @param n: An integer
     * @param str: a string with number from 1-n in random order and miss one number
     * @return: An integer
     */
    private int missed = -1;
    public int findMissing2(int n, String str) {
        // write your code here
        boolean[] visited = new boolean[n + 1];
        dfs(visited, str, n, 0);
        return missed;
    }

    private void dfs(boolean[] visited, String str, int n, int start) {
        if (str.length() == 0 && start == n - 1) {
            for (int i = 1; i <= n; ++i) {
                if (!visited[i]) {
                    missed = i;
                    return;
                }
            }
        }

        for (int i = 1; i <= 2 && i <= str.length(); ++i) {
            int num = Integer.parseInt(str.substring(0, i));
            if (num == 0 || num > n || visited[num]) {
                continue;
            }

            visited[num] = true;
            dfs(visited, str.substring(i, str.length()), n, start + 1);
            visited[num] = false;
        }
    }
}
```
