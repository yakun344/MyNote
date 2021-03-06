# 递归、回溯和搜索

**什么是递归 (Recursion) ？**

很多书上会把递归（Recursion）当作一种算法。事实上，递归是包含两个层面的意思的：

1. 一种由大化小，由小化无的解决问题的算法。类似的算法还有动态规划（Dynamic Programming）。
2. 一种程序的实现方式。这种方式就是一个函数（Function / Method / Procedure）自己调用自己。

与之对应的，有非递归（Non-Recursion）和迭代法（Iteration），你可以认为这两个概念是一样的概念（番茄和西红柿的区别）。不需要做区分。

**什么是搜索 (Search)？**

搜索分为深度优先搜索（Depth First Search）和宽度优先搜索（Breadth First Search），通常分别简写为 DFS 和 BFS。搜索是一种类似于枚举（Enumerate）的算法。比如我们需要找到一个数组里的最大值，我们可以采用枚举法，因为我们知道数组的范围和大小，比如经典的打擂台算法：\
Java:

```java
int max = nums[0];
for (int i = 1; i < nums.length; i++) {
    max = Math.max(max, nums[i]);
}
```

Python:

```python
max_num = nums[0]
for i in range(1, len(nums)):
    max_num = max(max_num, nums[i])
```

枚举法通常是你知道循环的范围，然后可以用几重循环就搞定的算法。比如我需要找到 所有 x^2 + y^2 = K 的整数组合，可以用两重循环的枚举法：\
Java:

```java
// 不要在意这个算法的时间复杂度
for (int x = 1; x <= k; x++) {
    for (int y = 1; y <= k; y++) {
        if (x * x + y * y == k) {
            // print x and y
        }
    }
}
```

Python:

```python
for x in range(1, k+1):
    for y in range(1, k+1):
        if x*x + y*y == k:
            # print x and y
```

而有的问题，比如求 N 个数的全排列，你可能需要用 N 重循环才能解决。这个时候，我们就倾向于采用递归的方式去实现这个变化的 N 重循环。这个时候，我们就把算法称之为`搜索`。因为你已经不能明确的写出一个不依赖于输入数据的多重循环了。

通常来说 DFS 我们会采用递归的方式实现（当然你强行写一个非递归的版本也是可以的），而 BFS 则无需递归（使用队列 Queue + 哈希表 HashMap就可以）。`所以我们在面试中，如果一个问题既可以使用 DFS，又可以使用 BFS 的情况下，一定要优先使用 BFS。`因为他是非递归的，而且更容易实现。

**什么是回溯(Backtracking)？**

有的时候，深度优先搜索算法（DFS），又被称之为回溯法，所以你可以完全认为回溯法，就是深度优先搜索算法。在我的理解中，回溯实际上是深度优先搜索过程中的一个步骤。比如我们在进行全子集问题的搜索时，假如当前的集合是 {1,2} 代表我正在寻找以 {1,2}开头的所有集合。那么他的下一步，会去寻找 {1,2,3}开头的所有集合，然后当我们找完所有以 {1,2,3} 开头的集合时，我们需要把 3 从集合中删掉，回到 {1,2}。然后再把 4 放进去，寻找以 {1,2,4} 开头的所有集合。这个把 3 删掉回到 {1,2} 的过程，就是回溯。\
Java:

```java
subset.add(nums[i]);
subsetsHelper(result, subset, nums, i + 1);
subset.remove(list.size() - 1) // 这一步就是回溯
```

Python:

```python
subset.add(nums[i])
subsetsHelper(result, subset, nums, i + 1)
subset.remove(len(list) - 1)
```

详情请参考：\
[http://www.jiuzhang.com/solutions/subsets/](http://www.jiuzhang.com/solutions/subsets/)
