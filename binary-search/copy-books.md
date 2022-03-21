---
description: LintCode 437
---

# Copy Books

{% embed url="https://www.lintcode.com/problem/437/" %}

#### Description

Given `n` books and the `i-th` book has `pages[i]` pages. There are `k` persons to copy these books.

These books list in a row and each person can claim a continuous range of books. For example, one copier can copy the books from `i-th` to `j-th` continuously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book).

They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?

Return the shortest time that the slowest copier spends.

The sum of book pages is less than or equal to 2147483647

#### Example

**Example 1:**

```
Input: pages = [3, 2, 4], k = 2
Output: 5
Explanation: 
    First person spends 5 minutes to copy book 1 and book 2.
    Second person spends 4 minutes to copy book 3.
```

**Example 2:**

```
Input: pages = [3, 2, 4], k = 3
Output: 4
Explanation: Each person copies one of the books.
```

#### Challenge

O(nk) time

{% hint style="info" %}
二分答案，最小时间的取值范围是0 - pages(max)，用可以完成的时间来计算需要在这个时间内完成需要的人数，如果这个人数小于等于k，则说明当前时间k个人可以完成，缩小搜索范围，看最能不能找到更小的时间来完成这件事；如果这个人数大于k，这就说明在当前的时间内需要比k还多的人数去完成，这时候增大时间，看更多的时间能不能让k个人完成。
{% endhint %}

1. 确定二分的对象是时间。
2. 判断在这个时间内能不能让k个人完成。写法有两种，直接判断mid能不能完成，可以就end = mid，不可以就start = mid；第二种写法，helper(mid, param) <= k，start = mid, else end = mid。推荐第一种。
3. 可以优化的地方：缩小答案的取值范围，从arr.max \~ arr.sum。

⚠️写这个helper函数的时候bug老多了😭

```java
public class Solution {
    /**
     * @param pages: an array of integers
     * @param k: An integer
     * @return: an integer
     */
    public int copyBooks(int[] pages, int k) {
        // write your code here
        int start = 0, end = getSum(pages);
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (getCopiers(pages, mid) <= k) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (getCopiers(pages, start) <= k) {
            return start;
        } else {
            return end;
        }
    }
    private int getSum(int[] pages) {
        int sum = 0;
        for (int i = 0; i < pages.length; ++i) {
            sum += pages[i];
        }
        return sum;
    }
    private int getCopiers(int[] pages, int cnt) {
        int sum = 0;
        int copiers = 1;
        for (int i = 0; i < pages.length; ++i) {
            if (pages[i] > cnt) {
                return Integer.MAX_VALUE;
            }
            if (sum + pages[i] > cnt) {
                sum = 0;
                copiers++;
            }
            sum += pages[i];
        }
        return copiers;
    }
}
```

#### 优化搜索范围后的写法

```java
public class Solution {
    /**
     * @param pages: an array of integers
     * @param k: An integer
     * @return: an integer
     */
    public int copyBooks(int[] pages, int k) {
        // write your code here
        int start = 0, end = 0;
        for (int i = 0; i < pages.length; ++i) {
            end += pages[i];
            start = Math.max(start, pages[i]);
        }

        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (canFinish(pages, k, mid)) {
                end = mid;
            } else {
                start = mid;
            }
        }

        if (canFinish(pages, k, start)) {
            return start;
        } else {
            return end;
        }
    }
    private boolean canFinish(int[] pages, int k, int cnt) {
        int sum = 0;
        int persons = 1;
        for (int page : pages) {
            if (sum + page > cnt) {
                persons++;
                sum = 0;
            }
            sum += page;
        }
        if (persons > k) {
            return false;
        } else {
            return true;
        }
    }
}
```
