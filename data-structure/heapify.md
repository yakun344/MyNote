---
description: LintCode 130
---

# Heapify

{% embed url="https://www.lintcode.com/problem/heapify/description" %}

#### Description

Given an integer array, heapify it into a min-heap array.For a heap array A, A\[0] is the root of heap, and for each A\[i], A\[i \* 2 + 1] is the left child of A\[i] and A\[i \* 2 + 2] is the right child of A\[i].

Have you met this question in a real interview?  YesProblem Correction

#### Clarification

_**What is heap?**_ _**What is heapify?**_ _**What if there is a lot of solutions?**_

* Heap is a data structure, which usually have three methods: push, pop and top. where "push" add a new element the heap, "pop" delete the minimum/maximum element in the heap, "top" return the minimum/maximum element.
* Convert an unordered integer array into a heap array. If it is min-heap, for each element A\[i], we will get A\[i \* 2 + 1] >= A\[i] and A\[i \* 2 + 2] >= A\[i].
* Return any of them.

#### Example

_**Example 1**_

```
Input : [3,2,1,4,5]
Output : [1,2,3,4,5]
Explanation : return any one of the legitimate heap arrays
```

#### Challenge

O(n) time complexity

```java
public class Solution {
    /**
     * @param A: Given an integer array
     * @return: void
     */
    private void siftdown(int[] A, int k) {
        while (k * 2 + 1 < A.length) {        //确保当前节点有孩子
            int son = k * 2 + 1;        //左孩子下标
            
            //如果有右孩子，并且左孩子比右孩子大，son指向小的孩子（选择小孩子）
            if (k * 2 + 2 < A.length && A[son] > A[k * 2 + 2])
                son = k * 2 + 2;
                
            //小的孩子和父节点比大小，父节点小，满足最小堆，退出循环
            if (A[son] >= A[k])
                break;
            
            //孩子比父节点小，互换位置，然后父节点走向孩子
            //再次向下走，看以当前节点为父节点的子树是否满足minheap
            int temp = A[son];
            A[son] = A[k];
            A[k] = temp;
            k = son;
        }
    }
    
    public void heapify(int[] A) {
        for (int i = (A.length - 1) / 2; i >= 0; i--) {
            siftdown(A, i);
        }
    }
}
```
