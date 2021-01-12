# 题目地址

https://leetcode.com/problems/climbing-stairs/



# 思路

应该用DP来做

+ Status: dp[i] 代表n=i的时候的 number of climbing ways 

+ Initialize: dp[0] = 1, dp[1] = 1
+ Function: <font color = grape>**dp[i] = dp[i - 1] + dp[i - 2]**</font> 

+ Answer: dp[i]

时间复杂度O(n)



# 题解

```java
public int climbStairs(int n) {
        
        if (n == 0 || n == 1) {
            return 1;
        }
        
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
```

