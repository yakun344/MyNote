---
description: LeetCode 170
---

# Two Sum III - Data structure design

{% embed url="https://leetcode.com/problems/two-sum-iii-data-structure-design/" %}

Design and implement a TwoSum class. It should support the following operations: `add` and `find`.

`add` - Add the number to an internal data structure.\
`find` - Find if there exists any pair of numbers which sum is equal to the value.

**Example 1:**

```
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

**Example 2:**

```
add(3); add(1); add(2);
find(3) -> true
find(6) -> false
```

{% hint style="info" %}
add操作O(1), find操作O(n);
{% endhint %}

```java
class TwoSum {

    /** Initialize your data structure here. */
    public TwoSum() {
        
    }
    
    private Map<Integer, Integer> map = new HashMap<>();
    private List<Integer> list = new ArrayList<>();
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        if (map.containsKey(number)) {
            map.put(number, map.get(number) + 1);
        } else {
            map.put(number, 1);
        }
        list.add(number);
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        for (int i = 0; i < list.size(); ++i) {
        
            //如果map里找到了第二个数，并且第一个数字和第二个数字不同，返回true
            if (map.containsKey(value - list.get(i)) && value - list.get(i) != list.get(i)) {
                return true;
            //如果map里第一个数和第二个数字相同，并且这个数字的数量大于等于2，返回true；
            } else if (value - list.get(i) == list.get(i) && map.get(list.get(i)) >= 2) {
                return true;
            }
        }
        return false;
    }
}

/**
 * Your TwoSum object will be instantiated and called as such:
 * TwoSum obj = new TwoSum();
 * obj.add(number);
 * boolean param_2 = obj.find(value);
 */
```

```java
public class TwoSum {
    /**
     * @param number: An integer
     * @return: nothing
     */
    private Map<Integer, Integer> map = new HashMap<>();
    public void add(int number) {
        // write your code here
        map.put(number, map.getOrDefault(number, 0) + 1);
    }

    /**
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    public boolean find(int value) {
        // write your code her
        for (int n : map.keySet()) {
            int num2 = value - n;
            if (n == num2 && map.get(n) > 1) {
                return true;
            } else if (n != num2 && map.containsKey(num2)) {
                return true;
            }
        }
        return false;
    }
}
```

{% hint style="info" %}
add操作O(n),find操作O(1);
{% endhint %}

```java
public class TwoSum {
    /**
     * @param number: An integer
     * @return: nothing
     */
    private Set<Integer> sum = new HashSet<>();
    private List<Integer> list = new ArrayList<>();
    public void add(int number) {
        // write your code here
        for (int n : list) {
            sum.add(n + number);
        }
        list.add(number);
    }

    /**
     * @param value: An integer
     * @return: Find if there exists any pair of numbers which sum is equal to the value.
     */
    public boolean find(int value) {
        // write your code here
        return sum.contains(value);
    }
}
```
