---
description: LintCode 198
---

# Permutation Index II

{% embed url="https://www.lintcode.com/problem/permutation-index-ii/" %}



Description

Given a permutation which may contain repeated numbers, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.Example

**Example 1:**

```
Input :[1,4,2,2]
Output:3
```

**Example 2:**

```
Input :[1,6,5,3,1]
Output:24
```



* 我们先考虑没有重复元素的排列：
* 要知道排列元素的所有排列按字典序排序后该排列的编号，可以通过求出小于这个排列的排列组合的数量num，那么答案就是num+1。
* 我们从第1位开始向后依次考虑，以`[4,3,2,1]`为例：
* 第1位：可选取的元素(3,2,1)小于第1位的元素有3个。假如我们固定第1位小于4的情况，那么第一位有3种，后面3位的排列组合数有3!种，所以比较到第1位就小于该排列的排列组合数共有3\*3!=18种。
* 第2位：固定第1位为4，那么可选取的元素(2,1)小于第2位的元素有2个。后面2位的排列组合数有2!种，所以比较到第2位才小于该排列的排列组合数共有2\*2!=4种。
* 第三位：固定第2位为3，那么可选取的元素(1)小于第3位的元素有1个。后面1位的排列组合数有1!种，所以比较到第3位才小于该排列的排列组合数共有1\*1!=1种。
* 第四位：没有选择余地。
* 所以答案为18+4+1+1=24

![图片](https://media-test.jiuzhang.com/media/markdown/images/6/3/73b93706-a581-11ea-bee7-0242c0a8b005.jpg)

### 复杂度分析

* 假设排列长度为n
* 取决于不重复元素的个数，最坏空间复杂度为O(n)。
* 哈希表查找重复元素的个数为O(1)，遍历每个位置O(n)，计数可选取的小于当前元素的元素数目为O(n^2)，所以时间复杂度为O(n^2)。

```java
public class Solution {
    /**
     * @param A: An array of integers
     * @return: A long integer
     */
    public long permutationIndexII(int[] A) {
        int n = A.length;
        long sum = 1;
        long factorial = 1;
        long repeat = 1;
        HashMap<Integer, Integer> num = new HashMap<>();
        for (int i = n - 1; i >= 0; i--) {
            if (num.containsKey(A[i])) {
    	 	   num.put(A[i], num.get(A[i]) + 1);
    	    }
    	    else {
    	 	   num.put(A[i], 1);
    	    }
            if(num.get(A[i]) > 1) {
                // 重复部分的阶乘
                repeat *= num.get(A[i]);
            }
            // 可选取的比当前元素小的元素数目
            int smaller = 0;
            for (int j = i + 1; j < n; j++) {
                if (A[j] < A[i]) {
                    smaller++;
                } 
            }
            sum += smaller * factorial / repeat;
            factorial *= (n - i);
        }
        return sum;
    }
}
```
