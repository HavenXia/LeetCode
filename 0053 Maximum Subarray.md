# 题目地址

https://leetcode.com/problems/maximum-subarray/



# 思路

还是DP

+ Status: dp[i]代表==以nums[i]为末尾值的largest subarray sum==, <font color = gree>注意重点在于nums[i]必须是subarray的最后一个值</font> 

  <font color = red>**并不是一般的从0到i的largest sum subarray 哦!**</font> 

+ Initialize: dp[0] = nums[0], 到第一个位置当然就一个

+ Function: dp[i] 需要分情况讨论

  + <font color = grape>**dp[i-1] <= 0, 代表以nums[i-1]结尾的largest subarray sum小于0 ,那么nums[i]就自立门户, 从自己开始, dp[i] = nums[i]**</font>
  + <font color = grape>**dp[i-1]>0, 代表nums[i] 可以直接接在dp[i-1]的subarray后面, dp[i] = dp[i-1] + nums[i]**</font>

+ Answer: 并不是dp[n-1], 实际上需要的是整个**dp数组中的最大值,** 所以需要用一个max来实时更新

时间复杂度O(n), 空间复杂度O(n)

优化: 首先max可以跟着dp一起更新, 其次这样的话可以完全不需要dp, 直接用一个值即可, 可以实现O(1)时间复杂度, 它有另一个名字, 就是:

#### 贪心法Greedy

每个currSum就是dp[i], 而maxSum实时更新, i一共就这么多轮, 所以可以简化成一个currSum来维持

<font color = grape>**要么继续加上nums[i]要么直接从nums[i]重新开始, 时间复杂度O(n), 空间复杂度O(1)**</font>

```java
public int maxSubArray(int[] nums) {
    int n = nums.length;
    int currSum = nums[0], maxSum = nums[0];

    for(int i = 1; i < n; ++i) {
        currSum = Math.max(nums[i], currSum + nums[i]);
        maxSum = Math.max(maxSum, currSum);
    }
    return maxSum;
}
```



# 题解

DP

```java
public int maxSubArray(int[] nums) {

    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;
    int[] dp = new int[n];
    int max = nums[0];

    //initialize
    dp[0] = nums[0];
    //function
    for (int i = 1; i < n; i++) {
        dp[i] = dp[i - 1] > 0 ? dp[i - 1] + nums[i] : nums[i];
        max = Math.max(dp[i], max);
    }

    return max;
}
```

