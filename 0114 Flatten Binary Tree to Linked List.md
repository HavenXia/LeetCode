# 题目地址

https://leetcode.com/problems/flatten-binary-tree-to-linked-list/



# 思路

一道标准的DFS分治法,  但是问题在于如何找到flatten当前node之后的<font color = grape>**end node?**</font>

首先null case肯定是return null, 所以每次做flatten的时候要检测下是否 `leftend == null`, ==如果等于, 那必然是root.left就是null!!==

<font color = grape>**什么时候能做flatten? 当leftend不是null的时候, 那么第一次必然就是leftend是某个左右都为null的node!**</font>

接着要对三种最底层node做return分类:

+ 如果leftend和rightend都是null, 直接return当前node
+ 如果leftend不是null, 说明已经做了flatten, 然后看rightend是不是null
  + 如果rightend是null, 那么就直接返回leftend
  + 反之则返回rightend



# 题解

时间复杂度O(n), 空间复杂度O(h)

```java
public void flatten(TreeNode root) {

    dfs(root);            
}

private TreeNode dfs(TreeNode root) {

    if (root == null) {return null;}

    TreeNode leftend = dfs(root.left);
    TreeNode rightend = dfs(root.right);
    // connect
    if (leftend != null) {
        TreeNode temp = root.right;
        root.right = root.left;
        root.left = null;
        leftend.right = temp;
    }
    // 按顺序决定return
    if (rightend != null) {
        return rightend;
    } else if (leftend != null) {
        return leftend;
    } else {
        return root; 
    }
}
```



