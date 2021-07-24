适合用dfs来做的题, 每当sum = target, count++
需要传一个isStartOfPath来判断是不是要从自己的left和right再开始两次新的dfs
但是太暴力了

Use prefix sum, 真的很聪明, 见https://leetcode.com/problems/subarray-sum-equals-k/
这里也是HashMap<prefixSum, countOfKey>
dfs过程中, 每当prefixSum - target 存在于map中, count += map.get(prefixSum - k) 说明又一段path满足sum = k了, 第一次sum到怎么办? 当然是一开始就加入{0:1}, 
接着就put 当前的sum进map, 然后dfs 左右

<font color = grape>**但是这里和之前的subarray sum不一样, 可能存在的curSum - k是在另外一条path上!! map里永远只能有从root到当前的这一条path上的curSum!!! 所以需要backtrack, 因为是dfs, 所以left和right走完, 要减去map里这个curSum!!**</font>



```java
int count = 0;

public int pathSum(TreeNode root, int targetSum) {

    if (root == null) return 0;

    // prefix hash map
    Map<Integer, Integer> prefixSumCount = new HashMap<>();
    prefixSumCount.put(0, 1);

    dfs(root, targetSum, 0, prefixSumCount);
    return count;
}

private void dfs(TreeNode root, int target,
                 int curSum, Map<Integer, Integer> prefixSumCount) {

    if (root == null) return;

    // check if path exist
    curSum += root.val;
    if (prefixSumCount.containsKey(curSum - target)) {
        count += prefixSumCount.get(curSum - target);
    } 

    // update map
    prefixSumCount.put(curSum, prefixSumCount.getOrDefault(curSum, 0) + 1);

    dfs(root.left, target, curSum, prefixSumCount);
    dfs(root.right, target, curSum, prefixSumCount);

    // backtrack
    // if only once, set it to 0 also works
    prefixSumCount.put(curSum, prefixSumCount.get(curSum) - 1); 
}
```

