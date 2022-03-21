---
description: LeetCode 1456
---

# Maximum Number of Vowels in a Substring of Given Length

Given a string `s` and an integer `k`.

Return _the maximum number of vowel letters_ in any substring of `s` with length `k`.

**Vowel letters** in English are (a, e, i, o, u).

**Example 1:**

```
Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.
```

**Example 2:**

```
Input: s = "aeiou", k = 2
Output: 2
Explanation: Any substring of length 2 contains 2 vowels.
```

**Example 3:**

```
Input: s = "leetcode", k = 3
Output: 2
Explanation: "lee", "eet" and "ode" contain 2 vowels.
```

**Example 4:**

```
Input: s = "rhythms", k = 4
Output: 0
Explanation: We can see that s doesn't have any vowel letters.
```

**Example 5:**

```
Input: s = "tryhard", k = 4
Output: 1
```

**Constraints:**

* `1 <= s.length <= 10^5`
* `s` consists of lowercase English letters.
* `1 <= k <= s.length`

#### Sliding Window

```java
class Solution {
    public int maxVowels(String s, int k) {
        Set<Character> set = new HashSet<>(Arrays.asList('a','e','i','o','u'));
        int cur = 0, max = 0, left = 0, right = -1;
        while (true) {
            if (right - left + 1 < k) {
                right++;
                if (right >= s.length()) break;
                if (set.contains(s.charAt(right))) {
                    cur++;
                    max = Math.max(max, cur);
                }
            } else {
                if (set.contains(s.charAt(left))) {
                    cur--;
                }
                left++;
            }
        }
        return max;
    }
}
```
