# 题目地址

https://leetcode.com/problems/maximum-product-subarray/



# 思路

依旧是DP, 是[0053 Maximum Subarray](0053 Maximum Subarray.md) 的升级版

+ Status: <font color = grape>**`dp[i][0]` 代表以index i结尾的maxProduct, `dp[i][1]` 代表negative maxProduct**</font>
+ Initialize: `dp[0][0]` 和 `dp[0][1]` 如果nums[0] >= 0, 那么+max就是nums[0], negative max就是0, 反之亦然

+ Function: 

  + nums[0] >= 0, <font color = grape>**`dp[i][0] =Math.max(nums[i] * dp[i-1][0], nums[i]), dp[i][1] = dp[i - 1][1] * nums[i]` **</font>

    对于maxProduct, 前一个的max可能是0, 那么就从自己开始,  但是对于negative maxProduct,前一个是0那么还是0 ,前一个不是0则直接乘

  + nums[0] < 0, <font color = grape>**`dp[i][0] =nums[i] * dp[i-1][1], dp[i][1] = Math.min(dp[i - 1][0] * nums[i], nums[i])` **</font>

    对于max, 前一个的neg是0, 那么自然是0 ,前一个的neg是负数, 那么当前的max也只能是相乘

    对于neg, 前一个的max可能是0, 那么就从自己开始, 所以用一个Math.min

+ 在过程中用一个max来一直存最大值,返回max(<font color = grape>**max初始值必须为nums[0], 因为有one num array这种edge case**</font>)

时间复杂度O(n), 空间复杂度O(n)

```java
public int maxProduct(int[] nums) {

    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;

    // dp[i]: the current maxProduct of subarray end with nums[i]
    int[][] dp = new int[n][2];

    // initialize
    dp[0][0] = Math.max(nums[0], 0);
    dp[0][1] = Math.min(nums[0], 0);       
    int max = nums[0];

    // function 
    for (int i = 1; i < n; i++) {

        if (nums[i] >= 0) {
            dp[i][0] = Math.max(nums[i] * dp[i - 1][0], nums[i]);
            dp[i][1] = dp[i - 1][1] * nums[i];
        } else {
            dp[i][0] = dp[i - 1][1] * nums[i];
            dp[i][1] = Math.min(dp[i - 1][0] * nums[i], nums[i]);
        }
        max = Math.max(max, dp[i][0]);
    }
    return max;
}
```



#### 优化

可以把空间复杂度优化到O(1), 已经简化到不是dp了

用maxSoFar 和 minSoFar 代表 <font color = grape>**以当前i为end的subarray的maxProduct和minProduct**</font>

Initialize: 最开始两个都是nums[0]

对于当前的nums[i], 永远只有 <font color = grape>**maxSoFar * nums[i] , minSoFar * nums[i] , nums[i] 这三种可能! 要么连续要么从自己开始!**</font> 

然后maxSoFar必然是这三个的最大值, minSoFar必然是最小值, 重点在于

==maxSoFar和minSoFar其实都不一定要negative 或者positive, 只要max >= min, 都不会影响后续的计算==



```java
public int maxProduct(int[] nums) {

    if (nums == null || nums.length == 0) {
        return 0;
    }

    int max_so_far = nums[0];
    int min_so_far = nums[0];
    int result = max_so_far;

    for (int i = 1; i < nums.length; i++) {
        int curr = nums[i];
        int temp_max = Math.max(curr, Math.max(max_so_far * curr, min_so_far * curr));
        min_so_far = Math.min(curr, Math.min(max_so_far * curr, min_so_far * curr));

        max_so_far = temp_max;

        result = Math.max(max_so_far, result);
    }

    return result;
}
```

