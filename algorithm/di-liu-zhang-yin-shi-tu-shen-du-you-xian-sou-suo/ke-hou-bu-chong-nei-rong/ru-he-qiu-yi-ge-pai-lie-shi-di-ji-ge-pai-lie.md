# 如何求一个排列是第几个排列？

#### 题目描述

给出一个不含重复数字的排列，求这些数字的所有排列按字典序排序后该排列的编号，编号从1开始。\
例如排列`[1,2,4]`是第`1`个排列。\
[http://www.lintcode.com/zh-cn/problem/permutation-index/](http://www.lintcode.com/zh-cn/problem/permutation-index/)

#### 算法描述

只需计算有多少个排列在当前排列`A`的前面即可。如何算呢?举个例子，`[3,7,4,9,1]`，在它前面的必然是**某位置`i`对应元素比原数组小，而`i`左侧和原数组一样**。也即`[3,7,4,1,X]`，`[3,7,1,X,X]`，`[3,1或4,X,X,X]`，`[1,X,X,X,X]`。\
而第`i`个元素，比原数组小的情况有多少种，其实就是`A[i]`右侧有多少元素比`A[i]`小，乘上`A[i]`右侧元素全排列数，即`A[i]`右侧元素数量的阶乘。`i`从右往左看，比当前`A[i]`小的右侧元素数量分别为`1,1,2,1`，所以最终字典序在当前`A`之前的数量为1×1!+1×2!+2×3!+1×4!=39，故当前`A`的字典序为40。

**具体步骤：**

* 用`permutation`表示当前阶乘，初始化为1,`result`表示最终结果，初始化为0。由于最终结果可能巨大，所以用`long`类型。
* `i`从右往左遍历`A`，循环中计算`A[i]`右侧有多少元素比`A[i]`小，计为`smaller`，`result += smaller * permutation`。之后`permutation *= A.length - i`，为下次循环`i`左移一位后的排列数。
* 已算出多少字典序在`A`之前，返回`result+1`。

#### 参考代码

Java:

```java
public class Solution {
    /**
     * @param A: An array of integers
     * @return: A long integer
     */
    public long permutationIndex(int[] A) {
        // write your code here
        long permutation = 1;
        long result = 0;
        for (int i = A.length - 2; i >= 0; --i) {
            int smaller = 0;
            for (int j = i + 1; j < A.length; ++j) {
                if (A[j] < A[i]) {
                    smaller++;
                }
            }
            result += smaller * permutation;
            permutation *= A.length - i;
        }
        return result + 1;
    }
}
```

Python:

```python
class Solution:
    """
    @param A: An array of integers
    @return: A long integer
    """
    def permutationIndex(self, A):
        permutation = 1
        result = 0
        for i in range(len(A) - 2, -1, -1):
            smaller = 0
            for j in range(i + 1, len(A)):
                if A[j] < A[i]:
                    smaller += 1
            result += smaller * permutation
            permutation *= len(A) - i
        return result + 1
```

**Q：为了找寻每个元素右侧有多少元素比自己小，用了O(n2)O(n2)的时间，能不能更快些？**\
A：可以做到O(nlogn)O(nlogn)！但是很复杂，这是另外一个问题了，可以使用BST，归并排序或者线段树，详见[http://www.lintcode.com/zh-cn/problem/count-of-smaller-number-before-itself/](http://www.lintcode.com/zh-cn/problem/count-of-smaller-number-before-itself/)

**Q：元素有重复怎么办？**\
A：好问题！元素有重复，情况会复杂的多。因为这会影响`A[i]`右侧元素的排列数，此时的排列数计算方法为**总元素数的阶乘，除以各元素值个数的阶乘**，例如`[1,1,1,2,2,3]`，排列数为6!÷(3!×2!×1!)6!÷(3!×2!×1!)。\
为了正确计算阶乘数，需要用哈系表记录`A[i]`及右侧的元素值个数，并考虑到`A[i]`与右侧比其小的元素`A[k]`交换后，要把`A[k]`的计数减一。用该哈系表计算正确的阶乘数。\
而且要注意，右侧比`A[i]`小的重复元素值只能计算一次，不要重复计算！
