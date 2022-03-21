---
description: LintCode 1538
---

# Card Game II

{% embed url="https://www.lintcode.com/problem/card-game-ii/description" %}

You are playing a card game with your friends, there are `n` cards in total. Each card costs `cost[i]` and inflicts `damage[i]` damage to the opponent. You have a total of `totalMoney` dollars and need to inflict at least `totalDamage` damage to win. And Each card can only be used once. Determine if you can win the game.

#### Example

**Example1**

```
Input:
cost = [1,2,3,4,5]
damage = [1,2,3,4,5]
totalMoney = 10
totalDamage = 10

Output: true
Explanation: We can use the [1,4,5] to cause 10 damage, which costs 10.
```

**Example2**

```
Input:
cost = [1,2]
damage = [3,4]
totalMoney = 10
totalDamage = 10

Output: false
Explanation: We can only cause 7 damage at most.
```

{% hint style="danger" %}
背包问题，用TotalMoney的话费得到的最大的收益，判断收益能否达到TotalDamage即可
{% endhint %}

```java
public class Solution {
    /**
     * @param cost: costs of all cards
     * @param damage: damage of all cards
     * @param totalMoney: total of money
     * @param totalDamage: the damage you need to inflict
     * @return: Determine if you can win the game
     */
    public boolean cardGame(int[] cost, int[] damage, int totalMoney, int totalDamage) {
        // Write your code here
        int []f = new int[totalMoney + 1];
        for (int i = 0; i <= totalMoney; i++) {
            f[i] = 0;
        }
        int n = cost.length;
        int i,j;
        for (i = 0; i < n; i ++) {
            for (j = totalMoney; j >= cost[i]; j --) {
                if (f[j] < f[j - cost[i]] + damage[i]) {
                    f[j] = f[j - cost[i]] + damage[i];
                }
            }
        }
        return f[totalMoney] >= totalDamage ? true : false;
    }
}
```
