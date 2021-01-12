# 题目地址

https://leetcode.com/problems/validate-binary-search-tree/



# 思路

如果要valid一个BST, 那么

+ <font color = grape>**root.val 必须大于左子树maxValue, 小于右子树minValue**</font>
+ 与此同时, 左子树和右子树也必须是valid的!

所以采用分治法即可, 注意<font color = red>这里需要三个return value, boolean valid, int max和int min, 所以用 **ReturnType Class**来作为返回值</font>

对left和right的情况分四类来做, <font color = gree>最后一类即为left和right都不是null</font>

注意这里的出口: 当node是null的时候, valid当然是true, 但是<font color = grape>**max必须是Integer.MIN_VALUE, min必须是Integer.MAX_VALUE**</font>



# 题解

```java
public boolean isValidBST(TreeNode root) {

    if (root == null) {return true;}

    ReturnType result = dfs(root);
    return result.valid;
}

private ReturnType dfs(TreeNode root) {

    if (root == null) {return new ReturnType(true, Integer.MIN_VALUE, Integer.MAX_VALUE);}

    ReturnType left = dfs(root.left);
    ReturnType right = dfs(root.right);

    int max = Math.max(root.val, Math.max(left.max, right.max));
    int min = Math.min(root.val, Math.min(left.min, right.min));


    if (root.left == null && root.right == null) {
        return new ReturnType(true, root.val, root.val);
    } else if (root.left == null && root.right != null) {
        return new ReturnType(root.val < right.min && right.valid, max, min);
    } else if (root.left != null && root.right == null) {
        return new ReturnType(root.val > left.max && left.valid, max, min);
    } else {
        return new ReturnType(root.val > left.max && root.val < right.min && left.valid && right.valid, max, min);
    }

}
```

