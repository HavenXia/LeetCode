简化为在unsorted array判断是否sum to target的题目
`dp[i][j]`代表前i个存在subset that sum to target
`dp[0][0] = true` 
其实就是knapsack dp,分拿和不拿
`dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]]`
如果j写成从subset向下考虑, 就不用判断j和nums[i]的关系了

可以优化成一维dp,不过时间复杂度不变, notTake就是上一维度的自己, 所以可以直接存在1D里面,每次用之前的自己即可, 在i的维度每次只用上一次value
<font color = grape>**但是j的维度必须从target到1, 否则dp[i-1][j - cur] 已经在之前走到j - cur的时候被改变了, 然后dp[i][j]就会错误**</font>



```java
public boolean canPartition(int[] nums) {

    if (nums == null || nums.length == 0) {
        return false;
    }

    int sum = 0;
    for (int num: nums) {
        sum += num;
    }

    if (sum % 2 == 1) return false;

    // find nums sum to target in unsorted array        
    return canSum(nums, sum / 2);
}

private boolean canSum(int[] nums, int target) {

    boolean[] dp = new boolean[target + 1];

    // initialize
    // represent dp[0][i] in 2D
    // only dp[0][0] is true, otherwise false
    dp[0] = true;

    for (int i = 1; i <= nums.length; i++) {
        int cur = nums[i - 1];
        for (int j = target; j >= cur; j--) {

            //boolean notTake = dp[i - 1][j];
            //boolean take = j - cur >= 0 ? dp[j - cur] : false;
            dp[j]= dp[j - cur] || dp[j];
        }
    }

    return dp[target];
}
```

