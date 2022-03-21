# Stack Sequence

Given an array, find all possible out stack sequence.

```java
import java.util.*;

public class OutStack {
    public static List<List<Integer>> seq(int[] nums) {
        if (nums == null || nums.length == 0) {
            return new ArrayList<>();
        }

        Stack<Integer> stack = new Stack<>();
        List<Integer> temp = new ArrayList<>();
        List<List<Integer>> res = new ArrayList<>();
        dfs(stack, temp, res, nums, 0);

        return res;
    }
    private static void dfs(Stack<Integer> stack, List<Integer> temp, List<List<Integer>> res, int[] nums, int cur) {
        if (temp.size() == nums.length) {
            res.add(new ArrayList<>(temp));
            return;
        }

        if (!stack.isEmpty()) { //pop栈顶的数字
            int t = stack.pop();
            temp.add(t);
            dfs(stack, temp, res, nums, cur);
            temp.remove(temp.size() - 1);
            stack.push(t);
        }
        if (cur != nums.length) {   //把当前的数字放进stack
            stack.push(nums[cur]);
            dfs(stack, temp, res, nums, cur + 1);
            stack.pop();
        }
    }
    public static void main(String[] args) {
        int[] test1 = new int[]{1, 2, 3};
        List<List<Integer>> res1 = seq(test1);
        System.out.println(res1);
    }
}
```
