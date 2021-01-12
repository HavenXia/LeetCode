# 题目地址

https://leetcode.com/problems/binary-tree-level-order-traversal/



# 思路

非常标准的single queue BFS, 用一个queue不断的poll 并且存到list中

<font color = grape>**注意由于是level order, 必须每一层先get size 然后用for loop进行遍历**</font>

==切记不能直接写成for (int i = 0; i < queue.size; i++), 因为queue的size一直在变, 比如root出来之后, queue的size反而可能变成了2!==

时间复杂度和空间复杂度都是O(n)



# 题解

```java
public List<List<Integer>> levelOrder(TreeNode root) {

    List<List<Integer>> result = new ArrayList<List<Integer>>();
    if (root == null) {return result;}

    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    List<Integer> level;
    queue.offer(root);

    while (!queue.isEmpty()) {    
        level = new ArrayList<Integer>();
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            level.add(cur.val);
            if (cur.left != null) {queue.offer(cur.left);}
            if (cur.right != null) {queue.offer(cur.right);}
        }
        result.add(new ArrayList(level));
    }
    return result;
}
```

