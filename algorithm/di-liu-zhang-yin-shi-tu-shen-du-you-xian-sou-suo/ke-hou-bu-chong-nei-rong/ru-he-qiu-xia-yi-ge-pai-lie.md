# 如何求下一个排列

#### 问题描述

给定一个若干整数的排列，给出按整数大小进行字典序从小到大排序后的下一个排列。若没有下一个排列，则输出字典序最小的序列。\
例如`1,2,3` → `1,3,2`，`3,2,1` → `1,2,3`，`1,1,5 → 1,5,1`\
原题链接：\
[http://www.lintcode.com/problem/next-permutation-ii/](http://www.lintcode.com/zh-cn/problem/next-permutation-ii/#)\
[http://www.lintcode.com/problem/next-permutation/](http://www.lintcode.com/zh-cn/problem/next-permutation/#)\
（两题类似，一个要求原地修改，一个要求返回新的排列）

#### 算法描述

如果上来想不出方法，可以试着找找规律，我们关注的重点应是原数组末尾。

从末尾往左走，如果一直递增，例如`...9,7,5`，那么下一个排列一定会牵扯到左边更多的数，直到一个非递增数为止，例如`...6,9,7,5`。对于原数组的变化就只到`6`这里，和左侧其他数再无关系。`6`这个位置会变成`6`**右侧所有数中比`6`大的最小的数**，而`6`会进入最后3个数中，且后3个数必是**升序数组**。

所以算法步骤如下：

* 从右往左遍历数组`nums`，直到找到一个位置`i`，满足`nums[i] > nums[i - 1]`或者`i`为`0`。
* `i`不为0时，用`j`再次从右到左遍历`nums`，寻找第一个`nums[j] > nums[i - 1]`。而后交换`nums[j]`和`nums[i - 1]`。**注意，满足要求的`j`一定存在！且交换后`nums[i]`及右侧数组仍为降序数组**。
* 将`nums[i]`及右侧的数组翻转，使其升序。

**Q：i为0怎么办？**\
A：i为0说明整个数组是降序的，直接翻转整个数组即可。

**Q：有重复元素怎么办？**\
A：在遍历时只要严格满足`nums[i] > nums[i - 1]`和`nums[j] > nums[i - 1]`就不会有问题。

**Q：元素过少是否要单独考虑？**\
A：当元素个数小于等于1个时，可以直接返回。

#### 参考代码

Java:

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers that's next permuation
    */
    // 用于交换nums[i]和nums[j]
    public void swapItem(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    // 用于翻转nums[i]到nums[j]，包含两端的这一小段数组
    public void swapList(int[] nums, int i, int j) {
        while (i < j) {
            swapItem(nums, i, j);
            i ++; 
            j --;
        }
    }
    public void nextPermutation(int[] nums) {
        int len = nums.length;
        if ( len <= 1) {
            return;
        }
        int i = len - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) {
            i --;
        }
        if (i != 0) {
            int j = len - 1;
            while (nums[j] <= nums[i - 1]) {
                j--;
            }
            swapItem(nums, j, i-1);
        }
        swapList(nums, i, len - 1);
    }
}
```

Python:

```python
class Solution:
    # 用于翻转nums[i]到nums[j]，包含两端的这一小段数组
    def swapList(self, nums, i, j):
        while i < j:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
            j -= 1
            
    """
    @param nums: An array of integers
    @return: nothing
    """
    def nextPermutation(self, nums):
        n = len(nums)
        if n <= 1:
            return
        
        i = n-1
        while i > 0 and nums[i] <= nums[i-1]:
            i -= 1

        if i != 0:
            j = n-1
            while nums[j] <= nums[i-1]:
                j -= 1
            nums[j], nums[i-1] = nums[i-1], nums[j]
        self.swapList(nums, i, n-1)
```
