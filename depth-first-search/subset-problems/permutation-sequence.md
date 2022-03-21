---
description: LeetCode 60
---

# Permutation Sequence

{% embed url="https://leetcode.com/problems/permutation-sequence/" %}



The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:

1. `"123"`
2. `"132"`
3. `"213"`
4. `"231"`
5. `"312"`
6. `"321"`

Given `n` and `k`, return the `kth` permutation sequence.

**Example 1:**

```
Input: n = 3, k = 3
Output: "213"
```

**Example 2:**

```
Input: n = 4, k = 9
Output: "2314"
```

**Example 3:**

```
Input: n = 3, k = 1
Output: "123"
```

**Constraints:**

* `1 <= n <= 9`
* `1 <= k <= n!`

#### DFS Approach(**200 / 200** test cases passed, but took too long.)

Time Complexity O(N!)

```java
class Solution {
    private int count;
    public String getPermutation(int n, int k) {
        List<String> res = new ArrayList<>();
        boolean[] visited = new boolean[n + 1];
        StringBuilder sb = new StringBuilder();
        dfs(n, res, 1, sb, visited);
        return res.get(k - 1);
    }
    
    private void dfs(int n,
                    List<String> res,
                    int startIndex,
                    StringBuilder sb,
                    boolean[] visited) {
        if (sb.length() == n) {
            String str = sb.toString();
            res.add(str);
            return;
        }
        
        for (int i = 1; i <= n; ++i) {
            if (visited[i]) continue;
            sb.append(i);
            visited[i] = true;
            dfs(n, res, i + 1, sb, visited);
            sb.deleteCharAt(sb.length() - 1);
            visited[i] = false;
        }
    }
}
```

{% hint style="info" %}
太长，自己看。
{% endhint %}

I'm sure somewhere can be simplified so it'd be nice if anyone can let me know. The pattern was that:

say n = 4, you have {1, 2, 3, 4}

If you were to list out all the permutations you have

1 + (permutations of 2, 3, 4)\
\
2 + (permutations of 1, 3, 4)\
\
3 + (permutations of 1, 2, 4)\
\
4 + (permutations of 1, 2, 3)

\
We know how to calculate the number of permutations of n numbers... n! So each of those with permutations of 3 numbers means there are 6 possible permutations. Meaning there would be a total of 24 permutations in this particular one. So if you were to look for the (k = 14) 14th permutation, it would be in the

3 + (permutations of 1, 2, 4) subset.

To programmatically get that, you take k = 13 (subtract 1 because of things always starting at 0) and divide that by the 6 we got from the factorial, which would give you the index of the number you want. In the array {1, 2, 3, 4}, k/(n-1)! = 13/(4-1)! = 13/3! = 13/6 = 2. The array {1, 2, 3, 4} has a value of 3 at index 2. So the first number is a 3.

Then the problem repeats with less numbers.

The permutations of {1, 2, 4} would be:

1 + (permutations of 2, 4)\
\
2 + (permutations of 1, 4)\
\
4 + (permutations of 1, 2)

But our k is no longer the 14th, because in the previous step, we've already eliminated the 12 4-number permutations starting with 1 and 2. So you subtract 12 from k.. which gives you 1. Programmatically that would be...

k = k - (index from previous) \* (n-1)! = k - 2\*(n-1)! = 13 - 2\*(3)! = 1

In this second step, permutations of 2 numbers has only 2 possibilities, meaning each of the three permutations listed above a has two possibilities, giving a total of 6. We're looking for the first one, so that would be in the 1 + (permutations of 2, 4) subset.

Meaning: index to get number from is k / (n - 2)! = 1 / (4-2)! = 1 / 2! = 0.. from {1, 2, 4}, index 0 is 1

\
so the numbers we have so far is 3, 1... and then repeating without explanations.

\
{2, 4}\
\
k = k - (index from pervious) \* (n-2)! = k - 0 \* (n - 2)! = 1 - 0 = 1;\
\
third number's index = k / (n - 3)! = 1 / (4-3)! = 1/ 1! = 1... from {2, 4}, index 1 has 4\
\
Third number is 4

\
{2}\
\
k = k - (index from pervious) \* (n - 3)! = k - 1 \* (4 - 3)! = 1 - 1 = 0;\
\
third number's index = k / (n - 4)! = 0 / (4-4)! = 0/ 1 = 0... from {2}, index 0 has 2\
\
Fourth number is 2

\
Giving us 3142. If you manually list out the permutations using DFS method, it would be 3142. Done! It really was all about pattern finding.

```java
class Solution {
    public String getPermutation(int n, int k) {
        int pos = 0;
        List<Integer> numbers = new ArrayList<>();
        int[] factorial = new int[n + 1];
        StringBuilder sb = new StringBuilder();

        // create an array of factorial lookup
        int sum = 1;
        factorial[0] = 1;
        for(int i = 1; i <= n; i++){
            sum *= i;
            factorial[i] = sum;
        }
        // factorial[] = {1, 1, 2, 6, 24, ... n!}

        // create a list of numbers to get indices
        for(int i = 1; i <= n; i++){
            numbers.add(i);
        }
        // numbers = {1, 2, 3, 4}

        k--;

        for(int i = 1; i <= n; i++){
            int index = k/factorial[n - i];
            sb.append(String.valueOf(numbers.get(index)));
            numbers.remove(index);
            k-=index*factorial[n-i];
        }
    
        return String.valueOf(sb);
    }
}
```

```java
public class Solution {
    public String getPermutation(int n, int k) {
        StringBuilder sb = new StringBuilder();
        ArrayList<Integer> num = new ArrayList<Integer>();
        int fact = 1;
        for (int i = 1; i <= n; i++) {
            fact *= i;
            num.add(i);
        }
        for (int i = 0, l = k - 1; i < n; i++) {
            fact /= (n - i);
            int index = (l / fact);
            sb.append(num.remove(index));
            l -= index * fact;
        }
        return sb.toString();
    }
}
```
