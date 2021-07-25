老dfs了
暴力法就是维护一个cur, 更新全局的count, O(2^n)

但是有重复计算, 优化可以用memorize, 相同的index和相同的cur, 可能有不同的path, 此时dfs()return的是当前这种index和cur后面所可能实现的次数, 这可以用一个`memo[][]` 来存, 此时是O(l * n), sum这里的取值是0-2000, 因为curSum可能有正负, 所以用1000作为offset!

**重点:**
接着是DP的做法, `dp[i][j]`是index走到i时, curSum=j的情况下(还没有加减nums[i]), 能够的到target的expression数量, 
`dp[i][j] = dp[i + 1][j - nums[i]] + dp[i + 1][j + nums[i]]`
是后一位能够到j-nums[i]和j+nums[i]的可能方案数之和
然后initialize是`dp[len][target] = 1`, 必须是走过了最后一位!
<font color = grape>**这里只能从后往前, 因为answer是`dp[0][nums[0]] + dp[0][-nums[0]]`,而且这里其实用1000做offset不太好, 可以直接用sum来做, 确保不会小于0! **</font>

**神仙优化**
因为都是positive, 可以理解为一部分选为negative, 其总和为neg
所以 (sum - neg) - neg = target, neg = (sum - target) / 2
从positive arr中找出总和为neg的nums.
此时可以用非常普通的01背包, `dp[i][j]`代表前i个sum to j的种类数
然后`dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i]]`



```java
public int findTargetSumWays(int[] nums, int target) {

    int sum = 0;
    // all element >= 0, otherwise should use Math.abs(num) to get sum
    for (int num: nums) sum += num;
    if (target > sum || target < 0 - sum) return 0;

    // use 2 * sum to provide offset
    // dp[i][j + sum] represents the num of ways when go to index i and curSum = j
    int[][] dp = new int[nums.length + 1][2 * sum + 1];

    // initialize
    // when out of bound and j = target, only this one way
    dp[nums.length][target + sum] = 1;

    for (int i = nums.length - 1; i >= 0; i--) {
        int cur = nums[i];
        for (int j = -sum; j <= sum; j++) {
            // j + cur must not > sum
            if (j + cur <= sum) dp[i][j + sum] += dp[i + 1][j + cur + sum];
            // j - cur must not < -sum
            if (j - cur >= -sum) dp[i][j + sum] += dp[i + 1][j - cur + sum];
        }
    }
    return dp[0][sum];
}
```

