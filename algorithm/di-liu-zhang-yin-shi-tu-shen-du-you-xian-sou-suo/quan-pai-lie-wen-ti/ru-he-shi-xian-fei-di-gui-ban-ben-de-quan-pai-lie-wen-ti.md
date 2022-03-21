# 如何实现非递归版本的全排列问题？

#### 基本思路

非递归的全排列，采用的是**迭代**方式，在[如何求下一个排列](http://www.jiuzhang.com/tutorial/algorithm/439)中，我们讲过如何求下一个排列，那么我们**只需要不断调用这个`nextPermutation`方法即可**。

**一些可以做得更细致的地方**：

* 为了确定何时结束，建议在迭代前，先对输入`nums`数组进行升序排序，迭代到降序时，就都找完了。有心的同学可能还记得在`nextPermutation`当中，当且仅当数组完全降序，那么从右往左遍历的指针`i`**最终会指向0**。所以可以为`nextPermutation`带上布尔返回值，当`i`为0时，返回`false`，表示找完了。要注意，排序操作在这样一个NP问题中，消耗的时间几乎可以忽略。
* 当数组长度为1时，`nextPermutation`会直接返回`false`；当数组长度为0时，`nextPermutation`中`i`会成为-1，所以返回`false`的条件可以再加上`i`为`-1`。
* Java中，如果输入类型是`int[]`，而输出类型是`List<List<Integer>>`，要注意，并没有太好的方法进行类型转换，这是由于`int`是基本类型。建议还是自行手动复制，实际工作中还可使用`guava`库。

#### 核心代码

Java:

```java
class Solution {
    public boolean nextPermutation(int[] nums) {
        int len = nums.length;
        int i = len - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) {
            i--;
        }
        if (i <= 0) {
            return false;
        }
        /* 
            所有剩余代码参见 http://www.jiuzhang.com/tutorial/algorithm/439
        */
        return true;
    }

    public List<List<Integer>> permute(int[] A) {
        Arrays.sort(A);
        List<List<Integer>> result = new ArrayList<>();
        
        boolean next = true;  // next 为 true 时，表示可以继续迭代
        while (next)  {
            List<Integer> current = new ArrayList<>();  // 进行数组复制
            for (int a : A) {
                current.add(a);
            }
            
            result.add(current);
            next = nextPermutation(A);
        }
        return result;
    }
}
```

Python:

```python
class Solution:
    def nextPermutation(self, nums):
        n = len(nums)
        if n <= 1:
            return
        
        i = n-1
        while i > 0 and nums[i] <= nums[i-1]:
            i -= 1

        if i <= 0:
            return False

        # 所有剩余代码参见http://www.jiuzhang.com/tutorial/algorithm/439

        return True

    def permute(self, A):
        A.sort()
        result = []

        hasNext = True  # hasNext 为 true 时，表示可以继续迭代
        while hasNext:
            current = list(A)  # 进行数组复制
            result.append(current)
            hasNext = self.nextPermutation(A)
        
        return result
```
