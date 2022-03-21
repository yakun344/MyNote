---
description: LeetCode 1344
---

# Angle Between Hands of a Clock

{% embed url="https://leetcode.com/problems/angle-between-hands-of-a-clock" %}



Given two numbers, `hour` and `minutes`. Return the smaller angle (in degrees) formed between the `hour` and the `minute` hand.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/26/sample\_1\_1673.png)

```
Input: hour = 12, minutes = 30
Output: 165
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/26/sample\_2\_1673.png)

```
Input: hour = 3, minutes = 30
Output: 75
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/12/26/sample\_3\_1673.png)

```
Input: hour = 3, minutes = 15
Output: 7.5
```

**Example 4:**

```
Input: hour = 4, minutes = 50
Output: 155
```

**Example 5:**

```
Input: hour = 12, minutes = 0
Output: 0
```

&#x20;

**Constraints:**

* `1 <= hour <= 12`
* `0 <= minutes <= 59`
* Answers within `10^-5` of the actual value will be accepted as correct.

```java
class Solution {
    public double angleClock(int hour, int minutes) {
        int perMin = 6;
        int perH = 30;
        
        double minuteAngle = perMin * minutes;
        double hourAngle = (hour % 12 + minutes / 60.0) * perH;
        double angle = Math.abs(hourAngle - minuteA);
        return Math.min(angle, 360 - angle);
    }
}
```
