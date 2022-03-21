---
description: LeetCode 969 LintCode 894
---

# Pancake Sorting

{% embed url="https://leetcode.com/problems/pancake-sorting/" %}



Given an array of integers `arr`, sort the array by performing a series of **pancake flips**.

In one pancake flip we do the following steps:

* Choose an integer `k` where `1 <= k <= arr.length`.
* Reverse the sub-array `arr[0...k-1]` (**0-indexed**).

For example, if `arr = [3,2,1,4]` and we performed a pancake flip choosing `k = 3`, we reverse the sub-array `[3,2,1]`, so `arr = [1,2,3,4]` after the pancake flip at `k = 3`.

Return _an array of the_ `k`_-values corresponding to a sequence of pancake flips that sort_ `arr`. Any valid answer that sorts the array within `10 * arr.length` flips will be judged as correct.

**Example 1:**

```
Input: arr = [3,2,4,1]
Output: [4,2,4,3]
Explanation: 
We perform 4 pancake flips, with k values 4, 2, 4, and 3.
Starting state: arr = [3, 2, 4, 1]
After 1st flip (k = 4): arr = [1, 4, 2, 3]
After 2nd flip (k = 2): arr = [4, 1, 2, 3]
After 3rd flip (k = 4): arr = [3, 2, 1, 4]
After 4th flip (k = 3): arr = [1, 2, 3, 4], which is sorted.
```

**Example 2:**

```
Input: arr = [1,2,3]
Output: []
Explanation: The input is already sorted, so there is no need to flip anything.
Note that other answers, such as [3, 3], would also be accepted.
```

**Constraints:**

* `1 <= arr.length <= 100`
* `1 <= arr[i] <= arr.length`
* All integers in `arr` are unique (i.e. `arr` is a permutation of the integers from `1` to `arr.length`).

{% hint style="info" %}
烙饼排序

**算法：思维**

* 这题就是寻找当前未确定位置的数的范围内的的最大值，进行两次翻转。
  * 先翻转将最大值放到首位，然后再翻转将最大值放到尾部即已被排序部分（假设第一次反转排序，我们找到最大值，下标为index1，我们通过第一次翻转，将index1的数翻转到0，再通过第二次翻转，从0翻转到最后一位），寻找范围变小，
* 下一次继续寻找被确定范围内的最大值....一直重复直到序列有序
* 从左向右遍历，当前未被确定的范围为 \[0,len]\[0,len], 确定是最大的且排好序的范围为\[len+1,n]\[len+1,n]
* 查询当前范围\[0,len]\[0,len]的最大值，其位置为 pospos
* 通过第一次反转\[0,pos]\[0,pos], 将范围内最大值放到首部
* 通过第二次反转\[0,len]\[0,len]，将范维内最大值放到尾部
* 未被确定的范围更新为\[0,len−1]\[0,len−1]，重复上面的步骤，保证翻转操作次数最少

**复杂度分析**

* 时间复杂度`O(n*n)`
  * 最多交换n\*n次
* 空间复杂度`O(1)`
  * 不用额外空间
{% endhint %}

```java
class Solution {
    
    //res里存的是flip的index
    //结果是使得arr数组最后是排好序的就可以
    public List<Integer> pancakeSort(int[] arr) {
        int len = arr.length;
        if (arr == null || arr.length == 0) {
            return new ArrayList<>();
        }
        
        List<Integer> res = new ArrayList<>();
        while (len > 0) {
            int maxIndex = 0;
            for (int i = 0; i < len; ++i) {
                if (arr[maxIndex] < arr[i]) {
                    maxIndex = i;
                }
            }
            
            if (maxIndex == len - 1) {
                len--;
                continue;
            }
            
            if (maxIndex != 0) {
                flip(arr, maxIndex + 1);
                res.add(maxIndex + 1);
            }
            flip(arr, len);
            res.add(len);
            len--;
            
        }
        
        return res;
    }
    
    private void flip(int[] arr, int k) {
        for (int i = 0; i < k; ++i) {
            int temp = arr[i];
            arr[i] = arr[k - 1];
            arr[k - 1] = temp;
            k--;
        }
        change++;
    }
}
```

{% embed url="https://www.lintcode.com/problem/894/" %}



Description

Given an `unsorted array`, sort the given array. You are allowed to do only following operation on array.

```
flip(arr, i): Reverse array from 0 to i 
```

Unlike a traditional sorting algorithm, which attempts to sort with the fewest comparisons possible, the goal is to sort the sequence in as few reversals as possible.

You only call `flip` function.\
Don't allow to use any sort function or other sort methods.

Java：you can call `FlipTool.flip(arr, i)`\
C++： you can call `FlipTool::flip(arr, i)`\
Python2&3 you can call `FlipTool.flip(arr, i)`Example

**Example 1:**

```
Input: array = [6,11,10,12,7,23,20]
Output：[6,7,10,11,12,20,23]
Explanation：
flip(array, 5)
flip(array, 6)
flip(array, 0)
flip(array, 5)
flip(array, 1)
flip(array, 4)
flip(array, 1)
flip(array, 3)
flip(array, 1)
flip(array, 2)
```

**Example 2:**

```
Input: array = [4, 2, 3]
Output: [2, 3, 4]
Explanation:
	flip(array, 2)
	flip(array, 1)	
```

```java
public class Solution {
    /**
     * @param array: an integer array
     * @return: nothing
     */
    public void pancakeSort(int[] array) {
        // Write your code here
        int len = array.length;
        if (array == null || array.length == 0) {
            return;
        }

        while (len > 0) {
            int maxIndex = 0;
            for (int i = 0; i < len; ++i) {
                if (array[i] > array[maxIndex]) {
                    maxIndex = i;
                }
            }
            if (maxIndex == len - 1) {
                len--;
                continue;
            }

            if (maxIndex != 0) {
                FlipTool.flip(array, maxIndex);
            }
            
            len--;
            FlipTool.flip(array, len);
        }
    }
}
```
