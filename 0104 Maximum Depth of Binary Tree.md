# 题目地址

https://leetcode.com/problems/maximum-depth-of-binary-tree/



# 思路

最普通的分治法DFS

每次取left和right的最大值, 然后+1 return即可

最后return的一定是root 左右两边最大depth + 1, <font color = grape>**即二叉树的depth!**</font>

需要设置全局变量



# 题解

时间复杂度O(n), 空间复杂度O(h)

```java
class Solution {

    int maxdepth = Integer.MIN_VALUE;

    public int maxDepth(TreeNode root) {

        if (root == null) {
            return 0;
        }

        int left = maxDepth(root.left);
        int right = maxDepth(root.right);

        return Math.max(left, right) + 1;
    }
}
```

