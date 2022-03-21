---
description: LintCode 197
---

# Permutation Index

{% embed url="https://www.lintcode.com/problem/permutation-index/" %}



Description

Given a permutation which contains no repeated number, find its index in all the permutations of these numbers, which are ordered in lexicographical order. The index begins at 1.Example

**Example 1:**

```
Input:[1,2,4]
Output:1
```

**Example 2:**

```
Input:[3,2,1]
Output:6
```

>
>
> * 要知道排列元素的所有排列按字典序排序后该排列的编号，可以通过求出小于这个排列的排列组合的数量num，那么答案就是num+1。
> * 我们从第1位开始向后依次考虑，以\[4,3,2,1]为例：
> * 第1位：可选取的元素(3,2,1)小于第1位的元素有3个。假如我们固定第1位小于4的情况，那么第一位有3种，后面3位的排列组合数有3!种，所以比较到第1位就小于该排列的排列组合数共有3\*3!=18种。
> * 第2位：固定第1位为4，那么可选取的元素(2,1)小于第2位的元素有2个。后面2位的排列组合数有2!种，所以比较到第2位才小于该排列的排列组合数共有2\*2!=4种。
> * 第三位：固定第2位为3，那么可选取的元素(1)小于第3位的元素有1个。后面1位的排列组合数有1!种，所以比较到第3位才小于该排列的排列组合数共有1\*1!=1种。
> * 第四位：没有选择余地。
> * 所以答案为18+4+1+1=24
> * 综上所述，假设排列有n个元素，且用cnt\[i]表示小于第i位的元素个数，我们可以得到答案为:

![](<../.gitbook/assets/image (23).png>)

```java
public class Solution {
    /**
     * @param A: An array of integers
     * @return: A long integer
     */
    public long permutationIndex(int[] A) {
        // write your code here
        long factorial = 1;
        long sum = 1;

        for (int i = A.length - 1; i >= 0; --i) {
            int smaller = 0;
            for (int j = i + 1; j < A.length; ++j) {
                if (A[i] > A[j]) {
                    smaller++;
                }
            }

            sum += smaller * factorial;
            factorial *= (A.length - i);
        }

        return sum;
    }
}
//4,3,2,1
```
