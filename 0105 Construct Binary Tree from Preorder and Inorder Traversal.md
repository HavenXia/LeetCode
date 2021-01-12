# 题目地址

https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/



## 思路

这道题是前中后序遍历很重要的题

+ 首先, preorder的第一个node必定是root, 而inorder则不一定
+ <font color = grape>**在inorder中, root左边的subarray必然是所有root 左子树的node, 而root右边的subarray必然是所有root右子树的node**</font>
+ <font color = grape>**在preorder中, 在root后, 会先遍历所有root左子树的node, 然后是所有root右子树的node**</font>, <font color = gree>并且这两个subarray的第一个node也是各自的root</font>
+ <font color = grape>**在postorder中,会先遍历所有root左子树的node, 然后是所有root右子树的node,最后root**</font>, <font color = gree>并且这两个subarray的末尾node也是各自的root</font>



首先需要一个HashMap存放<font color = gree>inorder的value和index</font>

因此,这里先从preorder取出root, 然后根据root的value找到其在inorder中的index, 因此分出左右subarray

左subarray的长度就是左子树的node数, 用 preorder中root的`index + numsLeft + 1`就可以得到preorder中root右子树第一个点的index

然后递归就可以求到左子树和右子树的root node, 把他们接在当前的root上!



**递归出口**

当只有一个node的时候, 此时 `preStart = preEnd, inStart = inEnd`, 那么计算出的numsLeft就变成0

<font color = red>next recursion的 preStart > preEnd, inStart > inEnd, 所以这个就是出口, **此时直接return null,** 所以single node的left和right都是null</font> 

==而且这两个条件必然是同时出现的!==

时间复杂度O(n), 空间复杂度O(n)



# 题解

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {

    Map<Integer, Integer> map = new HashMap<Integer, Integer>();

    for (int i = 0; i < inorder.length; i++) {
        map.put(inorder[i], i);
    }
    TreeNode root = buildTree(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1, map);
    return root;  
}

private TreeNode buildTree(int[] preorder, int preStart, int preEnd, 
                           int[] inorder, int inStart, int inEnd, Map<Integer, Integer> map) {

    if (preStart > preEnd && inStart > inEnd) {
        return null;
    }

    // get current subtree root
    int curRoot = preorder[preStart];
    // create the current root node
    TreeNode root = new TreeNode(curRoot);
    // find corresponding index in inorder
    int inRoot = map.get(curRoot);
    int numsLeft = inRoot - inStart;

    TreeNode leftNode = buildTree(preorder, preStart+1, preStart + numsLeft, inorder, inStart, inRoot-1, map);
    TreeNode rightNode = buildTree(preorder, preStart + numsLeft + 1, preEnd, inorder, inRoot+1, inEnd, map);

    root.left = leftNode;
    root.right = rightNode;

    return root;
}
```

