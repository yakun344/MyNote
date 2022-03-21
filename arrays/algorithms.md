# Sort Algorithms

## Quick Select Algorithm

This [textbook algorthm](https://en.wikipedia.org/wiki/Quickselect) has O(N) average time complexity. Like quicksort, it was developed by Tony Hoare, and is also known as _Hoare's selection algorithm_.

The approach is basically the same as for quicksort. For simplicity let's notice that `k`th largest element is the same as `N - k`th smallest element, hence one could implement `k`th smallest algorithm for this problem.

First one chooses a pivot, and defines its position in a sorted array in a linear time. This could be done with the help of _partition algorithm_.

> To implement partition one moves along an array, compares each element with a pivot, and moves all elements smaller than pivot to the left of the pivot.

As an output we have an array where pivot is on its perfect position in the ascending sorted array, all elements on the left of the pivot are smaller than pivot, and all elements on the right of the pivot are larger or equal to pivot.

Hence the array is now split into two parts. If that would be a quicksort algorithm, one would proceed recursively to use quicksort for the both parts that would result in O(NlogN) time complexity. Here there is no need to deal with both parts since now one knows in which part to search for `N - k`th smallest element, and that reduces average time complexity to O(N).

Finally the overall algorithm is quite straightforward :

* Choose a random pivot.
* Use a partition algorithm to place the pivot into its perfect position `pos` in the sorted array, move smaller elements to the left of pivot, and larger or equal ones - to the right.
* Compare `pos` and `N - k` to choose the side of array to proceed recursively.

> ! Please notice that this algorithm works well even for arrays with duplicates.

## Quick Select

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

```java
class Solution {
    //平均时间复杂度是O(n);
    //最坏O(n^2);
    public int findKthLargest(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        return quickSelect(nums, k, 0, nums.length - 1);
    }
    private int quickSelect(int[] nums, int k, int start, int end) {
        if (start == end) {
            return nums[start];
        }
        
        int left = start, right = end;
        int pivot = nums[(start + end) / 2];
        
        while (left <= right) {
            while (left <= right && nums[left] > pivot) {
                left++;
            }
            while (left <= right && nums[right] < pivot) {
                right--;
            }
            
            if (left <= right) {
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++;
                right--;
            }
        }
        
        //start|________n1________|right|_|left_______n2________|end
        //if on the left
        if (start + k - 1 <= right) {
            return quickSelect(nums, k, start, right);
        //if on the right
        } else if (start + k - 1 >= left) {
            return quickSelect(nums, k - (left - start), left, end);
        //if on the middle
        } else {
            return nums[right + 1];
        }
    }
}
```

## Quick Sort

```java
public class Solution {
    
    //整体有序 -> 局部有序的分治排序算法。
    //平均时间复杂度是O(nlogn);
    public void sortIntegers2(int[] A) {
        quickSort(A, 0, A.length - 1);
    }
    
    private void quickSort(int[] A, int start, int end) {
        //start > end is illegal
        //we don't need to sort when start == end
        if (start >= end) {
            return;
        }
        
        int left = start, right = end;
        // key point 1: pivot is the value, not the index
        int pivot = A[(start + end) / 2];

        // key point 2: every time you compare left & right, it should be 
        // left <= right not left < right，保证有很多相同数字的情况下可以均分在pivot两边
        //避免相同元素达到最高的时间复杂度
        while (left <= right) {
            //A[left] < pivot rather than <=
            while (left <= right && A[left] < pivot) {
                left++;
            }
            while (left <= right && A[right] > pivot) {
                right--;
            }
            //第一次left >= right的时候就要退出整个while循环了
            //这个if判断是为了保证left 刚好和 right交换位置后不再往前走
            if (left <= right) {
                int temp = A[left];
                A[left] = A[right];
                A[right] = temp;
                
                left++;
                right--;
            }
        }
        
        quickSort(A, start, right);
        quickSort(A, left, end);
    }
}
```

## Merge Sort

```java
public class Solution {
    /**
     * @param A: an integer array
     * @return: nothing
     */
    public void sortIntegers(int[] A) {
        // write your code here
        if (A == null || A.length == 0) {
            return;
        }
        
        int[] temp = new int[A.length];
        mergeSort(A, 0, A.length - 1, temp);
        
        return;
    }
    
    //局部有序 -> 整体有序
    private void mergeSort(int[] A, int start, int end, int [] temp) {
        if (start >= end) {
            return;
        }
        
        mergeSort(A, start, (start + end) / 2, temp);
        mergeSort(A, (start + end) / 2 + 1, end, temp);
        merge(A, start, end, temp);
    }
    
    private void merge(int[] A, int start, int end, int[] temp) {
        int middle = (start + end) / 2;
        int leftIndex = start;
        int rightIndex = (start + end) / 2 + 1;
        int index = start;
        
        //merge two sorted subarrays in A to temp array
        while (leftIndex <= middle && rightIndex <= end) {
            if (A[leftIndex] < A[rightIndex]) {
                temp[index++] = A[leftIndex++];
            } else {
                temp[index++] = A[rightIndex++];
            }
        }
        
        //这里有可能有一个数组（或者左或者右）没有结束
        while (leftIndex <= middle) {
            temp[index++] = A[leftIndex++];
        }
        while (rightIndex <= end) {
            temp[index++] = A[rightIndex++];
        }
        
        //copy temp back to A
        for (int i = start; i <= end; ++i) {
            A[i] = temp[i];
        }
    }
}
```

排序的稳定性来讲，mergesort更稳定 2',1',1'',2'' -> 1',1'',2',2''.

## Heap Sort

![](<../.gitbook/assets/image (42).png>)

```java
//变量名：k是父节点，lc是left child，rc是right child
class Solution {
    public int[] sortArray(int[] nums) {
        for (int i = (nums.length - 1) / 2; i >= 0; --i) {
            heapify(nums, i, nums.length);
        }
        
        for (int i = nums.length - 1; i > 0; i--) {
            int temp = nums[i];
            nums[i] = nums[0];
            nums[0] = temp;
            heapify(nums, 0, i);
        }
        return nums;
    }
    
    //以k为root，大小为length的不完全平衡二叉树调整为堆
    private void heapify(int[] nums, int k, int length) {
        int rootNum = nums[k];
        int lc = k * 2 + 1;
        
        //lc存在
        while (lc < length) {
            int rc = k * 2 + 2;
            
            //如果k有rc
            //并且rc比lc大，那么lc更新为更大的子节点index
            if (rc < length && nums[lc] < nums[rc]) {
                lc++;
            }
            
            //根节点值 >= lc（lc已经指向左右孩子较大的那个）
            if (rootNum >= nums[lc]) break;
            
            //k往lc移动（这个时候lc指向的是较大的孩子），更新k和lc
            nums[k] = nums[lc];
            k = lc;
            lc = lc * 2 + 1;
        }

        nums[k] = rootNum;
    }
}
```

See [heapify](../data-structure/heapify.md)

```java
class Solution {
    public void heapSort(int[] nums) {
        // build max heap: for each root node bottom to top and right to left
        for (int i = nums.length / 2; i >= 0; i--) {
            maxHeapify(nums, i, nums.length - 1);
        }
        
        // swap first and adjust the new root to right position
        for (int i = nums.length - 1; i > 0; i--) {
            int tmp = nums[0];
            nums[0] = nums[i];
            nums[i] = tmp;
            // after each iteration, largest goes to ith, next end at i - 1
            maxHeapify(nums, 0, i - 1);
        }
    }
    
    private void maxHeapify(int[] nums, int start, int end) {
        int parent = start;
        
        // top to bottom
        while (parent <= end) {
            int left = parent * 2 + 1;
            int right = parent * 2 + 2;
            int child = -1;
            
            if (left <= end && right <= end) {
                // large value as swap candidate
                child = (nums[left] >= nums[right] ? left : right);
            } else if (left <= end) {
                child = left;
            } else {
                return;
            }
            
            // max heap root >= left && root >= right
            if (nums[parent] >= nums[child]) {
                return;
            }
            
            // swap
            int tmp = nums[parent];
            nums[parent] = nums[child];
            nums[child] = tmp;
            
            // update parent index
            parent = child;
        }
    }
}
```

![](<../.gitbook/assets/image (38).png>)
