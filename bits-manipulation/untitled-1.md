# Bits Calculate

### Tip1消去二进制中最右侧的那个1

```
x & (x - 1) 用于消去x最后一位的1, 比如x = 12, 那么在二进制下就是(1100)2

x           = 1100
x - 1       = 1011
x & (x - 1) = 1000
```

**应用一**：用 O(_1_) 时间检测整数 _n_ 是否是 _2_ 的幂次。

[http://www.lintcode.com/zh-cn/problem/o1-check-power-of-2/](http://www.lintcode.com/zh-cn/problem/o1-check-power-of-2/)

N如果是2的幂次，则N满足两个条件。

* N > 0
* N的二进制表示中`只有一个1`, 注意只能有1个。

因为N的二进制表示中只有一个1，所以使用N & (N - 1)将N唯一的一个1消去，应该返回0。

综合上述方法，我们可以写出非常简洁漂亮的Java代码：

```java
class Solution {
public:
    /*
     * @param n: An integer
     * @return: True or false
     */
    bool checkPowerOf2(int n) {
        // write your code here
        return n > 0 && (n & (n - 1)) == 0;
    }
};
```

#### **应用二**：计算在一个 32 位的整数的二进制表式中有多少个 1.

[http://www.lintcode.com/zh-cn/problem/count-1-in-binary/](http://www.lintcode.com/zh-cn/problem/count-1-in-binary/)

由x & (x - 1)消去x最后一位的1可知。不断使用 x & (x - 1) 消去x最后一位的1，计算总共消去了多少次即可。

```java
public class Solution {
    /**
     * @param num: an integer
     * @return: an integer, the number of ones in num
     */
    public int countOnes(int num) {
        int count = 0;
        while (num != 0) {
            num = num & (num - 1);
            count++;
        }
        return count;
    }
}
```

#### **应用三**：如果要将整数A转换为B，需要改变多少个bit位？

[http://www.lintcode.com/zh-cn/problem/flip-bits/](http://www.lintcode.com/zh-cn/problem/flip-bits/)

这个应用是上面一个应用的拓展\
思考将整数A转换为B，如果A和B在第i（0 <=i < 32）个位上相等，则不需要改变这个BIT位，如果在第i位上不相等，则需要改变这个BIT位。

#### 所以问题转化为了A和B有多少个BIT位不相同！

联想到位运算有一个`异或操作`，相同为0，相异为1，所以问题转变成了计算A异或B之后这个数中1的个数!

```java
class Solution {
    /**
     *@param a, b: Two integer
     *return: An integer
     */
    public int countOnes(int num) {
        int count = 0;
        while (num != 0) {
            num = num & (num - 1);
            count++;
        }
        return count;
    }

    public int bitSwapRequired(int a, int b) {
        // write your code here
        return countOnes(a ^ b);
    }
}
```

### Tip2：使用二进制进行子集枚举

**应用：给定一个含不同整数的集合，返回其`所有的子集`**

[http://www.lintcode.com/zh-cn/problem/subsets/](http://www.lintcode.com/zh-cn/problem/subsets/)

思路就是使用一个`正整数`二进制表示的`第i位`是1还是0来代表集合的`第i个数取或者不取`。\
所以从`0到2^n-1`总共`2^n`个整数，正好对应集合的2^n个子集。如下是就是 `整数 <=> 二进制 <=> 对应集合` 之间的转换关系。

S = {1,2,3}

```
N bit Combination
0 000 {}
1 001 {1}
2 010 {2}
3 011 {1,2}
4 100 {3}
5 101 {1,3}
6 110 {2,3}
7 111 {1,2,3}
```

**Java code实现如下:**

```java
// Non Recursion
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public List<ArrayList<Integer>> subsets(int[] nums) {
        List<ArrayList<Integer>> result = new ArrayList<List<Integer>>();
        int n = nums.length;
        Arrays.sort(nums);
        
        // 1 << n is 2^n
        // each subset equals to an binary integer between 0 .. 2^n - 1
        // 0 -> 000 -> []
        // 1 -> 001 -> [1]
        // 2 -> 010 -> [2]
        // ..
        // 7 -> 111 -> [1,2,3]
        for (int i = 0; i < (1 << n); i++) {
            List<Integer> subs= new ArrayList<Integer>();
            for (int j = 0; j < n; j++) {
                // check whether the jth digit in i's binary representation is 1
                if ((i & (1 << j)) != 0) {
                    subset.add(nums[j]);
                }
            }
            result.add(subset);
        }
        
        return result;
    }
}
```

### Tip3: 巧用异或运算

```
a ^ b ^ b = a // 对一个数异或两次等价于没有任何操作！
```

**应用一：数组中，只有一个数出现一次，剩下都出现两次，找出出现一次的数**

[http://www.lintcode.com/en/problem/single-number/](http://www.lintcode.com/en/problem/single-number/)

因为只有一个数恰好出现一个，剩下的都出现过两次，所以只要将所有的数异或起来，就可以得到唯一的那个数，因为相同的数出现的两次，异或两次等价于没有任何操作！

```java
public class Solution {
    public int singleNumber(int[] nums) {
        int result = 0, n = nums.length;
        for (int i = 0; i < n; i++)
        {
            result ^= nums[i];
        }
        return result;
    }
}
```

**应用二：数组中，只有一个数出现一次，剩下都出现三次，找出出现一次的数**

[http://www.lintcode.com/en/problem/single-number-ii/](http://www.lintcode.com/en/problem/single-number-ii/)

因为其他数是出现三次的，也就是说，对于每一个二进制位，如果只出现一次的数在该二进制位为1，那么这个二进制位在全部数字中出现次数无法被3整除。\
对于每一位，我们让Two，One表示当前位的状态。

我们看Two和One里面的每一位的定义，对于ith(表示第i位)：

**如果Two里面ith是1，则ith当前为止出现1的次数模3的结果是2**

**如果One里面ith是1，则ith目前为止出现1的次数模3的结果是1**

注意Two和One里面不可能ith同时为1，因为这样就是3次，每出现3次我们就可以抹去（消去）。那么最后One里面存储的就是每一位模3是1的那些位，综合起来One也就是最后我们要的结果。

如果B表示输入数字的对应位，Two+和One+表示更新后的状态\
那么新来的一个数B，此时跟原来出现1次的位做一个异或运算，&上`~Two`的结果(也就是不是出现2次的)，那么剩余的就是当前状态是1的结果。\
同理Two ^ B （2次加1次是3次，也就是Two里面ith是1，B里面ith也是1，那么ith应该是出现了3次，此时就可以消去，设置为0），我们相当于会消去出现3次的位。

**但是Two ^ B也可能是ith上Two是0，B的ith是1，这样Two里面就混入了模3是1的那些位！！！怎么办？我们得消去这些！我们只需要保留不是出现模3余1的那些位ith，而One是恰好保留了那些模3余1次数的位，\`取反不就是不是模3余1的那些位ith么？最终对(\~One+)取一个&即可。**

综合起来就是：

```
One+ = (One ^ B) & (~Two)
Two+ = (~One+) & (Two ^ B)
```

以下就是非常漂亮的Java代码实现：

```java
public class Solution {
    public int singleNumber(int[] nums) {
        int ones = 0, twos = 0;
        for(int i = 0; i < nums.length; i++){
            ones = (ones ^ nums[i]) & ~twos;
            twos = (twos ^ nums[i]) & ~ones;
        }
        return ones;
    }
}
```

**应用三：数组中，只有两个数出现一次，剩下都出现两次，找出出现一次的这两个数**

[http://www.lintcode.com/en/problem/single-number-iii/](http://www.lintcode.com/en/problem/single-number-iii/)

有了第一题的基本的思路，我们可以将数组分成两个部分，每个部分里只有一个元素出现一次，其余元素都出现两次。那么使用这种方法就可以找出这两个元素了。不妨假设出现一个的两个元素是x，y，那么最终所有的元素异或的结果就是等价于`res = x^y`。\
`并且res！=0`

**为什么呢？ 如果res 等于0，则说明x和y相等了！！！！**

因为res不等于0，那么我们可以**一定**可以找出res二进制表示中的某一位是1。

**对于x和y，一定是其中一个这一位是1，另一个这一位不是1！！！细细琢磨， 因为如果都是0或者都是1，怎么可能异或出1**

对于原来的数组，我们可以根据这个位置是不是1就可以将数组分成两个部分。`x，y一定在不同的两个子集中`。

**而且对于其他成对出现的元素，要么都在x所在的那个集合，要么在y所在的那个集合。对于这两个集合我们分别求出单个出现的x 和 单个出现的y即可。**

```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        //用于记录，区分“两个”数组
        int diff = 0;
        for(int i = 0; i < nums.length; i ++) {
            diff ^= nums[i];
        }
        //取最后一位1
        //先介绍一下原码，反码和补码
        //原码，就是其二进制表示（注意，有一位符号位）
        //反码，正数的反码就是原码，负数的反码是符号位不变，其余位取反
        //补码，正数的补码就是原码，负数的补码是反码+1
        //在机器中都是采用补码形式存
        //diff & (-diff)就是取diff的最后一位1的位置
        diff &= -diff;
        
        int[] rets = {0, 0}; 
        for(int i = 0; i < nums.length; i ++) {
            //分属两个“不同”的数组
            if ((nums[i] & diff) == 0) {
                rets[0] ^= nums[i];
            }
            else {
                rets[1] ^= nums[i];
            }
        }
        return rets;
    }
}
```
