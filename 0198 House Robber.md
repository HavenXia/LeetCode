# 题目地址

https://leetcode.com/problems/house-robber/



## 思路

感觉是DP

+ Status: `dp[i][0] `代表<font color = grape>**如果add nums[i] 能得到的最大值**</font>, `dp[i][1]` 代表<font color = grape>**如果不选nums[i]能得到的最大值**</font>

+ Initialize: `dp[0][0] = nums[0], dp[0][1] = 0`

+ Function:  

  + 如果抢的话, `dp[i][0] = dp[i - 1][1] + nums[i]` , <font color = grape>**自身+前一个不抢时的maxSum**</font>  

  + 如果不抢的话. `dp[i][1] = Math.max(dp[i - 1][0], dp[i - 1][1])` , <font color = grape>**不能直接等于上一个抢情况下的值!!!! 而是取两个里面较大的, 比如**</font> 

    [15 , 1, 2], 当走到2的时候, 1抢劫只能拿到1, 但是1不抢可以拿到15, <font color = grape>**所以这一步是取较大的!!!**</font>

+ Answer: return dp[n]里面较大的那个

#### 优化

对于这种只用到i-1的dp, 可以优化成O(1)的时间复杂度, 直接用一个<font color = gree>rob 代表当前抢的话能拿到的maxSum, notRob代表当前不抢的maxSum</font> 

注意: 这里的tempRob是为了防止算notRob的时候rob已经改变了.

这样时间复杂度O(n), 空间复杂度O(1)

```java
public int rob(int[] nums) {

    if (nums == null || nums.length == 0) {
        return 0;
    } 

    int rob = nums[0];
    int notRob = 0;

    for (int i = 1; i < nums.length; i++) {

        int tempRob = notRob + nums[i];

        notRob = Math.max(rob, notRob);
        rob = tempRob;
    }
    return Math.max(rob, notRob);

    }
```

