# 函数式编程

在一个好的编程风格中，将部分独立的逻辑函数化是一个重要的手段。函数化有如下的一些好处：

1. 代码可读性更高
2. 不容易写出 Bug
3. 写出 Bug 之后也很容易进行局部测试进行 Debug

比如，在使用 O(n3) 的枚举法解决最长回文子串（Longest Palindromic Substring）的问题中，我们需要判断一个子字符串是否是一个回文串，没有进行函数化的写法为：

Java:

```java
public int longestPalindrome(String s) {
    int longest = 0;
    for (int i = 0; i < s.length(); i++) {
        for (int j = i; j < s.length(); j++) {
            int left = i, right = j;
            while (left <= right && s.charAt(left) == s.charAt(right)) {
                left++;
                right--;
            }
            if (left > right) {
                longest = Math.max(longest, j - i + 1);
            }
        }
    }
		return longest;
}
```

Python:

```python
def longestPalindrome(self):
    longest = 0
    for i in range(len(s)):
        for j in range(i, len(s)):
            left, right = i, j
            while left <= right and s[left] == s[right]:
                left += 1
                right -= 1
            if left > right:
                longest = max(longest, j-i+1)
    return longest
```

C++:

```cpp
int longestPalindrome(const string& s) {
    int longest = 0;
    for (int i = 0; i < s.size(); i++) {
        for (int j = i; j < s.size(); j++) {
            int left = i, right = j;
            while (left <= right && s[left] == s[right]) {
                left++;
                right--;
            }
            if (left > right) {
                longest = max(longest, j - i + 1);
            }
        }
    }
    return longest;
}
```

我们将判断 s 的某一段区间是否为一个回文串的部分包装为一个子函数之后，得到函数化的代码结构如下：\
Java:

```java
public int longestPalindrome(String s) {
    int longest = 0;
    for (int i = 0; i < s.length(); i++) {
        for (int j = i; j < s.length(); j++) {
            if (isPalindrome(s, i, j)) {
                longest = Math.max(longest, j - i + 1);
            }
        }
    }
		return longest;
}

private boolean isPalindrome(String s, int left, int right) {
    while (left <= right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

Python:

```python
def longestPalindrome(s):
    longest = 0
    for i in range(len(s)):
        for j in range(i, len(s)):
            if isPalindrome(s, i, j):
                longest = max(longest, j-i+1)
    return longest
                
def isPalindrome(s, left, right):
    while left <= right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True
```

C++:

```cpp
int longestPalindrome(const string& s) {
    int longest = 0;
    for (int i = 0; i < s.size(); i++) {
        for (int j = i; j < s.size(); j++) {
            if (isPalindrome(s, i, j)) {
                longest = max(longest, j - i + 1);
            }
        }
    }
    return longest;
}

bool isPalindrome(const string& s, int left, int right) {
    while (left <= right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

可以很明显的看出，第二段代码的总体行数虽然更长，但是更加容易阅读，也更加容易 Debug。
