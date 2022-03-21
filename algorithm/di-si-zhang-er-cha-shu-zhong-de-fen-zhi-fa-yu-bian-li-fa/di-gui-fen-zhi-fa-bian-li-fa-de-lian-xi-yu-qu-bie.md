# 递归，分治法，遍历法的联系与区别

**联系**

分治法（Divide & Conquer）与遍历法（Traverse）是两种常见的递归（Recursion）方法。

**分治法解决问题的思路**

先让左右子树去解决同样的问题，然后得到结果之后，再整合为整棵树的结果。

**遍历法解决问题的思路**

通过前序/中序/后序的某种遍历，游走整棵树，通过一个全局变量或者传递的参数来记录这个过程中所遇到的点和需要计算的结果。

**两种方法的区别**

从程序实现角度分治法的递归函数，通常有一个`返回值`，遍历法通常没有。