# Greedy Algorithms

Greedy is an algorithmic paradigm that builds up a solution piece by piece, always choosing the next piece that offers the most obvious and immediate benefit. So the problems where choosing locally optimal also leads to global solution are best fit for Greedy.&#x20;

For example consider the Fractional Knapsack Problem. The local optimal strategy is to choose the item that has maximum value vs weight ratio. This strategy also leads to global optimal solution because we allowed to take fractions of an item.

![](https://www.geeksforgeeks.org/wp-content/uploads/Fractional-Knapsackexample-min-768x384.png)

**Input:** \
Items as (value, weight) pairs \
arr\[] = \{{60, 10}, {100, 20}, {120, 30\}} \
Knapsack Capacity, W = 50;&#x20;

**Output:** \
Maximum possible value = 240 \
by taking items of weight 10 and 20 kg and 2/3 fraction \
of 30 kg. Hence total price will be 60+100+(2/3)(120) = 240
