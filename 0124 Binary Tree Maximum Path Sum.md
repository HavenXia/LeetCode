# 题目地址

https://leetcode.com/problems/binary-tree-maximum-path-sum/

# 思路

不在于复杂而是比较难想

#### 如果必须要求经过root

那么就是普通的分治法DFS, 首先如果root 是null, 就直接return 0

按照分治法:

+ ` int left = Math,max(dfs(root. left), 0)`, <font color = grape>**这里和0比是为了保证不小于0, 因为小于0必然不会选! 即使是出现在中间过程中, 如果某个node某一侧总和还是负数,那么宁愿只选node自己也不选这一侧!**</font>  
+ ` int right = Math,max(dfs(root. right), 0)`

然后当前的 value就是 `root.val + max(left, right)` , 即最大的单边path, 然后return

<font color = grape>**到最外层, 也就是initial root, 然后这次需要两边都加上(已经排除了负数的可能性), `root.val+left+right`**</font>

#### 但是这里的要求是可以不经过root

==那么就需要在每一次recursion中都检测一下当前的root.val+left+right, 并且用一个global max随时更新!==

<font color = grape>**因为dfs会走遍所有的节点, 所以这样一定能找出最大值!**</font> 

但是return的还得是`root.val + max(left, right)` ,  因为到上一层算`root.val+left+right` 还是需要这个单边sum!



# 题解

时间复杂度O(n), 空间复杂度O(h)

```java
class Solution {

    int max = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {

        dfs(root);
        return max; 
    }

    private int dfs(TreeNode root) {

        if (root == null) {return 0;}

        int left = Math.max(dfs(root.left), 0);
        int right = Math.max(dfs(root.right), 0);

        int newSum = left + right + root.val;

        max = Math.max(max, newSum);

        return root.val + Math.max(left, right);
    }
}
```

