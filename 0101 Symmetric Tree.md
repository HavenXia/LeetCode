# 题目地址

https://leetcode.com/problems/symmetric-tree/



# 思路

#### 迭代法

重点在于queue里面可以有多个null, <font color = grape>**最初放入两个相同的root, 然后每次loop中pop两个node**</font>

+ 如果两个node都是null, 那么直接continue, pop后面两个node
+ 如果两个node都不是null且val相等,  那么把 ==四个child node全部存入queue中==
+ 反之则return false

#### 递归法

大问题是整个树的summetry,小问题是==两个node的summetry==

+ 首先两个node的value必须相等
+ 其次left1和right2必须是symmetric, right1和left2必须是symmetric

递归出口是两个node都是null, return false



# 题解

#### Iterative 迭代法 

时间复杂度O(n)

```java
public boolean isSymmetric(TreeNode root) {

    if (root == null) {
        return true;
    }
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);
    queue.offer(root);

    while (!queue.isEmpty()) {

        TreeNode cur1= queue.poll();
        TreeNode cur2 = queue.poll();
        if (cur1 == null && cur2 == null) {continue;}
        if (cur1 == null || cur2 == null) {return false;}
        if (cur1.val != cur2.val) {return false;}
        queue.offer(cur1.left);
        queue.offer(cur2.right);
        queue.offer(cur1.right);
        queue.offer(cur2.left);
    } 
    return true;
}
```



#### 分治法

时间复杂度O(n)

```java
public boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

public boolean isMirror(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return (t1.val == t2.val)
        && isMirror(t1.right, t2.left)
        && isMirror(t1.left, t2.right);
}
```



